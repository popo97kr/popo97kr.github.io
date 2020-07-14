---
layout: post
title:  "Functional Paradigm(2)"
subtitle: "Functional Paradigm(2)"
categories: programming
tags: python
comments: true
---

first class function, callable, closure, decorator

---

### Functional Paradigm(2)



#### [First Class Function]

- function을 값처럼 사용할 수 있음
  - 할당 가능
  - 리스트 안에 여러 function 집어넣을 수 있음

~~~python
a = [len, str]
a[0]([1,2,3])
~~~



#### [Callable]

- **괄호를 붙일수 있다 = callable 리턴값 True**

- callable 3가지

  1.function 계열

  ~~~python
  callable(len) #len()가능
  callable(int) #int() 가능
  
  def f():
      return 1
  callable(f)
  ~~~

  2.객체 클래스 인스턴스화

  ~~~python
  class A:
      pass
  
  callable(A) #인스턴스화 할 때 a = A()
  ~~~

  3.**클래스 내 call함수** 존재 시 객체에 괄호 붙일 수 ㅇ.

  ~~~python
  class A:
      def __call__(self):
          print('call')
          
  a = A()
  a() #call
  ~~~

  - function으로 closure만든 방식



※-able/is- : 주로 predicate 용어로 쓰임



#### [Closure]

- 중첩 : function 안에 function 정의

~~~python
def x():
    def y():
        return 1
    return y #함수를 return

callable(x()) #True - 한번 더 괄호 붙일 수 ㅇ
x()() #1
~~~



- 장점 : 동적으로 무수한 함수를 만들어 낼 수 있음

~~~python
def f(t):
    def g(x):
        return x+t
    return g

f(1)(5) #6
y = f(4) #y=x+4가 됨
~~~

- 딥러닝api개발자에겐 유용 - 가데이터 생성 시 유용(무수한 함수를 만들기때문)

- 함수 안에 함수 정의하는 방법 두가지 : 1.Closure, 2. Decorator



#### [Decorator]

