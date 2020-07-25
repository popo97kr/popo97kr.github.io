---
layout: post
title:  "Functional Paradigm(4) - Itertools"
subtitle: "Functional Paradigm(4) - Itertools"
categories: programming
tags: python
comments: true

---

Iterator Protocol, itertools, Lazy Evaluation

---

### Functional Paradigm(4)

#### [The Iterator Protocol]

- Iterator라면 반드시 가져야 할 메소드들  = iterator protocol

  - 즉, **iter**와 **next**
  - iterator, iterable에 관한 정보는 [여기](https://popo97kr.github.io/programming/2020/07/07/python-iterator/) 참고

  ※ **iterable 상속 받지않고 protocol만 가지고 있으면 iterator처럼 활용할 수 있음** (duck typing)

  - 즉, dir 안에 'iter '와 'next'있으면 iterator로 부른다

~~~python
'__iter__' in dir(lazy) and '__next__' in dir(lazy)
~~~



#### [itertools 라이브러리]

- **itertools.cycle** => 무한루프 돌아서 iterable 내 원소 계속 순환

~~~python
from itertools import cycle
a = cycle([1,2,3])
next(a) #1,2,3,1,2,3,....
~~~



- **itertools.count** => start 부터 해서 step만큼 증가시킴

~~~python
from itertools import count
a = count(10,2)
next(a) #10,12,14,....
~~~



위 두개 : while문 사용 - stopIteration error 발생 X

-  while은 객체지향에 어울리지 않아 잘 쓰이지 않으나 무한루프 돌릴 때 사용함



- **itertools.combination** => sequence 내 n개 갯수 combination 출력

~~~python
from itertools import combinations
a = combinations([1,2,3],2)
dir(a) #iter, next존재
next(a) #(1,2), (1,3), ... (2,3)
~~~



- **itertools.permutation** => permutation 출력

~~~python
from itertools import permutations
a = permutations([1,2,3],2)
next(a) #(1,2)...(1,2),...(3,2)
~~~



- **itertools.accumulate** => 누적합

~~~python
from itertools import accumulate
a = accumulate([1,2,3])
list(a) #[1,3,6]
next(a) #1,3,6
~~~

+) numpy 에서도 누적합 구하기 가능 : **np.cumsum**

~~~python
a = np.array([1,2,3])
np.cumsum(a) #[1,3,6]
~~~



#### [Lazy Evaluation/Execution]

- Iterator에서 **next 부르는 순간**에만 연산 하는것

  - next가 불러와져야 메모리에 올려놓는다

  (<->) eager execution