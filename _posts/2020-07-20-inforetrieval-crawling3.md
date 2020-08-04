---
layout: post
title:  "크롤링&스크래핑 실습 - Scraping 편(2)"
subtitle: "크롤링&스크래핑 실습 - Scraping 편(2)"
categories: inforetrieval
tags: inforetrieval
comments: true
---

- 이전 포스트에서 download()라는 스크래핑 함수를 생성했다.

- 스크래핑 함수를 이용하여 다양한 사이트의 소스를 가져와보고, 여러가지 상황에 어떻게 대처할 수 있는지 알아보자.
  - REST API 형태
  - dom 형태

---

download() 함수

~~~~python
from urllib.robotparser import RobotFileParser
from requests import request
from requests.compat import urlparse, urljoin
from requests.exceptions import HTTPError
import time
import random

def canfetch(url, agent='*', path='/'): 
	r = urljoin(url, '/robots.txt')
    robot = RobotFileParser(r)
    robot.read()
    return robot.can_fetch(agent, urlparse(url)[2]) 

def download(url, params = {}, headers = {}, method='GET', limit=3):
    if canfetch(url) == False:
        print('[Cannot Fetch] '+url)
    try:
        resp = request(method, url,
                      params=params if method.upper()=='GET' else {},
                      data=params if method.upper()=='POST' else {},
                      headers=headers)
        resp.raise_for_status()
    except HTTPError as e:
        if limit > 0 and e.response.status_code >=500: 
            time.sleep(random.random()*5+1)
            resp = download(url, params, headers, method, limit-1)
        else:
            print('[{}]'.format(e.response.status_code) + url)
            print(e.response.status_code)
            print(e.response.reason)
            print(e.response.headers)
   return resp
~~~~



### 1. REST API 형태 사이트

**a. <u>사이트 내용</u>이 별개의 json/xml형태로 이루어진 경우**

예시 ) 고용노동부 검색결과 가져오기 (http://www.moel.go.kr/local/seoul/search.do)

- 우선 개발자도구의 search.do파일의 Preview/Response 부분 확인한 결과, 내용이 들어있지 않다는 것을 알 수 있다.

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling12.png?raw=true" alt="image1" style="zoom:50%;" />

이러한 경우 DB에 있는 내용과, html 틀을 따로 관리하고 있는 상태이다. 진짜 내용은 주로 **json, xml**에 위치하고 있다.

- file type: **xhr = 데이터를 분리해서 저장해 놓은 부분**

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling13.png?raw=true" alt="image1" style="zoom:50%;" />

보아하니, search_json.jsp 에 내용이 모두 저장되어 있다.

<u>사이트 표시 flow</u>

1. 가장 먼저 search.do가 호출된 뒤, 각종 js가 호출이 됨

2. css, js등 통해 이미지와 그래픽을 다 준비한 뒤에,

3. json 파일이 호출되어 쿼리를 통해 DB에서 내용을 가져옴

plus) search_json 헤더의 referer가 search.do임을 보아 search.do가 json을 불러왔음을 알 수 있음



So, xhr에서 데이터를 가져와보자.

~~~python
url = 'http://www.moel.go.kr/searchapi/search_json.jsp'

params = { 					#Headers의 Form Data부분=params
    'query': '',
    'prefixQuery': 'mfaq',
    ...
    'listCount': 3,
    'sdate': '2014-01-01',
    'edate': '2020-07-17'
    }

params['query'] = '최저임금'

resp = download(url, params, headers, 'POST')
~~~

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling14.png?raw=true" alt="image1" style="zoom:50%;" />

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling15.png?raw=true" alt="image1" style="zoom:50%;" />



원하는 내용을 가져왔음을 알 수 있다.



**b. form 태그의 input query가 jquery로 넘겨지는 경우**

http://example.webscraping.com/places/default/search 를 scrap 해보자.

웹사이트는 이렇게 생겼다.

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling16.png?raw=true" alt="image1" style="zoom:50%;" />

코드상에서 input값을 집어넣어 검색을 하고, 그내용을 가져오는 건 어떻게 할까?

- js확인 결과, input 입력 시 search.json파일로 넘겨짐을 알 수 있다. 따라서 json파일에 params로 입력값을 넣을 것이다.

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling17.png?raw=true" alt="image1" style="zoom:50%;" />

- OR another tip : 네트워크를 열어두고, 제출 버튼 누를 때 추가되는 파일(주로 xhr파일)이 있는데, 얘가 data 가져오는 애로 볼 수 있다.



