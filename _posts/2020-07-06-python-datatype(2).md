---
layout: post
title:  "파이썬 데이터타입(2) - Collections"
subtitle: "파이썬 데이터타입(2) - Collections"
categories: programming
tags: python
comments: true
---

- Collections
  - DefaultDict, OrderedDict, NamedTuple, Counter

---

### Python Data Types(2) - Collections

aka. Container

#### [DefaultDict]

디폴트값을 설정해 줄 수 있는 dictionary

키를 지정해주지 않은 모든 키는 불러올 때 default값으로 저장됨

- int의 default =0
- list의 default=[]
- set의 default=set()
- 등등

~~~python
from collections import defaultdict

dfd1 = defaultdict(int)
dfd1 
#defaultdict(<class 'int'>, {})

dfd1['key1'] #0 <-- 에러나지 않고 default값으로 저장한 뒤 불러옴.
dfd1 
#defaultdict(<class 'int'>, {'key1':0})

dfd1['key2'] = 'hi'
dfd1 
#defaultdict(<class 'int'>, {'key1':0, 'key2':'hi'})
~~~

~~~python
from collections import defaultdict

dfd2 = defaultdict(list)

dfd2['key1'] #[]
dfd2 
#defaultdict(<class 'int'>, {'key1':[]})

dfd2['key2'] = 'hi'
dfd2 
#defaultdict(<class 'int'>, {'key1':[], 'key2':'hi'})
~~~

- memoization할 때 사용하면 좋을 것 같음! (default값을 0으로 하고싶을 때)



#### [OrderedDict]

순서가 고려되는 dictionary

- 3.6 이전의 파이썬에서는 dictionary의 데이터 입력이 무작위였음
  - 이를 보완하고자 고안된 기능이 OrderedDict --> 입력한 순서대로 저장
  - 3.6이상 파이썬은 입력한 순서대로 저장되기 때문에 OrderedDict가 잘 안쓰이긴 하나, 하위호환을 위해 사용하면 좋음
- 사용법1) **두 dictionary의 원소+순서까지 동등성**을 비교할 때.

~~~python
#일반 Dict
dict1 = {'A':1, 'B':2}
dict2 = {'B':2, 'A':1}
dict1 == dict2 #True

#OrderedDict
from collections import OrderedDict
dict1 = OrderedDict({'A':1, 'B':2})
dict2 = OrderedDict({'B':2, 'A':1})
dict1 == dict2 #False
~~~

- 사용법2) **popitem()**
  - key, value를 함께 pop.
  - last=True / False에 따라 뒤에서 pop 할지, 앞에서 pop 할지 결정

~~~python
k,v = dict1.popitem(last=False)
print(k,v) #A 1
~~~

- 사용법3) **move_to_end**
  - 원소를 맨끝(last=True일때)/맨앞(last=False일때)으로 이동

~~~python
dict2.move_to_end('B', last=True)
dict2 #OrderedDict([('A', 1), ('B', 2)])
~~~



#### [NamedTuple] 

기존의 튜플과 다르게, **튜플에 이름**이 있고, **키워드방식으로 값을 불러올 수 있음**

~~~python
#일반 tuple
tup = (3,5)
tup[0] #3
tup[0] = 1 #error

#namedtuple
from collections import namedtuple
ntup = namedtuple('Point', ['x','y'])
print(ntup) #<class '__main__.Point'>

p = ntup(x=3, y=1)
p[0] #3
p.x #3
p.x = 1 #error

~~~

- 클래스로 생성됨

- named<u>tuple</u>이기 때문에 immutable이며, 튜플 기능 사용 가능

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/datatype1.png?raw=true" alt="image-20200709093732192" style="zoom: 50%;" />

- 얘네도 namedtuple



#### [Counter]

- Sequence를 입력값으로 받고, 원소의 개수를 count해줌
- dict형태로 출력 - mutable

~~~python
from collections import Counter
t = Counter('abcaa') 
t 						#Counter({'a': 3, 'b': 1, 'c': 1})
t.most_common() 		#[('a', 6), ('c', 4), ('b', 3)]

t = Counter((3,4,4,2))
t 						#Counter({3: 1, 4: 2, 2: 1})
~~~

