---
layout: post
title:  "크롤링&스크래핑 실습 - Scraping 편(1)"
subtitle: "크롤링&스크래핑 실습 - Scraping 편(1)"
categories: inforetrieval
tags: inforetrieval
comments: true
---

- 크롤링의 기본 개념을 알아보자.

  

- 스크래핑을 할 때 필요한 모듈 및 함수들을 알아보자.

  - urllib 라이브러리
  - requests 라이브러리

---

### 스크래핑 관련 함수

#### [builtwith, whois 라이브러리]

- builtwith : 사용된 웹 기술 확인

- whois : 웹사이트 소유자 확인

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling7.png?raw=true" alt="image1" style="zoom:50%;" />

  

#### [urllib 라이브러리]

- **urllib.robotparser**

  robots.txt를 읽고 주어진 봇의 접근권한 여부를 True/False로 출력

  ~~~python
  from urllib import robotparser
  robot = robotparser.RobotFileParser()
  robot.set_url('https://www.google.com/robot.txt')
  robot.read()
  robot.can_fetch('*', 'https://www.google.com') #(agent이름, uri)
  '''
  True
  '''
  ~~~

  

- **urllib.parse** 

  URL주소 처리 관련 

  ※ URL 파라미터는 key, value로 나타내어져 있음

  ~~~
  'https://www.google.com/search?q=%ED%8C%8C%EC%9D%B4%EC%8D%AC&oq=%ED%8C%8C%EC%9D%B4%EC%8D%AC&aqs=chrome..69i57j0l2j69i59j0j69i61l3.2641j0j7&sourceid=chrome&ie=UTF-8'
  # URI 뒤에 ?로 분리되어 있으며, 각 key/value는 &로 분리되어있음
  
  params = {
  'q' : '%ED%8C%8C%EC%9D%B4%EC%8D%AC',
  'oq' : '%ED%8C%8C%EC%9D%B4%EC%8D%AC',
  'aqs' : 'chrome..69i57j0l2j69i59j0j69i61l3.2641j0j7',
  'sourceid': 'chrome&ie=UTF-8'
  }
  ~~~

  

  

  1. <u>urllib.parse.urlparse</u> - breaks string into 6 parts

     <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling8.png?raw=true" alt="image1" style="zoom:50%;" />

  2. <u>urllib.parse.quote /.quote_plus / .unquote</u> - 퍼센트코드 변환 관련

     quote vs. quote_plus = 띄어쓰기를 %20로 표현하거나 +로 표현해줌 => 사이트 별로 다르니 잘 확인해야 함

     <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling9.png?raw=true" alt="image1" style="zoom:50%;" />

  3. <u>urllib.parse.urlencode</u> - 파라미터 주어질 때 key=value꼴로 변환

     <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling10.png?raw=true" alt="image1" style="zoom:50%;" />

  4. <u>urllib.parse.urljoin</u> - URI + 파라미터를 합쳐 전체 주소 생성

     <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling11.png?raw=true" alt="image1" style="zoom:50%;" />

     

- **urllib.request**

  <u>urlopen함수</u> 통해 웹에 요청 보내고 HTTPResponse를 리턴값으로 받음

  ~~~python
  from urllib import request
  
  resp = request.urlopen('https://www.google.com') 
  
  print(resp.geturl()) 
  print(resp.code) 
  print(resp.reason) 
  print(resp.getheaders()) 
  '''
  https://www.google.com
  200
  OK
  [('Date', 'Sun, 26 Jul 2020 12:39:00 GMT'),...('Content-Type', 'text/html; charset=ISO-8859-1'), ...]
  '''
  ~~~

  ~~~python
  body = resp.read()
  body #bytestring 형태의 html 내용물 출력
  body.decode('UTF-8') #str으로 변환
  ~~~

  request.urlopen('주소')의 경우, 무조건 get방식으로 가져옴 -> post방식 사용할 땐 <u>requests.Request</u> 사용해 구체적으로 명시

  ~~~python
  url = 'http://httpbin.org/post'
  params = {
      'key':'value',
      'name':'한글'
  }
  
  resp = urlopen(request.Request(url=url,data=urlencode(params).encode('utf8'),
                                 method='POST')) #param넘김
  print(resp.headers)
  print(resp.read().decode('utf8')) #성공적으로 출력
  ~~~

  

- **urllib.response**

  urllib.request로부터 internally 많이 쓰임



- **urllib.error**

  defines exception class raised by urllib.request

  ~~~python
  from urllib.error import HTTPError
  
  try:
      resp = request.urlopen('https://www.google.com/search?q=파이썬')
  except HTTPError as e:
      print(e.code, e.reason, e.headers)
  
  '''
  403
  Forbidden
  Content-Type: text/html; charset=UTF-8
  Date: Sun, 26 Jul 2020 13:03:18 GMT...
  '''
  ~~~

  forbidden의 경우 사람이 아닌 로봇이 접근해서 발생하는 경우가 대부분 -> user agent를 변경해준다