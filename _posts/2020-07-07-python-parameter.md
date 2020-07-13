---
layout: post
title:  "매개변수와 인자"
subtitle: "매개변수와 인자"
categories: programming
tags: python
comments: true
---

파이썬 매개변수(parameter)와 인자(argument)

- parameter
  - 함수선언, 클래스선언
- argument
  - 키워드방식
  - positional 방식

Python은 Multiparadigm Language이다.

---


### Python Parameter/Argument
#### [Parameter]

- for declaration(선언, 정의)
  
  - 1.함수선언, 2.클래스선언이 있음
  
  ~~~python
  def hi(a,b,c):
      return a+b+c
  ~~~



#### [Argument]

- 선언한 함수에 집어넣는 실제 값 (호출할 때 사용)

- argument 종류: 1.키워드방식, 2.positional 방식

  **1. 키워드방식** : hi(a=1, b=1, c=1)

  - 장점1: 순서를 변경해서 인자 전달 할 수 ㅇ
  - 장점2: 함수 정의시default값을 지정할 수 있음

  ~~~python
  def hi(a,b,c=5): #대신 default 정해놓은 인자가 맨 뒤로
      return a+b+c
  
  hi(b=1,c=3,a=2)
  ~~~

  - 키워드 방식있어서 함수 오버로딩 필요 X
    * 안쓰는건 None으로 default 해놓을 수 있기 때문

  **2. positional 방식** : hi(1,2,3)

  - 내장함수 shift+tab시 **'/'**가 있다면? - positional 방식만 지원한다는 뜻

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/argument1.jpg?raw=true"/>

- argument에 식 집어넣을수 있음 vs. parameter에는 식 사용할 수 X

~~~python
def aa(x):
    return x
aa(3 if a>0 else 5)
~~~

- 함수 내 인자 한번 정의 시 메모리에 저장 - 더 불러와도 메모리 변경되지 X

~~~python
import time
def bb(x=time.time()):
    return x
bb()
bb() #바뀌지않음
~~~

~~~python
def cc(x=[]): #default parameter를 mutable로 주는 것은 좋지 X
    x.append(3)
    return x

cc() #[3]
cc() #[3,3]
~~~















