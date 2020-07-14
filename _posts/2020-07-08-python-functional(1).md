---
layout: post
title:  "Functional Paradigm(1)"
subtitle: "Functional Paradigm(1)"
categories: programming
tags: python
comments: true
---

lambda, encapsulation, comprehension, generator, recursion

---

### Functional Paradigm(1)

- 절차적 형식과 다르게, 함수형 내에서는 이름 짓지 않고 정의할 수 있음
  - ex) lambda
- 기본적으로 함성합수로 구성되어 연산



#### [lambda]

- 익명함수 식(expression)
- 일회용으로 사용
- 함수의 정확한 이름이 없음
  - 단, 가짜 이름으로 불러올 수 있음 (=메모리에 남길 수 있음)

~~~python
b = lambda : 2 #b = 가짜 이름
b.__name__ #'<lambda>' - 정확한 이름이 아님
b() #실행
~~~

- 가짜이름로 정의한 람다식 vs. def방식 뭐가더빠를까?
  - 시간 같음 : 내부구현은 같기 때문



#### [Encapsulation]

- '함수화' : 여러개의 문장을 묶어 함수로 만든 것

  - 이를 통해 **재사용**이 용이해짐
  - ex. hyperparameter tuning 시 모델 자체를 함수로 만듬

- LEGB 유의 : 함수 외부에서 내부 variable에 접근 불가능

  ```python
  def dd():
      xx = 1
      return xx
  xx #NameError - 존재하지X
  ```

- 함수 내부에서 외부 variable 접근 가능

  ```python
  cccc=1
  def dd():
      return cccc
  dd() #1
  ```

  - 단, 변경은 못함

  ```python
  cccc=1
  def dd():
      cccc +=1
      return cccc
  dd() ##UnboundLocalError 발생
  ```

  - 해결방안: **global**

  ```python
  x = 1
  def a():
      x=2
      def b():
          global x
          x+=3
          return x
      return b()
  
  a() #4
  ```

  - nonlocal : 같은 큰 함수 내에 존재하는 변수를 참고

  ```python
  x = 1
  def a():
      x=2
      def b():
          nonlocal x
          x+=3
          return x
      return b()
  
  a() #5
  ```

  - LEGB : 가장 가까운 것 순서대로 값을 찾아가는 규칙
    - Local -> Enclosing -> Global -> Builtin

- def x(): ~~~ 에서 함수는 x()가 아닌 x

  - x를 가지고 함수 관리할 수 있음





#### [Comprehension]

- sequence를 한꺼번에 만들어냄 - functional의 장점
  - 하나씩 메모리 올리는 iter의 반대

~~~python
#list comprehension
[x+1 for x in range(10) if x%2 ==0] 
[str(x) if x%2==0 else 1 for x in range(10)] #앞쪽에 if 집어넣기 : else가 꼭 필요함

#set comprehension
{str(x+1) for x in range(10) if x%2==0}

#dict comprehension
{i:i for i in range(10) if i%2==0}
~~~

- 빠르고 간결

- sequence 모두 생성 가능(immutable인 tuple 빼고)

  - ()은 **generator**가 생성됨




#### [generator]

~~~python
t = (i for i in range(10) if i%2==0)
next(t) #0
~~~

- generator 특 : next 존재

- 왜 존재한다? tuple 형태로 만들었기 때문에 

- generator 생성 방법 2가지

  - tuple / yield

  ~~~python
  def b():
      yield from range(10)
  t=b()
  next(t)
  ~~~

  

#### [recursion]

- Functional paradigm에서는 loop/iter를 사용하지않는 대신 recursion을 사용한다
  - 그러나 파이썬에서는 사용하지 X
    - tail recursion elimination / optimization을 제공하지 않기 때문에 속도도 느리고, 메모리도 많이 잡음
    - tail recursion elimination에 대한 내용은 [여기](https://popo97kr.github.io)
- recursion은 수학과 연관 높음

~~~python
def running_sum(numbers, start=0):
 if len(numbers) == 0:
     print()
     return
 total = numbers[0] + start
 print(total, end=" ")
 running_sum(numbers[1:], total)

#이는 reduce sum 방식과 동일
~~~



- 파이썬에서는 주로 recursion 쓰지 않고 functional 관련 모듈 세가지 사용

  - itertools

  - functools

    - partial : 남이 만든 함수 일부 변경 가능

    ~~~python
    from functools import partial
    def add(x,y):
        return x+y
    add1 = partial(add, y=1)
    add1(3) #4
    ~~~

  - operator 

  ~~~python
  from operator import add
  add(3,6)
  ~~~

  ​		- 기호(ex.+)없이 function으로 모든게 가능하다.