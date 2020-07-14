---
layout: post
title:  "higher order function"
subtitle: "higher order function"
categories: programming
tags: python
comments: true
---

- map, filter, reduce

---


### Higher Order Function
- 함수를 인자로 받고 함수를 리턴하는 함수
- 함수형 패러다임 중 하나
  - 값을 동시에 변경할 수 있음

#### [**map**]

~~~python
def x(t):
    return t+1
list(map(x,[1,2,3,4,5]))

list(map(lambda x: x+1, [1,2,3,4,5]))
~~~

- map(func, *iterables)

- sequence 내 원소를 함수에 적용

  - 내부적으로 iter를 통해 list를 iterator로 돌려줌

- pandas, tf에 모두 존재

  

#### [**filter**]

~~~python
list(filter(lambda x: x>3, [1,2,3,4,5])) #[4,5]
~~~

- True 인 값만 리턴

- filter(predicate, iterable) 

  - predicate : true/false를 반환하는 함수

    

#### [**reduce**]

~~~python
from functools import reduce
reduce(lambda x,y : x+y, [1,2,3,4,5]) #15
~~~

- 여러개를 하나의 **대표 수**로 표현
  - sum, multiplication, mean, ...
- reduce(function, sequence[, initial]) -> value 
  - []부분은 option
  - -> 부분은 recommendation (type을 이렇게 써주면 좋겠다는 뜻)
- numpy,tf 에도 reduce 존재
  - np.add.reduce([1,2,3,4])
  - tf.math.reduce_sum([1,2,3,4])

