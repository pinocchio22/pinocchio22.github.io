---
title: "[iOS] [RxSwift] subscribe, bind, drive"
layout: single
date: 2024-04-10 18:37:00
categories: RxSwift
tag: [iOS, swift, RxSwift]
toc: true
---

# subscribe
onNext, onError, onCompleted, onDisposed를 모두 가지고 있다. 구독을 한 대상의 상태가 변하면 이벤트를 전달 받는다.

# bind
onNext만 가지고 있다. error와 completed가 발생하지 않고 무한히 이벤트를 방출하는 상황에 적절하며 항상 메인 스레드에서 동작하기 때문에 UI 작업에 적합하다. 내부적으로 subscribe를 사용하며 단순히 데이터의 변경을 처리할때 간편하게 사용하는 용도이다. onNext 이벤트에 대해서만 반응하고 error 이벤트가 들어오면 에러 로그를 콘솔에 출력한다.

# drive
onError를 가지고 있지 않다. 역시나 메인 스레드에서 동작하므로 UI 작업에 적합하다.  하지만 drive는 내부적으로 share(replay: 1, scope: .whileConnected)가 구현되어 있기 때문에 스트림을 공유한다는 점이 bind와의 차이점이다. 내부적으로 subscribe를 사용하며 drive를 사용하기 위해서는 asDrive()를 통해 옵저버블에 drive trait를 부여해야한다.

## Relay + Drive
relay는 subject를 래핑한 클래스로 complete와 error를 방출하지 않고 accept로 이벤트를 받는다. drive는 메인스레드에서만 실행되어야하기 때문에 보다 안전하고 컨트롤 프로퍼티는 실패할 수 없기 때문에 error를 가정하지 않는다. 따라서 relay + drive 는 에러가 없고 메인스레드에서 실행되어야하는 UI 작업에 적절하다.

## Subject + Subscribe / bind
값을 사용하기 위해 저장하거나 onCompleted / onError에 대한 처리가 필요할때 사용하면 적절할 것 같다(?)