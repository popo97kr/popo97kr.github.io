---
layout: post
title:  "객체지향(1)"
subtitle: "객체지향(1)"
categories: programming
tags: python
comments: true
---

- Class Attribute / Instance Attribute
- Class Method / Instance Method
- Staticmethod

---

### 객체지향(1)

**객체지향의 용도 : 1. 재사용, 2. 유지보수, 3. 확장**

- 여러 객체에 따라 각각 상태의 변경이 조금씩 필요할 때 유용

  ==> pytorch, keras 에서 구현된 모듈을 변경해 사용할 수 있는 이유

- 클래스는 실제 사용을 위해 **instance화**를 해야한다.

  ~~~python
  isinstance(b, B) #b는 B의 객체인가?
  ~~~

- 함수도 객체이다. (=first class function)



#### [Class Attribute / Instance Attribute]

**1. class attribute**

- 객체는 class attribute를 참조할 수 있음

~~~python
class A:
    a=1 #class attributes
    
x = A()
x.a #1
A.a #1
A.b #AttributeError 발생

A.a = 5
x.a #5
~~~



**2. instance attribute**

- 객체는 자기만의 고유값/메소드를 가질 수 있음 = instance attribute

  - instance는 우선 instance attribute를 찾으며, 만약 instance attribute가 정의되지 않았을 경우 class attribute를 참조함
    - 위 코드는 그의 예시

- instance attribute 존재 확인 방법 : **vars() / \__dict__**

  ~~~python
  vars(x) #{}
  ~~~

- instance attribute 생성 방법 : **\__init__** or **메소드**

  ~~~python
  class A:
      a=1
      b=2
      def __init__(self):
          self.a = 3
      def bb(self):
          self.b = 4
          
  A.a #1
  z = A() 
  z.a #3
  ~~~

  - \__init__ :객체생성과 동시에 호출되어 instance attribute 생성 

  - 메소드 : 원할 때만 instance attribute 생성

    ~~~python
    z.b #2 (아직까지는 class attribute)
    
    z.bb() #메소드 호출한 순간 instance attribute 생성
    z.b #4
    ~~~

- \__init__ 인자 받기

  ~~~python
  class A:
      a=1
      b=2
      def __init__(self,inp): 
          self.a = inp 
      def bb(self):
          self.b = 4
  w = A(10)
  w.a #10
  ~~~

  - 객체 부를 때 마다 attribute 다르게 설정할 수 있음





#### [Class Method / Instance Method]

**1. Instance Method**

- Instance의 관점에서 method는 **method**이다.
  - 따라서 self 생략 가능하다.

  ~~~python
  class B:
      x=1
      def __init__(self): print('init')
      def t(self):
          print('t')
          
  b = B()
  b.t()
  ~~~



**2. Class Method**

- Class의 관점에서 method는 **function**이다.

  - 따라서 class를 통해 method 호출할 땐 self를 생략할 수 없다.

  ~~~python
  B.t(b) #객체를 인자로 넣어야 함
  ~~~

- 그러나 Class도 method를 method로 바라볼 수 있다. **@classmethod**를 사용하면.

  ~~~python
  class B:
      x = 1
      def __init__(self):
          self.s = 3
          
      @classmethod
      def t(self):
          a = 3 #class attribute
          
  B.t() #Class Method
  ~~~

  - self 생략 가능

  - class method에서는 class attribute를 생성한다.

  - 또한, instance가 class method를 불러오면 class attribute를 생성한다.

    - 내부적으로 instance가 class로 변환되어 실행된다.

    ~~~python
    b = B()
    b.t() #class attribute
    ~~~




#### [Staticmethod]

- Instance method/Class method와 달리 self를 인자로 받지 않는다.
- 본래 staticmethod는 클래스에서만 직접 접근할 수 있는 메소드이나, 파이썬에서는 인스턴스를 통해서도 접근이 가능하다.
- @staticmethod 통해 구현

~~~python
class C:
    @staticmethod
    def a(x,y):
        print(x+y)
        
C.a(1,3)

c = C()
c.a(2,4) #instance통해서도 접근가능
~~~

