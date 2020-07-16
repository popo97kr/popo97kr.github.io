---
layout: post
title:  "memoization"
subtitle: "memoization"
categories: programming
tags: algorithm
comments: true
---

- memoization
  - code with & without decorator

---

### Memoization

- **캐싱**을 이용해 알고리즘의 성능을 개선하는 방법
- 반복적으로 같은 반환값을 불러오고자 할 때, 이를 리스트에 저장해두고 추가 연산 없이 가져오게 할 수 있음



#### **ex) 피보나치 함수**

본래 재귀함수를 사용한 경우, 

~~~
fibonacci(2) = fibonacci(1)+fibonacci(0)

fibonacci(3) = fibonacci(2)+fibonacci(1)
   			  = fibonacci(1)+fibonacci(0)+fibonacci(0)
   			  
fibonacci(4) = fibonacci(3)+fibonacci(2)
			 = fibonacci(2)+fibonacci(1)+fibonacci(2)
			 ...
			 = fibonacci(1)+fibonacci(0)+fibonacci(1)+fibonacci(1)+fibonacci(0)
~~~

로 계산해야 하지만, 

**memoization 사용시,** 

~~~
fibonacci(2) = fibonacci(1)+fibonacci(0) ==>기억
fibonacci(3) = fibonacci(2)+fibonacci(1) ==>기억
fibonacci(4) = fibonacci(3)+fibonnaci(2) ==>기억
...
~~~

으로 기억리스트에서 바로 꺼내와 계산시간을 현저히 줄일 수 있음



#### [CODE]

1) 피보나치 using memoization **without decorator**

~~~python
cache = {}

def fibo(n):
    if n in cache:
        pass
    elif n<2:
        cache[n]=n
    else:
        cache[n] = fibo(n-1)+fibo(n-2)
    
    return cache[n]
        
fibo(6) #8
~~~



2) 피보나치 using memoization **with decorator**

~~~python
def memoize(fn): #다른곳도 적용가능
    cache = {}
    def inner(*args):
        if args in cache:
            return cache[args]
        else:
            cache[args]=f(*args)
            return cache[args]
     return inner

@memoize
def fibo(n): #기존 피보나치 재귀함수로 사용 가능
    if n<2:
        return n
    else:
        return fibo(n-1)+fibo(n-2)

fibo(6) #8
~~~

