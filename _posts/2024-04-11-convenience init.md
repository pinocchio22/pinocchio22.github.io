---
title: "[iOS] init VS convenience init"
layout: single
date: 2024-04-11 18:13:00
categories: Swift
tag: [iOS, swift]
toc: true
---

# init 이란?
## Designated init
Swift의 초기화 이니셜라이저로서 클래스의 모든 프로퍼티가 초기화 될 수 있도록 해야한다. 사용시에는 그냥 Init() 으로 사용한다.

## Convenience init
보조 이니셜라이저로서 원래 이니셜라이저인 init을 도와준다. 클래스의 파라미터 중 일부의 default값을 지정하고 나머지 파라미터에 대해서 새로운 이니셜라이저를 선언한다. Designated init이 필수로 선언되어 있어야하며 모든 프로퍼티를 전달받지 않아도되는 상황에 유용하게  사용할 수 있다.

# 사용법
 ``` swift
 class Student() {
	 init (name: String, number: Int) {
	 	self.name = name
 		self.number = number
	 }
 
	 convenience init (number: Int) {
 		self.init(name: "이름", number: number)
 	}
 }
 
 var first = Student(name: "pinocchio22", number: 1)
 var second = Student(number: 2)
 ```

``` swift
print(first)
print(second)

Student(name: "pinocchio22", number: 1)
Student(name: "이름", number: 2)
```