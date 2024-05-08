---
title: "[iOS] Swift의 pattern"
layout: single
date: 2024-05-08 17:25:00 +0900
categories: Swift
tag: [iOS, swift]
toc: true
---
 
# Pattern
패턴은 특정 값이 아닌 값의 구조를 나타내므로 다양한 값으로 패턴을 일치시킬 수 있다. 대부분의 패턴은 switch / if / for 등의 키워드와 자주 사용되고 두 개 이상의 키워드가 합을 이뤄 동작한다. 우리가 자주 사용하던 코딩방식에도 **패턴**이 적용되고 있었으나 전혀 몰랐다. 이러한 패턴에는 어떤 종류의 값과도 일치하는 패턴과 어떤 종류의 값과도 일치하지 않는 패턴이 있다. 

# 어떤 종류의 값과도 일치하는 패턴
상수, 변수 및 옵셔널 바인딩에서 값을 소멸시키는데 사용되며 값을 추출하거나 무시하는 패턴이라고 생각하면 된다. 즉 값이 있어야만 실행되는 것(?)

## Wildcard Pattern
모든 값을 일치시키므로 값을 무시하고 _로 구성된다. 일치하는 어떠한 값도 사용되지 않을때 적절하다. 루프안에서 원하는 반복은 이루어지지만 현재에 일치하는 값을 사용하지 않고 무시할 수 있다.

```swift
let number = [1,2,3,4]

for _ in number {
	print("num")
}
```

## Identifier Pattern
모든값과 일치하며 일치된 값을 상수나 변수에 바인딩 한다. 상수나 변수에 값을 할당하는 자체가 Identifier Pattern이라고 할 수 있고 이때 암묵적으로 Identifier Pattern은 Value-Binding Pattern의 하위 패턴이다.

```swift
let number = 10
```

## Value-Binding Pattern
일치하는 값을 상수나 변수 이름에 바인딩한다. 상수는 let 키워드로 변수는 var 키워드로 시작하는것이 일반적인 값 할당과 동일하다. Value-Binding Pattern내의 Identifier Pattern은 새로운 상수나 변수를 일치하는 값에 바인딩한다. 선언된 상수나 변수에서 그치는 것이 아니라 새롭게 바인딩을 하는 것을 의미하는 것 같다(?)

```swift
let numbers = (1,10)

switch numbers {
	case let (a, b):
		print(a)
		print(b)
}
```
위와 같이 tuple로 선언된 numbers를 switch문을 통한 case안에서 새로운 a,b라는 상수에 일치하는 값을 바인딩하면 Value-Binding Pattern에 해당한다.

## Tuple Pattern
괄호로 묶인 0개 이상의 패턴을 쉼표로 구분하는 목록으로 해당 튜플 타입의 값과 일치한다. 타입 어노테이션을 이용하여 특정 타입의 튜플 타입과 일치하도록 튜플패턴을 제한할 수 있다. 

```swift
let numbers: (Int, Int) = (1,10)
```
의 경우 (x,y): (Int, Int)인 튜플 패턴을 사용했다고 볼 수 있다.

또한 튜플패턴은 어떤 종류의 값과도 일치하는 패턴이기 때문에 for-in문이나 상수, 변수 선언의 패턴으로 사용될 때 어떤 종류의 값과도 일치하지 않는 패턴과는 사용할 수 없다.

```swift
let numbers = [(0,0), (0,1), (0,2)]

for (0, x) in numbers {
	...
}
```
이때 0은 Expression Pattern에 해당하기 때문에 에러가 발생한다.

# 어떤 종류의 값과도 일치하지 않는 패턴
전체 패턴 일치에 사용되며 일치하려는 값이 런타입 시점에 없을 수 있다. 즉 값이 없어도 실행되는 것(?)

## Enumeration Case Pattern
열거형 패턴은 기존 열거형의 대소문자와 일치하고 switch문의 case레이블 / if, for-in 문등의 조건에 표시된다. 연관값을 가진 열거형의 case를 매칭시킬때 주로 사용한다고 보면 될 것 같다. 

switch문에서 이용

