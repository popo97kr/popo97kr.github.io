---
layout: post
title:  "하노이 탑"
subtitle: "하노이 탑"
categories: programming
tags: algorithm
comments: true
---

- 재귀함수를 활용한 하노이 탑 풀이 

---

### 하노이 탑 (hanoi tower)

재귀함수를 활용한 가장 유명한 예제 중 하나인 하노이 탑을 풀어보도록 한다.

학과 알고리즘 수업에서 분명히 배웠던 문제인데, 세월이 흐르며 잊어버리게 되었다..이제부터라도 잊어먹지 않고 놓쳤던 알고리즘 공부를 꾸준히 해야겠다는 생각을 했다.



배경) 3개의 막대기, 크기 순서대로 쌓여진 원판들. 

- 한번에 하나씩 다른 막대기로 옮길 수 있으며,
- 원판은 항상 위의 것이 아래보다 작아야 한다. 

- **최종 목표 : 최소 이동을 통해 1번->3번 막대기로 모든 원판을 옮긴다.**

![image1](https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/hanoi1.jpg?raw=true)



알고리즘)

우선 1개를 예시로 생각해보자.

원판이 한개 있을 경우, 1->3으로 바로 옮길 수 있다. 



3개를 예시로 생각을 해보자.

![image2](https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/hanoi2.jpg?raw=true)

문제는 크게 세 파트로 나눌 수 있다.

1. 위 2개의 원판을 1->2로 옮긴다 (3은 보조 역할을 한다).

2. 가장 큰 원판을 3으로 옮긴다.

3. 그렇게 옮긴 2개의 원판을 2->3으로 옮긴다 (1은 보조 역할을 한다).

   

이를 확장시켜,

n개의 원판이 있을 때, n-1개의 원판을 1->2로, 그리고 2->3으로 옮기는 방식으로 풀 수 있다.

n-1개의 원판을 옮기는 과정 안에서, n-2개의 원판을 옮기고, 또 그 안에서 n-3개의 원판을 옮기고... 이렇게 재귀적으로 풀어나갈 수 있다.



파이썬 코드로 나타내면 다음과 같다. 

~~~python
#이동순서 출력
def hanoi(n, fromm, to, aux):
    if n==1:
        print(fromm,to)
        return None
    hanoi(n-1, fromm, aux, to)
    print(fromm,to)
    hanoi(n-1, aux, to, fromm)

hanoi(3, 1,3,2)
'''
1 3
1 2
3 2
1 3
2 1
2 3
1 3
'''
~~~

