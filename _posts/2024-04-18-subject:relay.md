---
title: "[iOS] BehaviorSubject와 BehaviorRelay 중 무엇을 사용해야할까?"
layout: single
date: 2024-04-18 18:09:00 +0900
categories: RxSwift
tag: [iOS, swift, RxSwift]
toc: true
---
 
# Subject / Relay
subject는 RxSwift에서 Observable의 역할과 Observer의 역할을 모두 할 수 있다. Relay는 onCompleted, onError에 의해 이벤트 스트림이 종료되지 않도록 Subject를 Wrapping한 것이다.

## Subject
RxSwift에서 제공하는 타입으로 Completed, Error 이벤트가 발생하면 subscribe를 종료한다. onNext() 메소드를 이용하여 subject에게 이벤트를 전달할 수 있고 subscribe 클로저 안에서 이벤트에 접근할 수 있다. viewModel을 input / output 구조로 구성할때나 특정 관찰가능한 값을 저장하고 전역으로 사용할때 유용할 듯 하다.

### PublishSubject
초기값이 없는 상태 즉, 비어있는 상태로 생성되기 때문에 생성 직후에 observer가 구독을 시작하면 아무런 이벤트도 전달되지 않는다. 또한 onCompleted 이벤트를 전달하면 모든 구독자에게 completed를 전달하고 더 이상 이벤트를 전달하지 않는다. 초기값이 필요 없고 구독 이후부터 이벤트를 전달받아야하는 경우에 사용하면 될 것 같다. 

### BehaviorSubject
초기값을 지정하여 생성을 하기 때문에 Observer가 구독을 시작하면 저장된 최신 이벤트를 즉시 전달한다. 이후 onNext로 이벤트가 전달될때마다 observer에게 이벤트를 전달한다. onCompleted 이벤트가 전달되었을때는 동일하게 더 이상 이벤트를 전달하지 않는다.

### ReplaySubject
하나 이상의 이벤트를 버퍼에 저장하고 구독시 버퍼에 저장되어있던 모든 이벤트를 전달한다. 생성시에 버퍼 사이즈를 지정하여 저장할 이벤트의 개수를 정한다. 다만 버퍼는 메모리에 저장되기 떄문에 버퍼 크기를 항상 고민하고 사용해야하는 주의점이 있다. 위의 두 종류와 달리 onCompleted, onError 이벤트를 전달하면 새롭게 구독하는 observer에게는 기존 버퍼에 담겨있는 이벤트를 방출하고 종료한다는 차이점이 있다.

### AsyncSubject
위 세 종류와 다르게 생성이후 observer가 구독을 시작해도 이벤트를 즉시 전달하지 않는다. 정확히는 onCompleted 이벤트가 전달되기 전까지 어떠한 이벤트도 전달하지 않는 것 이다. onNext를 통해 이벤트를 전달하면 계속해서 이벤트를 최신화하고있다가 onCompleted 이벤트를 전달하면 최근 이벤트를 전달하고 종료된다. 다만 onError 이벤트가 전달되면 이벤트를 전달하지않은 채로 종료한다.

## Relay
RxCocoa에서 제공하는 타입으로 Completed, Error 이벤트를 발생시키지 않고 Dispose 되기 전까지 계속 동작하기 때문에 UI 작업에 용이하다. accept() 메소드를 이용하여 Relay에게 이벤트를 전달할 수 있고 value 프로퍼티를 통해 이벤트에 접근할 수 있다. <br>
종류로는 PublishSubject를 Wrapping 한 PublishRealy와 BehaviorSubject를 Wrapping 한 BehaviorRelay가 있다.