search.json파일 헤더 정보

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling18.png?raw=true" alt="image1" style="zoom:50%;" />

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling19.png?raw=true" alt="image1" style="zoom:30%;" />

ajax로 넘기는 url form은 다음과 같다.

~~~python
ajax = '/places/ajax/search.json'

params = {
    'search_term' : 'korea',
    'page_size' : 10,
    'page' : 0
}

nurl = urljoin(url, ajax)
resp = download(nurl, params = params)
~~~

json형태의 resp 결과 출력

~~~python
resp.json()
'''
{'records': [{'pretty_link': '<div><a href="/places/default/view/North-Korea-165"><img 	
			   src="/places/static/images/flags/kp.png" /> North Korea</a></div>',
   			  'country': 'North Korea',
   			  'id': 6732849},
  			  {'pretty_link': '<div><a href="/places/default/view/South-Korea-211"><img 				src="/places/static/images/flags/kr.png" /> South Korea</a></div>',
   				'country': 'South Korea',
   				'id': 6732895}],
 'num_pages': 1,
 'error': ''}
'''
~~~





### 2. HTML 형태 - DOM활용

반대로 HTML에 데이터가 존재한다면, **BeautifulSoup**을 활용해 <u>태그를 기반</u>으로 내용을 가져올 수 있다.

- bs는 HTML이나 XML을 parsing하여 **DOM구조의 데이터를 리턴**한다.

  - 파서 종류

    - html.parser (추천) : 내장된 패키지로서 별다른 패키지의 도움 필요 X

    - lxml (추천): 빠름
    - lxml-xml, xml : xml parser
    - html5lib  : 느리나 완성도↑

  - 파서들은 짝이 맞지 않는 태그들에 대해서도 수정해줌

- DOM이란 : 문서의 객체 기반 표현 방식 ==> "노트 트리" 형식

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling20.png?raw=true" alt="image1"/>

​											따라서 태그의 부모노드, 자식노드, 형제노드로 호출 가능하다.



- **dom.prettify()**

  엇갈린 태그도 짝에 맞게 정리해줌 (웬만하면)

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling21.png?raw=true" alt="image1"/>

- **tag호출방법1 : attribute 방식**

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling22.png?raw=true" alt="image1"/>

  - 단점 : 가장 먼저 보이는 것만 찾음

- **tag 호출방법2 : find(), find_all()**

  - find_all : 원하는 요소 한꺼번에 찾을 수 있음

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling23.png?raw=true" alt="image1"/>

  - **find_parents() / find_sibling()**
    - find_parent : 바로 위 부모
    - find_parents : 부모, 할머니, 증조, 등 전부

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling24.png?raw=true" alt="image1"/>

- **tag 호출방법3 : select(), select_one()**

  - #id
  - .class

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling25.png?raw=true" alt="image1"/>

  - parent > children

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling26.png?raw=true" alt="image1"/>

  - sibling_previous + sibling_to_find

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling27.png?raw=true" alt="image1"/>



**실습**

이제, 실습을 해보자.  http://pythonscraping.com/pages/page3.html 을 scrap한다.

~~~python
resp = download('http://pythonscraping.com/pages/page3.html')
dom = BeautifulSoup(resp.content, 'html.parser')
~~~



[목표1) 테이블 내 두번째 행의 이미지를 가져온다.]

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling27.png?raw=true" alt="image1" style="zoom:50%;"/>

1. 태그가 img인건 알았다. 단, 이 img2.jpg라는 specific한 이미지를 가져와야한다.
2. 우선 부모태그 중, 명확한 id나 class이름을 가진 것을 찾는다.
   - 바로 위 tr태그에 id="gift2"라는 이름으로 되어있다
   - 바로 위 태그에 없다면 이름 나올때 까지 계속해서 부모 태그 찾는다.
3. 해당 부모 태그의 children list 중 해당되는 태그를 indexing
4. attrs를 통해 이미지의 정확한 주소 알아온다.

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling29.png?raw=true" alt="image1" style="zoom:50%;"/>



부모/형제 통해서 태그 찾기

- find_all(recursive=False) 이면 각 태그를 하나씩 출력하도록 한다.(여러 조합이 아니라)

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling30.png?raw=true" alt="image1" style="zoom:50%;"/>



[목표2) 금액 추출]

~~~python
for _ in dom.find('table').find_all(recursive=False)[1:]:
    print(_.find_all('td')[2].text.strip())
    
'''
$15.00
$10,000.52
$10,005.00
$0.50
$1.50
'''
~~~

- text => 태그 제외한 내용 추출

- strip() => 공백 제거





