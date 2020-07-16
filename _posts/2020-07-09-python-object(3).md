---
layout: post
title:  "객체지향(3)"
subtitle: "객체지향(3)"
categories: programming
tags: python
comments: true
---

- 상속 - 다중상속, mixing technique

- 오버로딩 vs. 오버라이딩

---

### 객체지향(3)

#### [상속(Inheritance)]

= 부모클래스에서 정의된 모든 것을 사용할 수 있는 방법

- **\__mro__ / mro()**: 상속 체계 순서 출력

- **issubclass()** : 클래스의 부모클래스인지 확인하는 방법

  ~~~python
  class A(int):
      pass
  issubclass(A, int) #True
  issubclass(A, (int,str)) #True - 둘중 하나라도 해당되면 ㅇ
  ~~~

- **\__base__** : 부모클래스 중 mro 기준 가장 최근 클래스

- **\__bases__** : 모든 부모클래스

  ~~~python
  class A:
      pass
  class B:
      pass
  class C(A,B):
      pass
  
  C.__base__ #__main__.A
  C.__bases__ #(__main__.A, __main__.B)
  ~~~

  

**상속체계의 단점; 다중상속 시 순서의 문제가 발생**

~~~python
#다중상속 예시) 다이아몬드 문제
class A:
    def __init__(self):
        print('A')

class B(A):
    def __init__(self):
        self.x = 'B'
        A.__init__(self)
        print('B')
        
class C(A):
    def __init__(self):
        self.x = 'C'
        A.__init__(self)
        print('C')
        
class D(B,C):
    def __init__(self):
        C.__init__(self) 
        B.__init__(self)
        print('D')
        
d = D()
'''
A - 중복
C
A - 중복
B
D
'''
d.mro() #[__main__.D, __main__.B, __main__.C, __main__.A, object]
~~~



- 이는 super()를 사용하면 해결가능하다.

~~~python
class A:
    def __init__(self):
        self.x = 'A'
        print('A')

class B(A):
    def __init__(self):
        self.x = 'B'
        super().__init__()
        print('B')
        
class C(A):
    def __init__(self):
        self.x = 'C'
        super().__init__()
        print('C')
        
class D(B,C): #앞에꺼가 가장 최근꺼
    def __init__(self):
        super().__init__() #알아서 B, C두개 다 가줌
        print('D')
        
d = D()
'''
A
C
B
D
'''

d.x #A
~~~



- **mixing technique** = 서로 독립적인 부모 클래스로부터 다중상속을 받는 것

  - 대표 예가 sklearn

  ![image-20200709160639293](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200709160639293.png)



**오버로딩 vs. 오버라이딩**

- 오버로딩 : **다양한 인자를 가진 메소드** 여러개를 가지는 것
  - 파이썬에서는 지원하지 X
- 오버라이딩 : 하위 클래스가 상위 클래스의 **메소드를 재정의**하는 것
  - 부모 클래스의 메소드를 변경/추가하고 싶을 떄 사용