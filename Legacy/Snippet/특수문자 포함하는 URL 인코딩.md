---
boostnote:
  createdAt: '2019-02-08T10:14:16.283Z'
  updatedAt: '2019-06-11T08:41:18.437Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: 특수문자 포함하는 URL 인코딩
  tags:
    - Android
    - Java
    - Objective-C
    - iOS
  linesHighlighted: []
  isStarred: false
  key: d68c6a11-a73a-4a16-8b79-ac079df80f2e
  storage: d112f8ec1e85e056a09d
---

특수문자 포함하는 URL 인코딩

Android
---
```java
String test = "test";

URLEncoder.encode(test, "UTF-8");
```

iOS
---
```objc
NSString *test = @"test";

- (NSString*)dataEncodeing:(NSString*)str {
    NSString *encodeStr = [str stringByAddingPercentEncodingWithAllowedCharacters:[NSCharacterSet alphanumericCharacterSet]];
    return encodeStr;
}
```