---
layout: post
title:  "else의 용도"
subtitle: "else의 용도"
categories: programming
tags: python
comments: true
---

파이썬 내 else의 용도 3가지

Python은 Multiparadigm Language이다.

---


### Usage of else
**1. if else**

**2. 반복문 뒤**

~~~python
for i in range(5):
    print(i)
else:
    print('end')
~~~

- 반복문이 잘 돌아갔는지 확인하기 위해 사용
  - 중간에 break로 반복문 빠져나올 시엔 else부분 실행X

**3. try/except** 

- 특히 크롤링 시 유용

~~~python
try:
    a=1/0
except ZeroDivisionError:
    print('divided zero')
except Exception as e:
    print(e)
else:
    print('no error')
finally:
    print('always')
~~~

- error 나지 않을 때 else부분 실행



