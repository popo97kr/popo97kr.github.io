---
layout: post
title:  "Functional Paradigm(3) - Decorator"
subtitle: "Functional Paradigm(3) - Decorator"
categories: programming
tags: python
comments: true
---

Decorator

---

### Functional Paradigm(3)

#### [Decorator]

- @으로 시작하는 것들

- **타 클래스 내의 함수를 불러와서 **추가/변경할 때 사용

  - 특정 함수 위에 decorator 작성하면 decorator 내에서 정의된 함수가 불러와진다

- **decorator 생성 규칙** (@를 붙일 수 있는 조건)

  1. 가장 밖에 있는 함수는 인자로 function을 받아야 한다
     - (<-> closure 는 값을 받음)
  2. 인자로 받은 function을 nested 함수 안에서 실행시킨다
  3. nested 함수를 return한다

  ~~~python
  def foo(fn):
      def inner():
          print('call function')
          fn()
          print('finished calling')
      return inner
  
  @foo
  def bar():
      print('bar')
      
  bar() 
  
  '''
  call function
  bar
  finished calling
  '''
  ~~~

  

- function 에 인자 받기 (그 수에 상관 없이)

  - 가변 positional / keyword 사용

  ~~~python
  def author(fn):
      def inner(*args, **kwargs):
          print('by author')
          fn(*args, **kwargs)
      return inner
  
  @author
  def y(a):
      print(a)
      
  y(['jihye','jh'])
  
  '''
  by author
  ['jihye','jh']
  '''
  ~~~

  - 위 예제의 문제점 : 진짜 이름을 출력했을 때, 'y'가 아닌 'inner'가 출력됨

    ![image1](https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/functional1.jpg?raw=true)

  - 이는 **@wraps**를 사용하여 해결한다

  ~~~python
  from functools import wraps
  def wrapper(fn):
      @wraps(fn) #이름을 불렀을 때 inner 대신 진짜이름 출력
      def inner(*args, **kwargs):
          fn(*args, **kwargs)
      return inner
  
  @wrapper
  def y(x):
      print(x)
      
  y.__name__ #y
  ~~~

  

- 삼중구조

  - **decorator에 인자 전달**

  ~~~python
  def outer(a):
      def wrapper(fn):
          @wraps(fn)
          def inner(*args, **kwargs):
              if a<20:
                  fn(*args, **kwargs)
              else:
                  print('by author')
                  fn(*args, **kwargs)
                  print(a)
          return inner
  	return wrapper
  
  @outer(24) #인자를 decorator에 전달
  def y(x):
      print(x)
      
  y('jihye')
  
  '''
  by author
  jihye
  24
  '''
  ~~~

  

- 2개이상의 decorator 사용 가능

  ~~~python
  @decorator1
  @decorator2 
  def foo():
      print('foo')
  ~~~

  - (foo함수) -> decorator2 -> (리턴함수) -> decorator 1



- tf2에서 decorator 자주 사용함

  - ex) @tf.function

    - 쓰면 더 빨라짐

      

- numpy/pandas

  ~~~python
  import numpy as np
  
  @np.vectorize
  def x(a,b):
      return a+b
  
  x([1,2],[3,4]) #array([4,6])
  
  #없다면 [1,2,3,4]로 나왔을것임
  ~~~

  

- [@property]((https://popo97kr.github.io/programming/2020/07/09/python-object(3)/)

- [@classmethod / @staticmethod](https://popo97kr.github.io/programming/2020/07/09/python-object(2)/)



- 기타 decorator 사용 예시)
  - timeit
  - typechecking
  - deprecation(warning만드는것)
  - [memoization](https://popo97kr.github.io/programming/2020/07/17/algorithm-memoization/)
  - checkpoint
  - 등등...

