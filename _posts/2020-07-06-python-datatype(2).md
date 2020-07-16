---
layout: post
title:  "파이썬 데이터타입(2)"
subtitle: "파이썬 데이터타입(2)"
categories: programming
tags: python
comments: true
---

- Collections
  - defaultdict, ordereddict, namedtuple

---

### Python Data Types(2) - Collections

aka. Container

- **defaultdict**

- **ordereddict**

- **namedtuple** 

  - class 를 생성

  ~~~python
  x = namedtuple('Point', ['x','y']) #Point라는 클래스 생성
  print(x) #<class '__main__.Point'>
  type(x) #type
  ~~~

  - 클래스의 type은 type으로 출력됨

  ~~~python
  p = Point(x=3, y=1) #인자를 받음
  print(p.x, p.y) #3 1
  
  p[0] #3 출력
  ~~~

  - named 'tuple'이기 때문에 sequence 기능 사용 가능

  ![image-20200709093732192](https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/datatype1.jpg?raw=true)

  - 얘네도 namedtuple

- **Counter** - 유용

~~~python
from collections import Counter
t = Counter('abcaa') 
t # Counter({'a': 3, 'b': 1, 'c': 1})
t.most_common() #[('a', 6), ('c', 4), ('b', 3)]
~~~

- dict형태로 출력 - mutable