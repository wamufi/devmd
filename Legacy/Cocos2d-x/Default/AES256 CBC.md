---
boostnote:
  createdAt: '2022-01-13T06:54:42.391Z'
  updatedAt: '2022-01-13T09:04:21.727Z'
  type: MARKDOWN_NOTE
  folder: f9b4c2c416b7c9c01097
  title: AES256 CBC
  tags:
    - Swift
    - iOS
    - AES
  linesHighlighted: []
  isStarred: false
  key: 05280bee-d9b5-43e4-9be5-69ed718c0032
  storage: d112f8ec1e85e056a09d
---

AES256 CBC
---
[ios - AES encryption in swift - Stack Overflow](https://stackoverflow.com/questions/37680361/aes-encryption-in-swift)

```swift
import Foundation
import CommonCrypto

struct AES {
    // MARK: - Value
    // MARK: Private
    private let key: Data
    private let iv: Data


    // MARK: - Initialzier
    init?(key: String, iv: String) {
        guard key.count == kCCKeySizeAES128 || key.count == kCCKeySizeAES256, let keyData = key.data(using: .utf8) else {
            debugPrint("Error: Failed to set a key.")
            return nil
        }

        guard iv.count == kCCBlockSizeAES128, let ivData = iv.data(using: .utf8) else {
            debugPrint("Error: Failed to set an initial vector.")
            return nil
        }


        self.key = keyData
        self.iv  = ivData
    }

    // MARK: - Function
    // MARK: Public
    func encrypt(string: String) -> Data? {
        return crypt(data: string.data(using: .utf8), option: CCOperation(kCCEncrypt))
    }

    func decrypt(data: Data?) -> String? {
        guard let decryptedData = crypt(data: data, option: CCOperation(kCCDecrypt)) else { return nil }
        return String(bytes: decryptedData, encoding: .utf8)
    }

    func crypt(data: Data?, option: CCOperation) -> Data? {
        guard let data = data else { return nil }

        let cryptLength = data.count + kCCBlockSizeAES128
        var cryptData   = Data(count: cryptLength)

        let keyLength = key.count
        let options   = CCOptions(kCCOptionPKCS7Padding)

        var bytesLength = Int(0)

        let status = cryptData.withUnsafeMutableBytes { cryptBytes in
            data.withUnsafeBytes { dataBytes in
                iv.withUnsafeBytes { ivBytes in
                    key.withUnsafeBytes { keyBytes in
                    CCCrypt(option, CCAlgorithm(kCCAlgorithmAES128), options, keyBytes.baseAddress, keyLength, ivBytes.baseAddress, dataBytes.baseAddress, data.count, cryptBytes.baseAddress, cryptLength, &bytesLength)
                    }
                }
            }
        }

        guard UInt32(status) == UInt32(kCCSuccess) else {
            debugPrint("Error: Failed to crypt data. Status \(status)")
            return nil
        }

        cryptData.removeSubrange(bytesLength..<cryptData.count)
        return cryptData
    }
}
```

```swift
// 복호화
func getLottoDate(_ indexPath: Int) -> String {
    var returnString = ""

    if let aes = AES() {
        let encodedString = lottoList?[indexPath].date ?? "" // (서버에서 보낸) 암호화된 String        
        let encodedData = Data(base64Encoded: encodedString) // String -> Data
        returnString = aes.decrypt(data: encodedData) ?? "" // Data 복호화
    }

    return returnString
}

// 암호화
func encrypt(_ string: String) -> String {
    var returnString = ""
    
    if let aes = AES() {
        let encryptedData = aes.encrypt(string: "asdf")
        returnString = encryptedData?.base64EncodedString()
    }
    
    return returnString
}
```