---
title: "[iOS] Delegate vs Callback"
layout: single
date: 2024-04-25 13:02:00 +0900
categories: Swift
tag: [iOS, swift]
toc: true
---
 
# Swift에서 비동기를 처리하는 방법들
swift에서 비동기를 처리하는 방법으로 Delegate 패턴이나 Callback 함수등이 있다. 결국 이벤트를 감지해야하는 곳에서 메소드를 정의하고 이벤트를 발생시키는 부분(?)에서 메소드를 구현하여 반응하는 공통적인 구조이다. 목적이 같아서 그런지 구조가 비슷해보여서 정리가 필요하다고 생각이 된다. <br>

```swift
class A {
    var buttonTapped: (() -> Void)?
    var number = 0
}

class B {
    let aClass = A()
    let tableView = UITableView()

    func buttonTapped() {
        aClass.buttonTapped = {
            self.tableView.reloadData()
        }
    }
}
```
Callback

```swift
protocol aClassDelegate {
    func buttonTapped()
}

class A {
    var delegate: aClassDelegate?
}

class B: aClassDelegate {
    let tableView = UITableView()

    func buttonTapped() {
        tableView.reloadData()
    }
}
```
Delegate pattern

# Delegate 패턴
Delegate 패턴은 특정 작업을 다른 객체에게 위임하는 디자인 패턴으로 주로 1:1 관계에서 사용된다. 프로토콜을 통해 정의되며 객체 간의 상호작용을 관리할 때 주로 사용된다. delegate 패턴을 사용하면 관계를 명확히 정의할 수 있어서 가독성 측면에서 장점이 있다.

1. 프로토콜 정의: 특정 작업을 수행하기 위해 필요한 메소드를 정의한다.
2. 프로토콜 채택과 구현: 다른 클래스나 구조체가 이 프로토콜을 채택하고, 프로토콜에 정의된 메소드를 구현한다.
3. Delegate 객체 지정: 이벤트를 처리할 객체를 delegate로 지정하여, 이벤트 발생 시 해당 delegate 객체의 메소드를 호출한다.

# 콜백 함수
콜백 함수는 특정 시점에 실행되어야 함수를 말하며 함수나 클로저를 인자로 받아서 해당 시점에 호출하는 것이다. delegate 처럼 객체와 객체간의 관계가 중심이 아닌 특정 이벤트의 시점이 중심이다. 하나의 이벤트에 여러 개의 콜백을 연결할 수 있기때문에 delegate 보다 유연하지만 코드가 중첩될수록 복잡해질 수 있기 때문에 주의해야 한다.

1. 선언: 특정 작업이 완료될 때 실행될 함수나 클로저를 매개변수로 받는 함수를 정의한다.
2. 구현: 해당 함수를 호출할 때 실행될 클로저를 전달한다.
3. 실행: 조건이 충족되거나 이벤트가 발생했을 때, 전달받은 콜백 함수를 실행한다.

# Delegate 패턴의 장점
Delegate 패턴은 콜백함수에 비해서 명확성 이외에도 여러 측면에서 장점이 있다.

### 의존성 역전
Delegate 패턴을 사용하면 특정 클래스에 의존하는 것이 아닌 프로토콜에 의존하게 되므로 추상화의 장점이 있다. 높은 수준의 모듈이 낮은 수준의 모듈에 의존하지 않도록하기 때문에 코드간의 결합도가 낮아지고 유연성과 확장성이 향상된다.

### 책임의 분리
Delegate 패턴은 프로토콜을 통해 역할을 정의하기 때문에 어떤 기능이 어떤 객체에 의해 수행되어야 하는지가 명확해진다. 이렇게 하면 단일 책임 원칙을 지키는데 효과적이고 관심사 분리를 촉진할 수 있다.

### 테스트 용이
단위 테스트 과정에서 실제 객체 대신에 모의 객체를 사용하여 Delegate 메소드를 테스트할 수 있다. 이렇게 하면 수정이 편하고 원본 코드를 건드리지 않아도 되기 때문에 코드의 재사용성이 높아지고 테스트가 용이해진다.

### optional 메소드
Delegate 패턴은 프로토콜을 이용하기 때문에 프로토콜의 정의 과정에서 필수적인 메소드만 정의해야한다고 생각했으나 메소드를 optional로 구현하게되면 delegate가 모든 메소드를 구현할 필요가 없어지므로 더욱 유연한 설계가 가능하다. 다만 @objc 키워드가 필요하다..
