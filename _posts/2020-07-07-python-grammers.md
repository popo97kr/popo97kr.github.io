---
layout: post
title:  "else, *, from, _의 여러가지 용도"
subtitle: "else, *, from, _의 여러가지 용도"
categories: programming
tags: python
comments: true
---

else, *, from, _의 여러가지 용도

---


### Usage of "else"
**1. if else**

**2. 반복문 뒤**

~~~python
for i in range(5):
    print(i)
else:
    print('end')
~~~

- 반복문이 잘 돌아갔는지 확인하기 위해 사용
  - 중간에 break로 반복문 빠져나올 시엔 else부분 실행X

**3. try/except** 

- 특히 크롤링 시 유용

~~~python
try:
    a=1/0
except ZeroDivisionError:
    print('divided zero')
except Exception as e:
    print(e)
else:
    print('no error')
finally:
    print('always')
~~~

- error 나지 않을 때 else부분 실행



### Usage of "*"

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



### Usage of "from"

**1. from ~~ import ~~**

**2. yield를 사용해 generator 생성 **

~~~python
def b():
    yield from range(10)
t=b()
next(t)
~~~

**3. try except 문**

~~~python
try:
    1/0
except ZeroDivisionError as e:
    raise Exception('ttt') from None
~~~

- raise ~~ from ~~ : 어떻게 에러를 전파할지 정해준다
  - from None : 출력 X



### Usage of "_"

**1. 최근값 불러오기**

~~~python
1
_ #1
~~~

**2. int 중간에 있을 땐 아무런 의미 X**

~~~python
100_000 #=100000
~~~

**3. @property 에서의 mangling**

- \_x => \__class_x 으로 변경됨

**4. double underbar (\__)**

- ex) \__init__
- =dunder, magic/special 이라 불림
- 시스템적으로 구현되어있음

**5. assign 시 필요없는 변수에**

- ex) _, b = 3, 4

**6. 함수이름_**

- ex) isin_
- 원래 함수와 유사한 기능을 뜻할 때

**7. 한 변수에 다국어 처리할 때, 이름앞에 _을 붙임**