```swift
enum Animal {
    case dog(name: String)
    case cat(name: String)
    case fish(name: String, fin: Int)
}

let animal = Animal.dog(name: "snoopy")

// case (let _):
switch animal {
    case .dog(let name):
        print(name)
    case .cat(let name):
        print(name)
    case .fish(let name, _):
        // _ : Wildcard Pattern
        print(name)
}

// case let ():
switch animal {
    case let .dog(name):
        print(name)
    case let .cat(name):
        print(name)
    case let .fish(name, _):
        // _ : Wildcard Pattern
        print(name)
}
```

if문 에서 이용

```swift
if case let .fish(name, 4) = animal {
	// fin = 4 일치 && name 속성 사용
    print(name)
}

if case .fish(_, 4) = animal {
    // fin = 4 일치
}
```

for-in 에서 이용

```swift
let animals = [Animal.dog(name: "snoopy"), Animal.cat(name: "garfield"), Animal.fish(name: "nemo", fin: 4)]

for case let .fish(name, 4) in animals {
    print(name)
}

for case let .dog(name) in animals where name == "snoopy" {
    print(name)
}
```

## Optional Pattern
```swift
let number: Int? = 10

if let newNumber = number {
    print(newNumber)
}

if case let newNumber? = number {
    print(newNumber)
}
```
if let은 주로 단일 옵셔널 값의 바인딩에 사용되고 if case let은 패턴 매칭의 유연성을 활용해 다양한 조건을 포함할 수 있다는 점에서 차이가 있다고 하는데 솔직히 두개의 사용처를 아직 잘 구분 못하겠다..

대신 for-in 구문에서 옵셔널 값의 배열을 반복할 수 있는 편리한 방법을 제공하는 것은 이해가 간다. nil인 값이 포함된 배열이어도 nil이 아닌 요소에 대해서만 루프를 실행한다. 

```swift
let numbers: [Int?] = [1, 2, nil, 3]

for case let number? in numbers {
    print(number)
}
```
이런식으로 옵셔널인 배열을 이용하여 루프를 돌때 언래핑을 하지 않고 nil이 아닌 요소에만 접근할 수 있다.

```swift
let numbers = [1, 2, 3, 4]

for var number in numbers {
    number = number + 1
    print(number)
}

for case var number in numbers {
    number = number + 1
    print(number)
}
```
이렇게 반복문 안의 요소도 var 키워드를 이용하여 값을 변경할 수 있지만 아직 for-in + case를 이용한 패턴매칭의 사용처는 조금 어렵다.. (ps. for-case 구문은 패턴매칭을 위해서 사용되므로 var 키워드를 이용하여 값을 변경하여 사용하는 것보다 Enum 등을 이용한 값의 일치를 확인할 때 보다 적절한 사용방법이라는 말이 있다)

## Type-Casting Pattern
### is (명확하게는 Type-Checking Pattern)
* is **type**

is pattern은 런타임시 해당 값의 타입이 is pattern의 오른쪽에 지정된 타입 또는 해당 타입의 서브클래스와 동일한 경우 값과 일치하게 된다.

### as
* **pattern** as **type**

as pattern은 런타임시 해당 값의 타입이 as pattern의 오른쪽에 지정된 타입 또는 해당 타입의 서브클래스와 동일한 경우 값과 일치한다.

```swift
let numbers: [Any] = [1, "2", 3, "4"]

for case let number as Int in numbers {
    print(number)
}
```

# Expression Pattern
switch문의 case레이블에서만 나타나며 Swift의 범위연산자인 ~=를 사용한 입력표현식의 값과 비교된다. case레이블에서 변수가 아닌 직접적인 값을 이용하는 것이 표현 패턴이다.

```swift
let numbers = (1, 10)

switch numbers {
case (0, 0):
    break
case (0...1, 2...10):
    break
default:
    break
}
```

case (0...1, 2...10) 에서 0과1 사이, 2와 10사이라는 범위를 이용하였는데 case문에서 Equatable값과 일치할 때 뒤에서 ~= 연산자가 실행되기 때문에 내부적으로 범위연산자가 실행되고 있는 것이다.