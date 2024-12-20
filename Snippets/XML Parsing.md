---
title: XML Parsing
date: 2024-12-13
time: 21:35:14
tags:
- kotlin
- android
- swift
- ios
---
Test.xml
```xml
<Root>
<TestOne sdate="2025-01" edate="2025-02">
    <TestTwo date="2025-01-01" title="Lorem Ipsum" subTitle="Lorem ipsum dolor sit amet,">
        <TestThree>
            <title name="">
                <contentinfo bookcode="06" chapter="1">
                    <verse name="1">
                        <content>Maecenas urna ex, pharetra vel leo id, viverra faucibus mi. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.</content>
                    </verse>
                </contentinfo>
            </title>
            <title name="

            ">
                <contentinfo bookcode="01" chapter="1">
                    <verse name="1">
                        <content>Lorem ipsum dolor sit amet, consectetur adipiscing elit. </content>
                    </verse>
                </contentinfo>
            </title>
        </TestThree>
        <TestFour>
            <testfive>Hymn 585</testfive>
            <title name="Quisque dignissim lorem ut sem faucibus cursus.">
                <verse name="">
                    <content>Donec rhoncus ut urna quis feugiat. Quisque ornare, neque eu fringilla placerat, dui sem aliquet lacus, sit amet fermentum lectus nisl non sapien.</content>
                </verse>
            </title>
            <testsix title="test title">
                <testSeven title="">Pellentesque ac vestibulum eros, et rutrum libero. Donec sagittis, est commodo ultrices accumsan, magna metus blandit nunc, sit amet pharetra magna felis quis dui. Curabitur et lorem ipsum. Curabitur eleifend erat pretium rhoncus aliquam. </testSeven>
            </testsix>
        </TestFour>
    </TestTwo>
</TestOne>
</Root>
```

## Kotlin
### XmlUtil
https://github.com/pdvrieze/xmlutil
TestDataRoot.kt
```kotlin
@Serializable
@XmlSerialName("Root")
data class TestDataRoot(
    @XmlSerialName("TestOne")
    val testOne: TestOne
) {
    @Serializable
    data class TestOne(
        @XmlSerialName("TestTwo")
        val testTwo: List<TestTwo>,
        val sdate: String, val edate: String
    ) {
        @Serializable
        data class TestTwo(
            @XmlChildrenName("title")
            @XmlSerialName("TestThree")
            val testThree: List<TitleElement>,
            val testFour: TestFour,
            val date: String, val title: String, val subTitle: String
        )

//        @Serializable
//        data class TestThree(
//            @XmlChildrenName("title")
//            val title: List<TitleElement>
//        )

        @Serializable
        data class TitleElement(
            @XmlSerialName("contentinfo")
            val contentInfo: ContentInfo,
            val name: String
        )

        @Serializable
        data class ContentInfo(
            @XmlSerialName("verse")
            val verse: List<Verse>,
            @XmlSerialName("bookcode")
            val bookCode: String, val chapter: String
        )

        @Serializable
        data class Verse(
            @XmlElement val content: String,
            val name: String
        )

        @Serializable
        @XmlSerialName("TestFour")
        data class Explanation(
            @XmlElement val testFive: String,
            val title: TestFourTitleElement,
            @XmlSerialName("testSix")
            val testSix: TestSix
        )

        @Serializable
        @XmlSerialName("title")
        data class TestFourTitleElement(
            val name: String,
            @XmlSerialName("verse")
            val verse: List<Verse>
        )

        @Serializable
        data class TestSix(
            val title: String,
            @XmlSerialName("testseven")
            val testseven: TestSeven
        )

        @Serializable
        data class TestSeven(
            val title: String,
            @XmlValue val value: String
        )
    }
}
```

XmlParser.kt
```kotlin
class XmlParser() {  
    private val xml: XML by lazy {  
        XML {  
            xmlVersion = XmlVersion.XML10  
            xmlDeclMode = XmlDeclMode.Auto  
            indentString = "  "  
            repairNamespaces = true  
        }  
    }  
    fun decoding(context: Context): TestDataRoot{  
        val fileName = "TestData.xml"  // assets folder의 파일 이름
        val xmlString = context.assets.open(fileName).bufferedReader().use {  
            it.readText()
        }  
  
        return xml.decodeFromString(TestDataRoot.serializer(), xmlString)  
    }
}
```

