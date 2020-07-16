---
layout: post
title:  "객체지향(2)"
subtitle: "객체지향(2)"
categories: programming
tags: python
comments: true
---

- Type()은 객체의 클래스를 출력한다.
- 그 외 instance 관련 사항들

---

### 객체지향(2)

#### [Type 이란]

우선, 값을 만드는 방법에는 크게 두가지가 있다.

1. 리터럴(from 선언문)

   **2. 인스턴스화**

​		ex) a=1

​			--> 이는 사실 a = int(1)로 인스턴스화 한것이다.

​				  즉, \__init__의 인자로 1을 집어넣어 instance attribute를 생성한 것이다.

​		ex2) b = list('지혜')      #['지','혜']

​			--> 이 또한 인스턴스화를 통해 type casting coersion을 진행한 것이다.

​				  ※ 파이썬에는 as.integer와 같은 명시적 type casting이 존재하지 않음

​				  ※ typecasting이 다 가능한 것은 아니고, \__init__에서 제공하는 기능일 시에만 가능하다. 따라서 shift+tab을 잘 활용하자.

​		ex3) list(map(lambda x: x+1, [1,2,3,4])) 

​			--> 인스턴스화를 통해 map을 list 형태로 typecasting한 것



**즉, type은 객체의 class를 뜻한다.**

~~~python
type(1) #int
~~~

~~~python
def x(a):
    return a

type(x) #function
~~~

~~~python
import pandas as pd
type(pd) #module
~~~

- datatype, function, module 등 모든 것들은 **클래스**인 것이다



\__class__ 사용해도 된다.

~~~python
type(1)(4.0) # = int(4)
(1).__class__(4.0) # = int(4)
~~~



클래스의 type은? **type**

~~~python
type(int) #type
~~~





#### 그 외 instance관련 사항들

- **dir()**: 객체에서 불러올 수 있는 메소드, attribute list

  - 즉, dir을 실행할 수 있다면 그것은 객체

- **vars()**: instance attributes 출력

  - 클래스에 vars를 붙인다면?

    - 클래스의 관점에서 변수/메소드를 출력해준다.

    ![image-20200709144856009](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200709144856009.png)



- shift+tab 시 "Init Signature" = 인스턴스화 할 수 있다는 뜻

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200712133254271.png" alt="image-20200712133254271" style="zoom:50%;" />



- Instance들의 메모리는 다 다르다.

  - 단, instance의 class attribute는 같은 메모리를 참조한다.
    - instance가 class로 변형되어 참조하기 때문
    - instace attribute를 생성하는 순간 다른 메모리로 할당된다.

  ~~~python
  class Door:
      color = 'brown'
      
      def __init__(self, status):
          self.status = status
          
  door1 = Door('closed')
  door2 = Door('closed')
  
  #checking memory
  door1 is door2 #False
  Door is door1 #False
  
  #class attributes
  door1.color is door2.color #True
  Door.color is door1.color #True
  
  #instance attributes 생성
  door1.color = 'blue'
  door1.color is door2.color #False
  
  var(door) # {'status': 'closed', 'color': 'blue'}
  ~~~

  

- 외부에서 class attribute/method 생성 및 변경 가능

  ~~~python
  Door.new = 3 #class attribute 새로 생성
  
  def t(self):
      print('t')
  Door.t = t
  Door.open = t #메소드변경 또한 가능 ('hijacking')
  ~~~

