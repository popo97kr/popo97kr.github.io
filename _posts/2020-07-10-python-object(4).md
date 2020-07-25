---
layout: post
title:  "객체지향(4) - Composition, Descriptor"
subtitle: "객체지향(4) - Composition, Descriptor"
categories: programming
tags: python
comments: true
---

- 상속 vs. Composition, Delegation(위임)
- Descriptor - get, set, delete

---

### 객체지향(4)

#### [상속 vs. Composition]

- 공통점 : 둘 다 **delegation(위임)**
  - = 본인 클래스에 없는 메소드나 attribute라면 본인이 아닌 부모 클래스를 차용

**1. 상속**

~~~python
class A:
    def xx(self):
        print('xx')
        
class B(A):
    pass

B.xx is A.xx #True - B가 A를 참조해 xx불러옴
~~~

~~~python
class X:
    def xx(self):
        print('X')
        
class Y(X):
    def xx(self): #overriding
        print('---') #이부분 추가
        super().xx()
        
y = Y()
y.xx()
'''
---
X
'''
~~~

- Coupling(결합성이 큼)
  - 부모클래스에서의 변경사항을 작은 코드로 변화시킬 수 ㅇ
    - 필요한 것만 오버라이딩 가능

**2. composition**  : 클래스 내에서 다른 클래스를 불러오는 것

~~~python
class Door:
    color = 'brown'
    
    def __init__(self, number, status):
        self.number = number
        self.status = status
        
    def open(self):
        self.status = 'open'
        
    def close(self):
        self.status = 'closed'
        
class SDoor:
    color = 'gray'
    locked = True
    
    def __init__(self, number, status):
        self.door = Door(number, status) #클래스 내 Door를 인스턴스화
        
    def open(self): #Overriding - SDoor open하면 이걸 사용
        if self.locked:
            return self
        self.door.open() #delegation
        
    def close(self):
        self.door.close()
~~~

- Decoupling(유연함)
  - 가져온 클래스의 모든 메소드를 다시 입력시켜줘야 함
  - 이는 변경 사항이 많을 때 유용 -> 변경 시 오버라이딩하면 됨



**+) composition + \__getattr__**

- 상속과 똑같이 필요한 부분만 고칠 수 있음
  - 나머지는 원본 클래스에서 그대로 가져올 수 있게 함
  - 즉, 일일이 메소드 재입력 시킬 필요 X
- 파이썬에서 상속을 잘 안 쓰는 이유는 얘가 있기 때문임

~~~python
class SDoor:
    locked = True
    
    def __init__(self, number, status):
        self.door = Door(number,status)
        
    def open(self): #Overriding
        if self.locked:
            return self
        self.door.open()
        
    def __getattr__(self, attr): #나머지 메소드는 Door 가서 실행
        return getattr(self.dor, attr)
    
s = SDoor(1,'open')
s.close()
s.status #closed
~~~



#### [Descriptor]

- .을 이용하는 모든 행동을 바꾼다.
- **get, set, del**로 이루어져 있다
- descriptor의 사용 방법은 3가지가 있다
  - composition
  - descriptor property 객체
  - property decorator

**1. composition 방식**

~~~python
class Value:
    def __init__(self, x):
        print('init')
        self.x = x
    def __get__(self,a,b): #인자 3개 필요
        print('get')
        return self.x+100
    def __set__(self, x):
        print('set')
        self.x = x
        
class MyClass:
    t = Value(2)
    
MyClass.t #()안씀. get함수 호출
'''
init
get
102
'''
~~~

- 딥러닝에서 많이 쓰임
  - 모델클래스 안에 다른 레이어 클래스 원하는 값으로 불러오기
    - Dense(,,)

**2. descriptor property 객체 방식**

- 변수 대신 __변수 로 사용

~~~python
class C:
    def __init__(self, x):
        self.__x = x
    def getx(self): 
        print('get')
        return self.__x
    def setx(self, value): 
        print('set')
        self.__x = value
    def delx(self): 
        del self.__x
    x = property(getx, setx, delx, "'x' property")

c = C(4) 
c.__x #없음
c.x #get 호출 - 4 출력
~~~

- c.\__x 없는 이유 : \_C__x로 변하기 때문 **(mangling)**
  - 사용자는 편하게 c.x로 엑세스 가능
  - 이름을 변경해 **접근제한**을 둘 수 있음
    - information hiding에 유용
    - 또다른 접근제한 관리 방식: [encapsulation(global, nonlocal)](https://popo97kr.github.io/programming/2020/07/08/python-functional(1)/)

**3. property decorator 방식** - @property

- @property - get
- @x.setter - set
- @x.deleter - del

~~~python
class D:
    def __init__(self, x):
        self.__x = x
        
    @property
    def f(self):
        print('ff')
        return self.__x
    
    @x.setter
    def f(self, x): #오버로딩 지원
        print('set')
        self.__x = x
        
d = D(3)
d.x() #error - callable 안됨(get호출)

d.x #ff
~~~

- 오버로딩 지원
  - 즉, 이름이 같아도 덮어씌우지 않음
  - 파이썬에선 원래 오버로딩 지원 안하나 decorator를 통해 가능케 함