MainActivity.kt
```kotlin
val root = XmlParser().decoding(context = LocalContext.current)
```

## Swift
### SWXMLHash
https://github.com/drmohundro/SWXMLHash
TestDataRoot.swift
```swift
struct TestOne: XMLObjectDeserialization {
    let testTwo: [TestTwo]
    let sdate, edate: String
    
    static func deserialize(_ node: XMLIndexer) throws -> TestOne {
        return try TestOne(testTwo: node["TestTwo"].value(), sdate: node.value(ofAttribute: "sdate"), edate: node.value(ofAttribute: "edate"))
    }
    
    struct TestTwo: XMLObjectDeserialization {
        let testThree: TestThree
        let testFour: TestFour
        let date, title, subTitle: String
        
        static func deserialize(_ node: XMLIndexer) throws -> TestTwo {
            return try TestTwo(testThree: node["TestThree"].value(), testFour: node["TestFour"].value(), date: node.value(ofAttribute: "date"), title: node.value(ofAttribute: "title"), subTitle: node.value(ofAttribute: "subTitle"))
        }
    }

    struct TestThree: XMLObjectDeserialization {
        let title: [TitleElement]
        
        static func deserialize(_ node: XMLIndexer) throws -> TestThree {
            return try TestThree(title: node["title"].value())
        }
    }

    struct TitleElement: XMLObjectDeserialization {
        let contentinfo: Contentinfo
        let name: String
        
        static func deserialize(_ node: XMLIndexer) throws -> TitleElement {
            return try TitleElement(contentinfo: node["contentinfo"].value(), name: node.value(ofAttribute: "name"))
        }
    }

    struct Contentinfo: XMLObjectDeserialization {
        let verse: [Verse]
        let bookcode, chapter: String
        
        static func deserialize(_ node: XMLIndexer) throws -> Contentinfo {
            return try Contentinfo(verse: node["verse"].value(), bookcode: node.value(ofAttribute: "bookcode"), chapter: node.value(ofAttribute: "chapter"))
        }
    }

    struct Verse: XMLObjectDeserialization {
        let content, name: String
        
        static func deserialize(_ node: XMLIndexer) throws -> Verse {
            return try Verse(content: node["content"].value(), name: node.value(ofAttribute: "name"))
        }
    }

    struct Testfour: XMLObjectDeserialization {
        let testFive: String
        let title: [TestFive]
        let testSix: TestSix
        
        static func deserialize(_ node: XMLIndexer) throws -> Testfour {
            return try Testfour(testFive: node["testFive"].value(), title: node["title"].value(), testSix: node["testSix"].value())
        }
    }

    struct TestSix: XMLObjectDeserialization {
        let testSeven: [TestSeven]
        let title: String
        
        static func deserialize(_ node: XMLIndexer) throws -> Prays {
            return try TestSix(testSeven: node["testSeven"].value(), title: node.value(ofAttribute: "title"))
        }
    }

    struct TestSeven: XMLObjectDeserialization {
        let title, text: String
        
        static func deserialize(_ node: XMLIndexer) throws -> TestSeven {
            return try TestSeven(title: node.value(ofAttribute: "title"), text: node.value())
        }
    }

    struct TestFive: XMLObjectDeserialization {
        let verse: [Verse]
        let name: String
        
        static func deserialize(_ node: XMLIndexer) throws -> ExplainationTitle {
            return try ExplainationTitle(verse: node["verse"].value(), name: node.value(ofAttribute: "name"))
        }
    }
}
```

