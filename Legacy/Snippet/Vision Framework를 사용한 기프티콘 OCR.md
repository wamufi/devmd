---
title: "Vision Framework를 사용한 기프티콘 OCR"
date: 2023-05-03
time: 16:34:00
tags:
  - ios
  - swift
  - visionframework
id: 20230503163400
---
ViewController.swift
```swift
//
//  ViewController.swift
//  OCR
//
//  Created by tmp on 2023/04/27.
//

import UIKit
import Vision

class ViewController: UIViewController {
    
    private let imageView: UIImageView = {
        let imageView = UIImageView()
        imageView.contentMode = .scaleAspectFit
        
        return imageView
    }()
    
    private let label: UILabel = {
        let label = UILabel()
        label.numberOfLines = 0
        
        return label
    }()
    
    private let indicatorView = UIActivityIndicatorView()
    
œ    var pathLayer: CALayer?
    
    var imageWidth: CGFloat = 0
    var imageHeight: CGFloat = 0
    
    private var barcodeBoundingBox = CGRect.zero
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        
        view.addSubview(imageView)
        view.addSubview(label)
        view.addSubview(indicatorView)
        
        let tap = UITapGestureRecognizer(target: self, action: #selector(promptPhoto))
        self.view.addGestureRecognizer(tap)
    }
    
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        
        if imageView.image == nil {
            promptPhoto()
        }
    }
    
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        
        imageView.frame = CGRect(x: 0, y: view.safeAreaInsets.top, width: view.frame.size.width, height: view.frame.size.height / 1.5)
        label.frame = CGRect(x: 8, y: view.frame.size.height / 1.5 + 8, width: view.frame.size.width - 16, height: view.frame.size.height / 3)
        
        indicatorView.center = view.center
        indicatorView.style = .large
        indicatorView.hidesWhenStopped = true
    }
    
    @objc private func promptPhoto() {
        let imagePicker = UIImagePickerController()
        imagePicker.delegate = self
        
        func presentLibrary(_: UIAlertAction) {
            imagePicker.sourceType = .photoLibrary
            self.present(imagePicker, animated: true)
        }

        let libraryAction = UIAlertAction(title: "사진 선택", style: .default, handler: presentLibrary(_:))
        let cancelAction = UIAlertAction(title: "취소", style: .cancel)
        
        let prompt = UIAlertController(title: nil, message: nil, preferredStyle: .actionSheet)
        prompt.addAction(libraryAction)
        prompt.addAction(cancelAction)
        
        present(prompt, animated: true)
    }
    
    private func show(_ image: UIImage) {
        // Remove previous paths & image
        pathLayer?.removeFromSuperlayer()
        pathLayer = nil
        imageView.image = nil
        
        // Account for image orientation by transforming view.
        let correctedImage = image.scaleAndOrient()
        
        // Place photo inside imageView.
        imageView.image = correctedImage
        
        // Transform image to fit screen.
        guard let cgImage = correctedImage.cgImage else {
            print("Trying to show an image not backed by CGImage!")
            return
        }
        
        let fullImageWidth = CGFloat(cgImage.width)
        let fullImageHeight = CGFloat(cgImage.height)
        
        let imageFrame = imageView.frame
        let widthRatio = fullImageWidth / imageFrame.width
        let heightRatio = fullImageHeight / imageFrame.height
        
        // ScaleAspectFit: The image will be scaled down according to the stricter dimension.
        let scaleDownRatio = max(widthRatio, heightRatio)
        
        // Cache image dimensions to reference when drawing CALayer paths.
        imageWidth = fullImageWidth / scaleDownRatio
        imageHeight = fullImageHeight / scaleDownRatio
        
        // Prepare pathLayer to hold Vision results.
        let xLayer = (imageFrame.width - imageWidth) / 2
        let yLayer = imageView.frame.minY + (imageFrame.height - imageHeight) / 2
        let drawingLayer = CALayer()
        drawingLayer.bounds = CGRect(x: xLayer, y: yLayer, width: imageWidth, height: imageHeight)
        drawingLayer.anchorPoint = CGPoint.zero
        drawingLayer.position = CGPoint(x: xLayer, y: yLayer)
        drawingLayer.opacity = 0.5
        pathLayer = drawingLayer
        self.view.layer.addSublayer(pathLayer!)
    }
    
    // MARK: - Vision
    private func performVisionRequest(cgImage: CGImage) {
        let requests = [barcodeDetectionRequest, textRecognitionRequest]
        let requestHandler = VNImageRequestHandler(cgImage: cgImage)
        self.indicatorView.startAnimating()

        DispatchQueue.global(qos: .userInitiated).async {
            do {
                try requestHandler.perform(requests)
            } catch {
                print("error: \(error)")
                self.indicatorView.stopAnimating()
            }
        }
    }
    
    lazy var barcodeDetectionRequest: VNDetectBarcodesRequest = {
        let request = VNDetectBarcodesRequest(completionHandler: handleDetectedBarcodes)
        request.symbologies = [.code128]
        request.regionOfInterest = CGRect(x: 0, y: 0, width: 1, height: 1)
        #if targetEnvironment(simulator) && compiler(>=5.7)
        if #available(iOS 16, *) {
            request.revision = VNDetectBarcodesRequestRevision1 // https://stackoverflow.com/questions/73770477/vndetectbarcodesrequest-not-working-on-ios16
        }
        #endif
        
        return request
    }()
    
    lazy var textRecognitionRequest: VNRecognizeTextRequest = {
        let request = VNRecognizeTextRequest(completionHandler: handleRecognizedText)
        request.revision = VNRecognizeTextRequestRevision3 // iOS 16.0+
        request.recognitionLanguages = ["ko-KR"] // iOS 16.0+
        request.usesLanguageCorrection = true
        request.recognitionLevel = .accurate
        
        return request
    }()
    
    private func handleDetectedBarcodes(request: VNRequest, error: Error?) {
        if let error = error as NSError? {
            print("Barcode Detection Error: \(error)")
            return
        }
        
        guard let observations = request.results as? [VNBarcodeObservation] else { return }
        
        if observations.count > 0 {
            print("barcode: \(observations.map { $0.boundingBox })")
            
            // observations가 보통 2개 이므로 첫번째 바코드 영역 (보통 위) 가져오기
            // revision3일 때 확인 필요
            barcodeBoundingBox = observations[0].boundingBox
            
            DispatchQueue.main.async {
                guard let drawLayer = self.pathLayer else { return }
                self.draw(barcodes: observations, onImageWithBounds: drawLayer.bounds)
                drawLayer.setNeedsDisplay()
            }
        }
    }
    
    private func handleRecognizedText(request: VNRequest, error: Error?) {
        DispatchQueue.main.async {
            self.indicatorView.stopAnimating()
        }
        
        if let error = error as NSError? {
            print("Text Recognition Error: \(error)")
            return
        }
        
        guard let observations = request.results as? [VNRecognizedTextObservation] else { return }
        
        var texts: [String] = []
        
        var brandName = "" // 교환처
        var productName = "" // 상품명
        var expireDate = "" // 유효기간
        
        var brandMidY: Double = 0.0
        var expireDateMidY: Double = 0.0
        
        for observation in observations {
            let candidates = observation.topCandidates(1).first
            
            guard let string = candidates?.string else { continue }
            print("string: \(string)")
            texts.append(string)

            // '기간, 기한' 등의 단어가 있을 경우 그 단어의 midY 값 저장
            let dateStrings = ["기간", "기한"]
            if dateStrings.contains(where: { string.contains($0) }) {
                expireDateMidY = observation.boundingBox.midY
                continue
            }
            
            // '교환처, 사용처' 등의 단어가 있을 경우 그 단어의 midY값 저장
            let brandStrings = ["교환처", "사용처"]
            if brandStrings.contains(where: { string.contains($0) }) {
                brandMidY = observation.boundingBox.midY
                continue
            }
            
            // 저장한 '기간'의 midY 값이 있을 경우 좌표값(midY)을 비교하여 날짜를 가져오기
            if expireDateMidY > 0 {
                let midY: Double = observation.boundingBox.midY
                if fabs(midY - expireDateMidY) < 0.01 { // 좌표값 비교
                    print("expireMidDateY > 0 -- string: \(string)")
                    if let date = string.getDate() { // 비교한 텍스트가 날짜 형식일 경우
                        expireDate = String(date)
                        continue
                    }
                }
            }
            
            // 저장한 '교환처'의 midY 값이 있을 경우 좌표값(midY)을 비교하여 교환처 가져오기
            if brandMidY > 0 {
                let midY: Double = observation.boundingBox.midY
                print("brandMidY > 0 -- string: \(string)")
                if fabs(midY - brandMidY) < 0.01 { // 좌표값 비교
                    brandName = string
                    continue
                }
            }
        }
        
        // '기간' 단어가 없을 경우 임의의 날짜 형식 텍스트 가져오기
        if expireDate.isEmpty {
            let dates = texts.filter { $0.getDate() != nil }
            if dates.count > 0 {
                expireDate = dates[0]
                print("expireDate.isEmpty -- expireDate: \(expireDate)")
            }
        }
        
        var productObservation: VNRecognizedTextObservation? = nil

        if !barcodeBoundingBox.isEmpty { // 바코드 인식했을 경우
//            let result = observations.last(where: { $0.boundingBox.maxY >= barcodeBoundingBox.minY }) // 바로 이전 값
//            let result = observations.enumerated().min(by: { abs($0.element.boundingBox.minY - barcodeBoundingBox.minY) < abs($1.element.boundingBox.minY - barcodeBoundingBox.minY) })?.element // 가장 가까운 값
//            let result = observations.last(where: { $0.boundingBox.maxY >= barcodeBoundingBox.minY && $0.boundingBox.minX < 0.5 }) // 바로 이전 값 && x값이 절반을 넘지 않는 값
            
            let result = observations.filter { $0.topCandidates(1).first?.string.hasContainKorean() ?? false && $0.boundingBox.maxY >= barcodeBoundingBox.minY && $0.boundingBox.minX < 0.5 && $0.boundingBox.width > 0.3 } // 한국어 포함 여부 && 바코드 위의 값 && x값이 절반을 넘지 않는 값 && 너비 0.3 이상
                .max { $0.boundingBox.height < $1.boundingBox.height } // 높이가 가장 큰 값
            productName = result?.topCandidates(1).first?.string ?? ""
            
            productObservation = result
        } else { // 바코드 인식 못했을 경우
            let result = observations.filter { $0.topCandidates(1).first?.string.hasContainKorean() ?? false && $0.boundingBox.minX < 0.5 && $0.boundingBox.width > 0.3 && $0.boundingBox.minY > 0.1 } // 한국어 포함 여부 && x값이 절반을 넘지 않는 값 && 너비 0.3 이상 && y값 하단 0.1 이상
                .max { $0.boundingBox.height < $1.boundingBox.height } // 높이가 가장 큰 값
            productName = result?.topCandidates(1).first?.string ?? ""
            
            productObservation = result
        }
        
        // '교환처' 단어가 없지만 상품명이 있을 경우 상품명 바로 이전 값 가져오기
        if brandName.isEmpty && productObservation != nil {
            let result = observations.last(where: { $0.boundingBox.minY > productObservation!.boundingBox.minY })
            brandName = result?.topCandidates(1).first?.string ?? ""
        }
        
        DispatchQueue.main.async {
            self.label.text = "교환처: \(brandName)\n상품명: \(productName)\n유효기간: \(expireDate)"
        }
    }
    
    // MARK: - Draw
    fileprivate func boundingBox(forRegionOfInterest: CGRect, withinImageBounds bounds: CGRect) -> CGRect {
        
        let imageWidth = bounds.width
        let imageHeight = bounds.height
        
        // Begin with input rect.
        var rect = forRegionOfInterest
        
        // Reposition origin.
        rect.origin.x *= imageWidth
        rect.origin.x += bounds.origin.x
        rect.origin.y = (1 - rect.origin.y) * imageHeight + bounds.origin.y
        
        // Rescale normalized coordinates.
        rect.size.width *= imageWidth
        rect.size.height *= imageHeight
        
        return rect
    }
    
    fileprivate func shapeLayer(color: UIColor, frame: CGRect) -> CAShapeLayer {
        let layer = CAShapeLayer()
        layer.fillColor = nil
        layer.shadowOpacity = 0
        layer.shadowRadius = 0
        layer.borderWidth = 2
        layer.borderColor = color.cgColor
        layer.anchorPoint = .zero
        layer.frame = frame
        layer.masksToBounds = true
        layer.transform = CATransform3DMakeScale(1, -1, 1)
        
        return layer
    }
    
    // 바코드 영역 그리기
    fileprivate func draw(barcodes: [VNBarcodeObservation], onImageWithBounds bounds: CGRect) {
        CATransaction.begin()
        
        for barcode in barcodes {
            let barcodeBox = boundingBox(forRegionOfInterest: barcode.boundingBox, withinImageBounds: bounds)
            let barcodeLayer = shapeLayer(color: .purple, frame: barcodeBox)
            
            pathLayer?.addSublayer(barcodeLayer)
        }
        
        CATransaction.commit()
    }
}

// MARK: - UIImagePickerControllerDelegate, UINavigationControllerDelegate
extension ViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate {
    func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
        dismiss(animated: true)
    }
    
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        let originalImage = info[UIImagePickerController.InfoKey.originalImage] as! UIImage
        imageView.image = originalImage
        show(originalImage)
        
        guard let cgImage = originalImage.cgImage else { return }
        performVisionRequest(cgImage: cgImage)
        
        label.text = ""
        
        dismiss(animated: true)
    }
}

```

