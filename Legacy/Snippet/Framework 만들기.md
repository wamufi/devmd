---
boostnote:
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Framework 만들기
  tags:
    - Framework
    - Objective-C
  isStarred: false
  createdAt: '2017-08-29T09:10:40.897Z'
  updatedAt: '2019-03-06T03:10:01.238Z'
  linesHighlighted: []
  key: 6592eab53bce4ae98b8b
  storage: d112f8ec1e85e056a09d
---

Framework 만들기
---
1. Project 생성
2. Cocoa Touch Framework 템플릿 선택
3. Person 이름의 Cocoa Touch Class 생성
```objc
Person.h

#import <Foundation/Foundation.h>

@interface Person : NSObject

- (instancetype)initWithName:(NSString *)name age:(int)age;
- (void)info;

@end
```
```objc
Person.m

#import "Person.h"

@interface Person()

@property(strong, nonatomic) NSString *name;
@property(nonatomic) int age;

@end

@implementation Person

- (instancetype)initWithName:(NSString *)name age:(int)age
{
    if(self = [super init]) {
        self.name = name;
        self.age = age;
    }
    return self;
}

- (void)info
{
    NSLog(@"The person's name is %@ and his age is %d", self.name, self.age);
}

@end
```
4. Person.h가 외부 프로젝트에서 접근이 가능하게 하기 위해 Public으로 변경한다.
    - Person.h의 File Inspector 선택
    - Target Membership -> Framework의 오른편 Visibility -> Public으로 변경
5. Framework 헤더 파일에 import 선언
`#import <Framework/Person.h>`
6. 빌드

Framework 추가하기
---
1. Project 생성
2. framework 파일을 **폴더 생성 후 폴더 내에** 복사
3. Add File에서 폴더를 References로 추가
4. Target 설정
    - Linked Frameworks and Libraries에서 제거
    - Embedded Binaries에서 추가
5. Project의 ViewController에 추가
```objc
#import <Framework/Framework.h>
// Framework의 Person 클래스를 사용
Person *person = [[Person alloc] initWithName:@"Hello" age:19];
[person info];
```

Error
---
### App installation failed. No code signature found.
1. 프로젝트와 프레임워크의 Signing에서 Automatically manage signing을 체크 해제
2. Team과 Signing Certificate를 동일하게 맞춤. ex) Signing Certificate는 iOS Developer로.
*framework는 Provisioning Profile을 지원하지 않음*

Reference
---
[XCode 6 Framework 만들기](http://theeye.pe.kr/archives/2254)
[iOS framework 만들기](http://life-shelter.tistory.com/122)
