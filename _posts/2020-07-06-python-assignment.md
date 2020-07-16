---
layout: post
title:  "파이썬 할당문"
subtitle: "파이썬 할당문"
categories: programming
tags: python
comments: 파이썬 할당문의 종류
---

- " □ = △ "의 여러가지 종류
  - 할당문 (Assignment)에 대해 알아보고자 한다


---


### "Assignment" Types in Python
※ 파이썬에는 상수가 없다

- 상수란 : 재할당 할 수 없는 값
  - ex) java 내의 'const' 

- 물리적으로 상수값을 할당할 수 있는데, 모든 문자를 대문자로 작성 시 가능

##### 1) 값 대입문 (general case)

~~~python
a=1
~~~



**2) a=b=1**

~~~python
#immutable case
a=b=1
b=2
print(a) #1

#mutable class
c=d=[1,2,3]
d.append(4)
print(c) #[1,2,3,4]
~~~

- mutable과 immutable에 따라 다름
  - mutable의 경우, c와 d가 같은 메모리를 공유
  - mutable ex) set, list, dict



**3) x, y = 1, 2**

case1

```python
x, y = 1, 2
x, y = y, x
print(x, y) #2 1 출력
```

case2 - error

~~~python
x, y, z = 1, 2 #unpack error 발생 (수 일치해야 함)
~~~

case3

~~~ python
x=1,2,3,4 #(1,2,3,4) tuple 반환

x= 1, 
print(x) #(1,) 출력 - 하나로 이루어진 tuple
~~~



##### 4) x, *y  = 1, 2, 3, 4

~~~python
x, *y = 1,2,3,4
print(x, y) #1 [2,3,4]

*x, y, z = 1,2,3,4
print(x, y, z) #[1,2], 3, 4
~~~

- '나머지 할당' => list 출력

~~~python
*x = 1,2,3,4 #error - 나머지가 없음

#comma(,)를 붙인다
*x, = 1,2,3,4 
print(x) #[1,2,3,4] 출력 
~~~



**5) 증감**

~~~python
x+=1
~~~

- 파이썬 내 ++x는 존재하지 않음
  -  에러는 안나지만 +1의 개념이 아니라, positive sign + x를 뜻한 것 



**6) nonlocal / global** 



**7) 유사할당**

~~~python
x = [k for k in range(10)]
print(k) #error - k는 할당되지 않음
~~~

- for문의 경우 k가 메모리로 할당되지만 (for k in range(10): print(k))
- 유사할당의 경우 **k가 메모리로 할당되지 않음**



**8) as smth**

~~~python
with open('a') as f 
~~~

- as f : f에 할당