XmlParser.swift
```swift
class XmlParser {
    func readXml() {
        let fileManager = FileManager.default
        let path = Bundle.main.resourcePath!

		if let path = fileManager.contents(atPath: path + "/" + "Testdata.xml") { // resource folder의 파일 이름
			parseXML(xmlData: path)
		}
    }
    
    private func parseXML(xmlData: Data) {
        var xml = XMLHash.config { config in
//            config.caseInsensitive = false
        }.parse(xmlData)
        
        do {
            let test: TestOne = try xml["Root"].value()
            print(test)
        } catch {
            print("error: \(error)")
        }
    }
}
```

### XMLCoder
https://github.com/CoreOffice/XMLCoder
TestDataRoot.swift
```swift
@Observable class TestDataRoot: Codable {
	var testOne: TestOne = TestOne()

	enum CodingKeys: String, CodingKey {
		case _testOne = "TestOne"
	}
	
	@Observable class TestOne: Codable {
	    var testTwo: [TestTwo] = []
	    var sdate: String = ""
	    var edate: String = ""
	    
		enum CodingKeys: String, CodingKey {
			case _testTwo = "testTwo"
			case _sdate = "sdate"
			case _edate = "edate"
		}
	    
	    @Observable class TestTwo: Codable {
	        var testThree: TestThree = TestThree()
	        var testFour: TestFour = TestFour()
	        var date: String = ""
	        var title: String = ""
	        var subTitle: String = ""
	        
	        enum CodingKeys: String, CodingKey {
		        case _testThree = "TestThree"
		        case _testFour = "TestFour"
				case _date = "date"
	            case _title = "title"
	            case _subTitle = "subTitle"
	        }
	    }
	
	    @Observable class TestThree: Codable {
	        let title: [TitleElement]
	        
	        enum CodingKeys: String, CodingKey {
	            case _title = "title"
	        }
	    }
	
	    struct TitleElement: Codable {
	        var contentinfo: ContentInfo = ContentInfo()
	        var name: String = ""
	        
	        enum CodingKeys: String, CodingKey {
		        case _contentInfo = "contentinfo"
		        case _name = "name"
	        }
	    }
	
	    struct Contentinfo: Codable {
	        var verse: [Verse] = []
	        var bookcode: String = ""
	        var chapter: String = ""
	        
	        enum CodingKeys: String, CodingKey {
	            case _verse = "verse"
	            case _bookcode = "bookcode"
	            case _chapter = "chapter"
	        }
	    }
	
		@Observable class Verse: Codable {
	        var content: String = ""
	        var name: String = ""
	        
	        enum CodingKeys: String, CodingKey {
	            case _content = "content"
	            case _name = "name"
	        }
	    }
	
	    @Observable class Testfour: Codable {
	        var testFive: String = ""
	        var title: [TestFive] = []
	        var testSix: TestSix = TestSix()
	        
	        enum CodingKeys: String, CodingKey {
	            case _testFive = "testFive"
	            case _title = "title"
	            case _testSix = "testSix"
	        }
	    }
	
	    @Observable class TestSix: Codable {
	        var testSeven: [TestSeven] = []
	        var title: String = ""
	        
	        enum CodingKeys: String, CodingKey {
	            case _testSeven = "testSeven"
	            case _title = "title"
			}
	    }
	
	    @Observable class TestSeven: Codable {
	        var title: String = ""
	        var text: String = ""
	        
	        enum CodingKeys: String, CodingKey {
	            case _title = "title"
	            case _text = ""
			}
	    }
	
	    @Observable class TestFive: Codable {
	        var verse: [Verse] = []
	        var name: String = ""
	        
	        enum CodingKeys: String, CodingKey {
	            case _verse = "verse"
	            case _name = "name"
			}
	    }
	}
}
```

XmlParser.swift
```swift
class XmlParser {
    func readXml() {
        let fileManager = FileManager.default
        let path = Bundle.main.resourcePath!

		if let path = fileManager.contents(atPath: path + "/" + "Testdata.xml") { // resource folder의 파일 이름
			parseXML(xmlData: path)
		}
    }
    
    private func parseXML(xmlData: Data) {
		let test2 = try XMLDecoder().decode(TestDataRoot.self, from: xmlData)
        print(test2.testOne.sdate)
    }
}
```