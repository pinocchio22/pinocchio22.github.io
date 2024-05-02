---
title: "[iOS] Swift의 Type"
layout: single
date: 2024-05-02 15:44:00 +0900
categories: Swift
tag: [iOS, swift]
toc: true
---
 
# named Types
이름을 가진 타입으로 Class / Struct / Enum / Protocol이 여기에 해당한다. 이런 타입들은 **이름**을 지정해서 만들기 때문에 named Types이다(?) Int / String / Bool 등도 결국 Struct로 구현이 되어있기 떄문에 named Types 이라고 할 수 있다. 통상적으로 다른 언어에서도 기본으로 여겨지는 데이터 타입들이 이에 속하며 이름이 존재하기 때문에 확장이 가능하다.

```swift
struct People {
    var name: String
}

let first = People(name: "pinocchio22")

let socond = People(name: "pinocchio33")
```

# compound Types
이름이 없는 복합 타입으로 Swift 언어 자체에서 정의된다. 

## tuple Types
named types 나 compound types를 포함하며 다양한 데이터 묶음에 대하여 새롭게 정의하여 사용할 수 있는 타입이다. 

```swift
let tuple: (String, Int) = ("pinocchio22", 1)
```
tuple types에 named types인 String과 Int가 들어갈 수 있다.

## function Types
function / method / closure 등이 여기에 포함되며 일반적인 함수 사용방법과 약간 다르게 파라미터와 리턴타입이 있어야한다.
(parameter type) -> (return type)

### 파라미터가 존재하는 함수 타입
함수의 타입은 파라미터의 타입만을 작성하고 오직 어떤 타입의 파라미터인지만 고려하여 판단한다.

```swift
// 함수타입 (String) -> ()
func setPeople(name: String) {}
```

### 파라미터와 리턴타입이 존재하는 함수 타입
이 역시 파라미터와 리턴타입만을 작성하고 두 타입이 어떤 타입인지만 고려하여 판단한다.

```swift
// 함수타입 (String) -> (String)
func setPeople(name: String) -> String {}
```

### 파리미터와 리턴타입이 모두 존재하지 않는 함수 타입
파라미터와 리턴타입이 모두 존재하지 않기 떄문에 빈 괄호토 타입을 표기한다.

```swift
// 함수타입 () -> ()
func setPeople() {}
```
