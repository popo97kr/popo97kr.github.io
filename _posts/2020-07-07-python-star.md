---
layout: post
title:  "*의 용도"
subtitle: "*의 용도"
categories: programming
tags: python
comments: true
---

파이썬 내 *의 용도 7가지

Python은 Multiparadigm Language이다.

---


### Usage of *
**1. multiplication**

**2. exponent**

**3. 반복(string concatenate)**

**4. unpacking - 나머지것 할당 ex) a, *b = 1,2,3**

**5. 함수 가변 parameter - 2가지**

~~~python
def simple(*args, **kwargs):
    if 'color' in kwargs:
        print('hello')
    return args

simple(1, 2, color=3)
#hi
#(1,2) 출력
~~~

* **(*args) : 가변 positional 선언** 
  - return <u>tuple</u>
* **(\**kwargs) : 가변 keyword 선언**
  - return <u>dictionary</u>
  - 키워드 방식으로 인자 입력
  - 단 키가 중복되면X, 키에 숫자/특수문자가 들어올 수 X

- defaultdict(self, /, *args, *kwargs) =>어떻게 변할 지 모르는 경우

**6. argument unpacking**

~~~python
def x(*a):
    return a

x(*[1,2,3,4,5]) #(1,2,3,4,5)
x(*{'a':1, 'b':2}) #('a','b')
~~~

- \* : 튜플로 변경. Dict는 key만 입력됨

~~~python
def x(**a):
    return a

x(**{'a':1, 'b':2})
~~~

- \** : dict형태 그대로 입력 가능

**7. "all"**

- from sample import *
  - 해당 파일/폴더에 있는 모든 것을 불러옴
  - 단, 이름이 underbar로 시작되는 것은 가져오지 X

<파이썬 : multi paradigm language>

- 명령형
  - 절차적 프로그래밍
  - 객체지향 프로그래밍
    - 유지보수관리,기능추가에 효율적
    - 단 코드가 길어지는 단점
- 선언형
  - functional programming(https://docs.python.org/ko/3/howto/functional.html)
    - html, SQL
    - tf, keras, pytorch에서 사용 - functional API
    - 하나의 인자값에 대해 하나의 출력값을 가진다 (?)
    - 형식적으로 증명이 가능하다
      - 즉 이론을 프로그래밍으로 옮길 수 있음
    - 동시에 여러번 실행시킬 수 있다(multiprocessing) (모듈성)
    - 디버깅에 용이











