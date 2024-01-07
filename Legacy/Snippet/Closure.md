---
boostnote:
  createdAt: '2020-05-18T08:38:16.197Z'
  updatedAt: '2020-05-18T08:47:16.771Z'
  type: MARKDOWN_NOTE
  folder: ac09959a9f641bf5f4a6
  title: Closure
  tags:
    - Swift
  linesHighlighted: []
  isStarred: false
  key: a66b5ce5-f530-4683-a002-32804eeb228e
  storage: d112f8ec1e85e056a09d
---

Closure
---
함수명을 선언하지 않고 함수 몸체만 만들어 사용하는 일회용 함수
함수의 파라미터로 값이나 변수가 아닌 함수를 사용하고 싶을 때, 함수명을 사용하지 않고 함수의 몸체만 이용할 때 사용
```swift
// default
func 함수명(파라미터명: 자료형) -> (반환 타입) {
    실행 구문
}

// closure
{ (파라미터명: 자료형) -> (반환 타입) in
    실행 구문
}

// closure: 반환 타입 생략
{ (파라미터명: 자료형) in
    실행 구문
}

// closure: 파라미터 자료형 생략
{ (파라미터명) in
    실행 구문
}

// closure: 파라미터 자료형이 생략된 경우 (소괄호) 생략 가능
{ 파라미터명 in
    실행 구문
}
```

```swift
// default
func completeWork(finished: Bool) -> () {
    print("complete: \(finished)")
}

// closure
{ (finished: Bool) -> () in
    print("complete: \(finished)")
}

// closure: 컴파일러가 반환 타입을 미리 알고 있다면 반환 타입 생략 가능
{ (finished: Bool) in
    print("complete: \(finished)")
}

// closure: 매개변수의 파라미터 타입 생략 가능
{ (finished) in
    print("complte: \(finished)")
}

// closure: 파라미터 타입이 생략된 경우 매개변수의 (소괄호) 생략 가능
{ finished in
    print("complete: \(finished)")
}
```
