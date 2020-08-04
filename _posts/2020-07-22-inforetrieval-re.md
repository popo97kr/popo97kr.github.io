---
layout: post
title:  "정규식"
subtitle: "정규식"
categories: inforetrieval
tags: inforetrieval
comments: true
---

- 정규식 = 비정형데이터로부터 특정 텍스트를 탐색하기 위한 방법

---

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/re.png?raw=true" alt="image1" style="zoom:50%;" />



- **.  = any one character**

  - .x : 2글자인데 x로 끝남

  

- **\* = 0 or more** 

  - x* : x가 없거나 한번이상 반복

- **\+ = 1 or more**

  - x+ : x가 한번 이상 반복(x, xx, xxx, ..)

- **? = 0 or 1**

  - https? : http or https

- **{n,m} = n에서 m개**

- **{n,} = n개 이상**

  

- **^ = start of line**
- **$ = end of line**

ex) (^[1-9]{1}$|^[1-4]{1}[0-9]{1}$|^50$) : 1이상 50이하인 숫자



- **[] = checking any single character**

  - [http] : h | t | p 중 하나

  - [http]{3,4} : h | t | p 인 3글자 or 4글자 (ttph, thp,htpp)

  - [A-Z], [a-z], [ㄱ-ㅎ], [ㅏ-ㅣ], [가-힣] 

    - ㅋㅋㅋ,ㅎㅎ등 없이 문자만 가져온다 하면 [A-Za-z가-힣]+

  - **[^~] : not**

    - [^htp] : h,t,p가 아닌 모든 문자 하나

    

- **\b : 공백사이의 문자**

  - \b[a-z]{3}\b : 소문자 3자로 이루어진 한 단어 (adn, enr, ..)

- **\B : 공백사이 문자 X**

  - \B[a-z]+\B : 공백이 아닌, 양옆 한문자 사이에 있는 소문자(들) (F<u>il</u>e, I<u>nser</u>t, 3<u>thousand</u>s..)

- **\w : any word**, **\W : not a word**

- **\s : 공백, \S : 공백 X**

- **\d : 숫자, \D : 숫자 X**

  

- **.* : greedy vs. .*? : lazy**

  - greedy : 마지막으로 매치되는 것까지

    ​	s.*o => **stackoverflo**w

  - lazy : 제일 처음 매치되는 것까지

    ​	s.*?o => **stacko**verflow

    

- **() : 그룹핑**



#### re 라이브러리

- re.compile : 패턴 만들어줌
- re.search : 만든 패턴을 글 내용에서 찾아줌
- re.findall : 전부 다 찾아줌
- re.sub : 패턴 대체



**[examples]**

~~~python
#주민번호 뒷자리 변경하기
import re

data = '''
park 800908-1234567
kim 700808-1483291
'''

pattern = re.compile(r'(\d{6})[-](\d{7})')
pattern.search(data)
#<re.Match object; span=(6, 20), match='800908-1234567'>

pattern.search(data).groups()
#('800908', '1234567')

pattern.sub('\g<1>-*******', data)
'''
park 800908-*******
kim 700808-*******
'''
~~~

~~~python
re.search(r'(ABC)+', 'ABCABCA A').group()
#'ABCABC'

re.search(r'\bclass\b', 'the declassified algorithm') 
#공백으로 감싸지지 않아서 못찾음 
re.search(r'\Bclass\B', 'the declassified algorithm') 
#<re.Match object; span=(6, 11), match='class'>
~~~

~~~python
re.search(r'(\w+) (\w+)', 'Isaac Newton, physicist').groups() #앞에 두단어 찾기
#('Isaac', 'Newton')
~~~

~~~python
#핸드폰 번호 수집
data1 = '010-0000-0000'
data2 = '010 0000 0000'
data3 = '010.0000.0000'
data4 = '01000000000' #이 모든 것을 찾을 수 있도록

pattern = re.compile(r'(\d{3})[-. ]?(\d{4})[-. ]?(\d{4})')
pattern.search(data_)
~~~

~~~python
#이메일 주소 수집
data = '''
    test1@test.com
    test2@test.co.kr
    test3@email.test.com
    12test@test.com			#첫글자숫자X
    test4@email.test.co.kr
    test@test				#X
'''

re.findall(r'\b([A-Za-z][A-Za-z0-9-_.]+@[A-Za-z0-9]+(?:[.][A-Za-z0-9]+)+)', data)
#

'''
['test1@test.com',
 'test2@test.co.kr',
 'test3@email.test.com',
 'test4@email.test.co.kr']
'''

~~~

- **?:** = non capturing group -> 해당 그룹 출력하지 X