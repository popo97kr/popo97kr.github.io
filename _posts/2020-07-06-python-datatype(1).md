---
layout: post
title:  "파이썬 데이터타입(1)"
subtitle: "파이썬 데이터타입(1)"
categories: programming
tags: python
comments: true
---

- Numeric
- String
- Sequence - list, set, dictionary, tuple, string

---

### Python Data Types(1)

- Literal(리터럴)이란? 
  - Data type 정의 표기



#### [Numeric]

**1. integer**

- n진수 표기

~~~python
a = 0b10 #2진수
b = 0o47 #8진수
c = 0xa8 #16진수
~~~

- 여기서 0b, 0o, 0x는 각각 리터럴
  - 십진수의 리터럴은 존재 X
- 숫자 사이에 underbar 표시 가능 (실제 출력시엔 무시됨)
  - 100,000 나 0b_11011 와 같이 구분을 위해 표시

**2. float**

- 부동소수점이 존재 - **정확한 값을 구현할 수 X**

  - 그러나 ML/DL의 경우 exact value를 예측하지 않기 때문에 굳이 큰 상관없음

- 리터럴 2가지

  - .2
  - 3e4

- 정수보다 압도적으로 빠름

- infinite 의 type은 float

  - inf로 표기

    ~~~python
    float('int') #무한대 선언
    float('nan') #NAN 선언
    ~~~

**3. complex**

- 복소수(데싸에 전혀사용X)

**4. boolean**

- 0/1로 이루어짐
- int 클래스를 상속받은 형태
  - 따라서 int에서 사용되는 모든 함수가 수용됨
    - ex. True+True 시 2 출력



#### [String]

**1.  string**

- 리터럴 : u"" 
  - 이지만 생략하여 사용 

**2. Bytes**

~~~python
a = b"하이"
~~~

**3. Bytearray**

- 한정된 메모리 사용
- ex. beautiful soup

~~~python
b = bytearray(b"하이")
~~~



#### [Sequence]

- Container (or Collection) 하위 type

  - "순서"가 중요한 container

- indexing과 slicing을 지원

  - indexing 시 sequence의 크기 이상 출력 시 **index error** 발생
  - slicing은 에러나지 X

  ~~~python
  a=[1,2,3]
  a[4] #index error
  
  a[1:1000] #[1,2,3]출력
  a[slice(1,1000)]
  
  range(10)[3:100]
  ~~~

- **비교** 가능 

  ~~~python
  'a'>'b' #False
  'a'>'Aa' #True
  
  [1,2,3]>[4,5,6] #False
  ~~~

  - 맨 처음 원소를 비교

- **연산** 가능

  ~~~python
  'abc'+'def' #'abcdef'
  (1,)+(2,3) #(1,2,3) 
  (1,)*3 #(1,1,1)
  ~~~

  - 더하기/곱하기 : concatenate
  - 같은 형태의 sequence 끼리만 연산 가능

  ~~~python
  3+'strr' #typeerror 발생
  ~~~

- sequence들 기능 모두 같은 이유 = 같은 class에서 상속받기때문



**sequence 분류1 : homogeneous/heterogeneous**

- hetero : 다른 데이터 타입 수용 가능
  - list, set
- homo : 한가지 데이터 타입만
  - string

**sequence 분류2 : mutable/immutable**

- mutable : 자기 자신 변형이 가능
  - ex. bytearray,list,set,dictionary
  - 자기변형 함수 example
    - <u>return값 None</u> - append, remove, pd 함수 내 'inplace=True'
    - return값 ㅇ - pop
- immutable : 자기 자신 변형 불가능
  - ex. tuple
    - count,index만 존재(삭제,삽입 함수X)

- mutable / immutable은 항상 paired. (ex. list와tuple은 pair)



#### Sequence 종류

**1. list**

~~~python
b = [1,[2,3],] #trailing comma = 뒤에 더 삽입할 수 있다는 것을 표현
len(b) #2출력
~~~

- heterogenous, mutable

**2. set**

- 중복/순서가 존재하지X

  - indexing 불가능

- heterogenous, mutable

- 리터럴은 {}이나, 공집합을 {}로 생성할 수 X

  ~~~python
  z = set()
  z #set() = 공집합
  ~~~

- list를 집어넣을 수 없음

  ~~~python
  z = {1,[2,3]} #type error : unhashable type
  ~~~

  - 왜? set은 결국 dictionary의 key 부분이기 때문

- 집합연산자

~~~python
a={1,2,3}
b={3,4}
print(a&b) #{3}
print(a^b) #{1,2,4}
~~~

~~~python
#서로 다른 함수의 기능 비교 시에도 사용 가능
set(dir(list()))-set(dir(tuple()))
~~~

**3.dictionary**

- key 특징

  - hashable: 유일하게 하나로 결정되는 값(immutable)으로 배정해야함 	

    - mutable type이 오면 X
    - immutable 내 mutable이 존재해도 X

    ~~~python
    a = {'[1,2,3]':1, 'b':2, 'a':4} 
    #가능
    
    a = {(1,2,3):1, 'b':2, 'a':4} 
    #가능
    
    a = {(1,[2,2],3):1, 'b':2, 'a':4} 
    #error
    ~~~

  - 유일함

    ~~~PYTHON
    a = {'a':1, 'b':2, 'a':4} 
    a #{'a':4, 'b':2} 출력
    ~~~

- key가 중심

  - key 중심으로 len 계산

    ~~~python
    1 in {'a':1, 'b':2} #False
    ~~~

- 'dictionary view' - dict.keys(), dict.values(), dict.items()

  ~~~python
  for i,j in a.items(): #튜플할당 -(i,j)를 i,j로 표시할 수 ㅇ.
      print(i,j)
  ~~~

- 순서가 없음 - key이름으로 indexing 

  ~~~python
  a = {'a':1, 'b':2}
  a[1] #error
  a['a'] #1
  a['c'] #keyerror
  
  #단 키의 값을 추가시켜 keyerror 방지가능
  a['c']=3
  a #{'a': 1, 'b': 2, 'c': 3}
  ~~~

  ~~~python
  xx = 3
  globals()['xx'] #3출력
  ~~~

  - get 함수로 가져올수도 있음 - error 막기 위해 유용

  ~~~python
  a.get('a',3) #key가 없으면 3 반환
  ~~~

- pandas는 dictionary 구조임 - key(column)로 값 집어넣을 수 있는 이유

  - ex) df['temp'] = np.zeros(10)

- heterogeneous, mutable

  - hetero이지만 list를 key 집어넣으면 typeerror
  - immutable pair = frozenset

**4. string**

~~~python
import string

#유용한 모듈들
string.ascii_letters
string.digits
string.punctuation
string.whitespace
~~~



### [Collections]

Python Data Types(2)에 이어서 설명