Extensions.swift
```swift
//
//  Extensions.swift
//  OCR
//
//  Created by tmp on 2023/04/27.
//

import UIKit

extension String {
    func getBarcodeNumber() -> Regex<Substring>.RegexOutput? {
        let regex = /[0-9 ]{12,}/
        let wholeMatch = self.wholeMatch(of: regex)?.output
                
        return wholeMatch
    }
    
    func getDate() -> Regex<Substring>.RegexOutput? {
        let regex = /\d{2,4}년?[-|.|\s|\/]\d{1,2}월?[-|.|\s|\/]\d{1,2}일?/
//        let wholeMatch = self.wholeMatch(of: regex)?.output
        let match = self.matches(of: regex).first?.output
        
        return match
    }
    
    func hasContainKorean() -> Bool {
        let regex = /[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]/
        if self.matches(of: regex).first?.output != nil {
            return true
        } else {
            return false
        }
    }
    
    func contains(_ strings: [String]) -> Bool {
        strings.contains { contains($0) }
    }
    
}

extension UIImageView {
    var imageSizeAfterAspectFit: CGSize {
        var newWidth: CGFloat
        var newHeight: CGFloat

        guard let image = image else { return frame.size }

        if image.size.height >= image.size.width {
            newHeight = frame.size.height
            newWidth = ((image.size.width / (image.size.height)) * newHeight)

            if CGFloat(newWidth) > (frame.size.width) {
                let diff = (frame.size.width) - newWidth
                newHeight = newHeight + CGFloat(diff) / newHeight * newHeight
                newWidth = frame.size.width
            }
        } else {
            newWidth = frame.size.width
            newHeight = (image.size.height / image.size.width) * newWidth

            if newHeight > frame.size.height {
                let diff = Float((frame.size.height) - newHeight)
                newWidth = newWidth + CGFloat(diff) / newWidth * newWidth
                newHeight = frame.size.height
            }
        }
        return .init(width: newWidth, height: newHeight)
    }
}

extension UIImage {
    func scaleAndOrient() -> UIImage {
        
        // Set a default value for limiting image size.
        let maxResolution: CGFloat = 640
        
        guard let cgImage = self.cgImage else {
            print("UIImage has no CGImage backing it!")
            return self
        }
        
        // Compute parameters for transform.
        let width = CGFloat(cgImage.width)
        let height = CGFloat(cgImage.height)
        var transform = CGAffineTransform.identity
        
        var bounds = CGRect(x: 0, y: 0, width: width, height: height)
        
        if width > maxResolution ||
            height > maxResolution {
            let ratio = width / height
            if width > height {
                bounds.size.width = maxResolution
                bounds.size.height = round(maxResolution / ratio)
            } else {
                bounds.size.width = round(maxResolution * ratio)
                bounds.size.height = maxResolution
            }
        }
        
        let scaleRatio = bounds.size.width / width
        let orientation = self.imageOrientation
        switch orientation {
        case .up:
            transform = .identity
        case .down:
            transform = CGAffineTransform(translationX: width, y: height).rotated(by: .pi)
        case .left:
            let boundsHeight = bounds.size.height
            bounds.size.height = bounds.size.width
            bounds.size.width = boundsHeight
            transform = CGAffineTransform(translationX: 0, y: width).rotated(by: 3.0 * .pi / 2.0)
        case .right:
            let boundsHeight = bounds.size.height
            bounds.size.height = bounds.size.width
            bounds.size.width = boundsHeight
            transform = CGAffineTransform(translationX: height, y: 0).rotated(by: .pi / 2.0)
        case .upMirrored:
            transform = CGAffineTransform(translationX: width, y: 0).scaledBy(x: -1, y: 1)
        case .downMirrored:
            transform = CGAffineTransform(translationX: 0, y: height).scaledBy(x: 1, y: -1)
        case .leftMirrored:
            let boundsHeight = bounds.size.height
            bounds.size.height = bounds.size.width
            bounds.size.width = boundsHeight
            transform = CGAffineTransform(translationX: height, y: width).scaledBy(x: -1, y: 1).rotated(by: 3.0 * .pi / 2.0)
        case .rightMirrored:
            let boundsHeight = bounds.size.height
            bounds.size.height = bounds.size.width
            bounds.size.width = boundsHeight
            transform = CGAffineTransform(scaleX: -1, y: 1).rotated(by: .pi / 2.0)
        default:
            transform = .identity
        }
        
        return UIGraphicsImageRenderer(size: bounds.size).image { rendererContext in
            let context = rendererContext.cgContext
            
            if orientation == .right || orientation == .left {
                context.scaleBy(x: -scaleRatio, y: scaleRatio)
                context.translateBy(x: -height, y: 0)
            } else {
                context.scaleBy(x: scaleRatio, y: -scaleRatio)
                context.translateBy(x: 0, y: -height)
            }
            context.concatenate(transform)
            context.draw(cgImage, in: CGRect(x: 0, y: 0, width: width, height: height))
        }
    }
}

extension Double {
    static func equal(_ lhs: Double, _ rhs: Double, precise value: Int? = nil) -> Bool {
        guard let value = value else {
            return lhs == rhs
        }
        
        return lhs.precised(value) == rhs.precised(value)
    }

    func precised(_ value: Int = 1) -> Double {
        let offset = pow(10, Double(value))
        return (self * offset).rounded() / offset
    }
    
    static func compare(_ lhs: Double, _ rhs: Double, absTolerance: Double = 1.0e-8, relTolerance: Double = ulpOfOne) -> Bool {
        let diff = lhs - rhs
        if (fabs(diff) <= absTolerance) {
            return true
        }
        
        let largest = max(fabs(lhs), fabs(rhs))
        if (fabs(diff) <= largest * relTolerance) {
            return true
        }
        
        return false
    }
}

```