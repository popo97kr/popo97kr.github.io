---
layout: post
title:  "Dynamic Programming"
subtitle: "Dynamic Programming"
categories: programming
tags: algorithm
comments: true
---

- Dynamic Programming
  - Overlapping Subproblems
  - Optimal Substructure - Memoization, Tabulation

---

### Dynamic Programming

부분문제를 통해 주어진 문제를 단계적으로 해결하는 방법

다이나믹 프로그래밍 문제의 특성

​	**1) Overlapping Subproblems**

​		= 큰 문제를 작은 문제로 쪼갤 수 있다.

​		ex) 피보나치 : f(n) = f(n-1) + f(n-2)

​			  이항계수 : nCr = n-1Cr + n-1Cr-1

​	**2) Optimal Substructure**

​		= 큰 문제의 정답을 작은 문제의 정답에서 구할 수 있다

​		따라서, 작은 문제의 정답을 따로 저장해두고, 필요할 때 바로 꺼내쓴다. (캐싱)

​        	방법1) Memoization (Top-Down)

​			방법 2) Tabulation (Bottom-Up)

##### 

#### Memoization

Top-Down solution for DP

- 재귀적인 방법과 유사하나, 재귀 시행하기 전에 캐시를 lookup

- 장점 : 필요한 것만 저장한다.

~~~python
cache = {} #dict 사용

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

메모이제이션 데코레이터 사용 및 더 자세한 사항은 [여기](https://popo97kr.github.io/programming/2020/07/17/algorithm-memoization/) 참고



#### Tabulation

Bottom-Up solution for DP

- n=1 부터 시작해 모든 subproblem의 답을 저장해놓는 방법
- 구하고자 하는 n번째 원소를 리턴 

~~~python
lst = []

def fibo(n):
    if n==0 or n==1:
        return n
    else:
        return lst[n-1]+lst[n-2]
    
for _ in range(6+1):
    lst.append(fibo(_))
    
lst[6] #8
~~~



※ Overlapping subproblems 이지만, optimal substructure이 아닌 예) 최장 단순 거리 구하기

<img src="https://t1.daumcdn.net/cfile/tistory/991256335A10029801" alt="img" style="zoom: 67%;" />

q에서 t로 가는 최장단순거리 : q->r->t 는 q->r / r->t 로 쪼갤 수 있으나, 

각 부분문제의 최장단순거리(q->r : q->s->t->r / r->t : r->q->s->t)로 문제의 정답을 구할 수는 없다.

