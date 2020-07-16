---
layout: post
title:  "iterator"
subtitle: "iterator"
categories: programming
tags: python
comments: true
---

Iterable과 Iterator에 대해 알아본다.

---

### Iterator/Iterable

- **for 구문에 쓰일 수 있음**

- iterable = iterator가 될 수 있는 것들

  - dir했을 때 **\__iter__** 존재한다 = iterator로 바꿀 수 있다
  - iter 단점 : sequence 의 의미를 잃어버림 - indexing 할 수 X

  ~~~python
  a = [1,2,3]
  dir(a) #__iter__있음
  
  b = iter([1,2,3])
  dir(b) #__iter__ 와 __next__ 있음
  type(b) #list_iterator
  ~~~

- **iterator&generator는 next를 사용할 수 있음**

  - next = memory에 한꺼번에 안올리고 하나씩 올림 - 기존 리스트에선 지워짐 - 이는 파일크기큰 이미지 처리 시 유용

    - ex. iter(range(10))후 dir보면 \__next__ 존재

    - next 계속 했을 시 크기 초과 : StopIteration error 발생
      - 이는 try except문 통해 막을 수 있음

  ~~~python
  next(b) #1
  list(b) #[2,3]
  
  next(b)
  next(b)
  next(b) #StopIterationError
  ~~~

- 반복문 (ex. for i in [1,2,3]) : 내부적으로 iterator로 변신시킴

- enumerate = iterator

  ~~~python
  for n, i in enumerate([1,2,3,4]):
      print(n,i)
  ~~~

  - unpacking돼서 (n,i)에 집어넣음

  



