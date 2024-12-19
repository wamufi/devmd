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
  
        return xml.decodeFromString(DailyBibleRoot.serializer(), xmlString)  
    }
}
```

MainActivity.kt
```kotlin
val root = XmlParser().decoding(context = LocalContext.current)
```

## Swift
https://github.com/drmohundro/SWXMLHash
TestDataRoot.swift
```swift
struct TestOne: XMLObjectDeserialization {
    let testTwo: [TestTwo]
    let sdate, edate: String
    
    static func deserialize(_ node: XMLIndexer) throws -> TestOne {
        return try TestOne(bible: node["TestTwo"].value(), sdate: node.value(ofAttribute: "sdate"), edate: node.value(ofAttribute: "edate"))
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
        let title: [ExplainationTitle]
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

    struct ExplainationTitle: XMLObjectDeserialization {
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
class DailyBibleParser {
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