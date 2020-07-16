---
layout: post
title:  "객체지향(5)"
subtitle: "객체지향(5)"
categories: programming
tags: python
comments: true
---



디자인설계에 유용한 메타클래스(Metaclass)와 추상클래스(Abstractclass)에 대해 알아본다.

- Metaclass (메타클래스)
- Abstractclass (추상클래스)

---

### 객체지향(5)

#### [메타클래스 (Metaclass)]

**= 클래스의 클래스**

- default는 type이며, 이를 변경하여 클래스의 행동방식을 제약할 수 있음

~~~python
#메타클래스 생성
t = type('int2', (int,), {})
issubclass(t, int) #True

#메타클래스 생성2
class MyType(type):
    pass

class MySpecialClass(metaclass = MyType):
    pass
~~~

​			object(default 기능)로부터 기능을 물려받고, 행동은 MyType으로부터 물려받음



 - 대표적인 예) **Singleton**

    - 인스턴스를 하나만 만들도록 제한
      	- 기존 인스턴스 존재 시 그 인스턴스 반환, 없으면 새 인스턴스 생성
   	- 값을 전체 프로그래밍 내에서 동일하게 관리할 때 유용함

   ~~~python
   class Singleton(type):
       instance = None
       def __call__(cls, *args, **kwargs): #cls = self
           if not cls.instance: #instance 가 없다면 새로 객체 생성
               cls.instance = super(Singleton, cls).__call__(*args, **kwargs)
           return cls.instance
       
   class ASingleton(metaclass = Singleton):
       pass
   
   a = ASingleton()
   b = ASingleton()
   a is b #True
   ~~~

- dir에서 'meta'가 나오면 metaclass 사용한 것



#### [추상클래스 (Abstractclass)]

- 설계만 하고 실체는 구현하지 않은 클래스
  - 자신을 물려받은 클래스가 직접 구현하도록 함

- **NotImplemented type** : 나중 구현을 위해 지정해놓은 type

  - NotImplementedError = 내 기능을 구현해야 하는데 안해서 발생

- from **collections.abc** import Iterator, Iterable

  - abc = abstract base class

- from abc import **ABCMeta**

  - sklearn에 많이 구현되어 있음

    - mlpclassifier, SamplerMixin, 등

  - dir 했을 때 '\__abstractmethods__'가 튀어나온 것 = ABC를 사용한것

    - 즉, 상속받은 클래스가 구현해야 한다는 뜻

    ~~~python
    from abc import ABCMeta
    
    class MyABC(metaclass=ABCMeta):
        pass
    x = MyABC()
    dir(x) #__abstractmethods__ 있음
    ~~~

    