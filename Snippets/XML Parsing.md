---
title: XML Parsing
date: 2024-12-13
time: 21:35:14
tags:
- kotlin
- android
---
https://github.com/pdvrieze/xmlutil
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
                <pray title="">Pellentesque ac vestibulum eros, et rutrum libero. Donec sagittis, est commodo ultrices accumsan, magna metus blandit nunc, sit amet pharetra magna felis quis dui. Curabitur et lorem ipsum. Curabitur eleifend erat pretium rhoncus aliquam. </pray>
            </testsix>
        </TestFour>
    </TestTwo>
</TestOne>
</Root>
```


TestDataRoot.kt
```kotlin
@Serializable
@XmlSerialName("Root")
data class TestDataRoot(
    @XmlSerialName("TestOne")
    val dailyBible: TestOne
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