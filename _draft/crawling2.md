### Crawling & Scraping 실습 - Scraping 편(1)

#### [builtwith, whois]

- builtwith : 사용된 웹 기술 확인

- whois : 웹사이트 소유자 확인

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200726211016444.png" alt="image-20200726211016444" style="zoom: 40%;" />



#### [urllib 라이브러리]

HTTP를 쉽게 사용할 수 있도록 하는 모듈

--> robotparser, request, response, error, parse 로 구성

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

  URL 처리 관련 함수들 보유

  URL 파라미터는 key, value로 나타내어져 있음

  ~~~
  'https://www.google.com/search?q=%ED%8C%8C%EC%9D%B4%EC%8D%AC&oq=%ED%8C%8C%EC%9D%B4%EC%8D%AC&aqs=chrome..69i57j0l2j69i59j0j69i61l3.2641j0j7&sourceid=chrome&ie=UTF-8'
  
  params = {
  'q' : '%ED%8C%8C%EC%9D%B4%EC%8D%AC&oq=%ED%8C%8C%EC%9D%B4%EC%8D%AC',
  'aqs' : 'chrome..69i57j0l2j69i59j0j69i61l3.2641j0j7',
  'sourceid': 'chrome&ie=UTF-8'
  }
  ~~~

  URI 뒤에 ?로 분리되어 있으며, 각 key/value는 &로 분리되어있음

  

  1. <u>urllib.parse.urlparse</u> - breaks string into 6 parts

     ![image-20200726221113024](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200726221113024.png)

  2. <u>urllib.parse.quote /.quote_plus / .unquote</u> - 퍼센트코드 변환 관련

     quote vs. quote_plus = 띄어쓰기를 %20로 표현하거나 +로 표현해줌 => 사이트 별로 다르니 잘 확인해야 함

     <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200726221610669.png" alt="image-20200726221610669" style="zoom: 33%;" />

  3. <u>urllib.parse.urlencode</u> - 파라미터 주어질 때 key=value꼴로 변환

     <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200726222105619.png" alt="image-20200726222105619" style="zoom: 33%;" />

  4.  <u>urllib.parse.urljoin</u> - URI + 파라미터를 합쳐 전체 주소 생성

     <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200726222503491.png" alt="image-20200726222503491" style="zoom:33%;" />

     

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



#### [requests 라이브러리]

urllib보다 간편함 - **(크롤링할때 거의 얘만 쓴다고 함...)**

- **requests.get / .post / .put / .delete**

  ~~~python
  import requests
  resp1 = requests.post('httpL//httpbin.org/post', data={'key':'value'}) #post방식
  
  resp = requests.get('http://httpbin.org/get?key=한글') #get방식
  ~~~

  - urllib의 경우 urlopen, method 명시, encoding 등 여러 처리가 필요했지만 여기선 필요 X

  ~~~python
  #request header
  resp.request.headers
  
  #response status
  resp.status_code
  
  #response header
  resp.headers
  
  #response body
  resp.content #bytestring형태
  resp.content.decode('utf8') #string형태
  resp.text #string형태 (resp.content.decode('utf8')과 같음)
  
  #response 인코딩 set
  resp.encoding = 'euc-kr'
  ~~~



- **requests.request**

  모든 method에 사용가능. response를 리턴함

  ~~~python
  resp_get = request('GET', 'http://httpbin.org/get', params={}, headers={}) #GET - params를 인자로
  resp_post = request('POST', 'http://httpbin.org/post', data={}, headers={}) #POST - data를 인자로
  ~~~

  

- **requests.compat** - <u>urlparse</u>, <u>urljoin</u> 등 파싱함수 제공
- **requests.response**
- **requests.exceptions,...** - HTTTPError



#### [정리] Scraping 함수 정의

~~~~python
from urllib.robotparser import RobotFileParser
from requests import request
from requests.compat import urlparse, urljoin
from requests.exceptions import HTTPError
import time
import random

def canfetch(url, agent='*', path='/'): #봇이 내용 가져올 수 있는지
	r = urljoin(url, '/robots.txt')
    robot = RobotFileParser(r)
    robot.read()
    return robot.can_fetch(agent, urlparse(url)[2]) #agent이름, uri

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
        if limit > 0 and e.response.status_code >=500: #서버에러나면 limit=0까지 다시 시도
            time.sleep(random.random()*5+1) #랜덤하게 쉰다(로봇인거 티안내게)
            resp = download(url, params, headers, method, limit-1) #recursively
        else:
            print('[{}]'.format(e.response.status_code) + url)
            print(e.response.status_code)
            print(e.response.reason)
            print(e.response.headers)
   return resp
~~~~

~~~python
#실제 스크래핑

url = 'https://www.google.com/search'
params = {
    'q':'',
    'oq':'',
    'aqs':'chrome.0.69i59j0l4j69i61l3.3662j0j7',
    'sourceid':'chrome',
    'ie':'UTF-8'
}

params['q'] = params['oq'] = '파 이 썬'
headers = {
    'user-agent':'Mozilla/5.0 (Wind .... Chrome/84.0.4147.89 Safari/537.36'
} #사람이 한것처럼 user agent 변경 <-- forbidden error 방지
resp = download(url, params, headers, 'GET')
resp.text #html파일을 string 형태로 show
~~~



※ 사이트 method 찾는법 : 개발자도구 > Network > Headers 확인 (RESTful : html아닌 json/xml 확인)

![image2](https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling3.png?raw=true)



[다음 포스트]()에선 위 함수를 사용하여 다양한 경우에 스크래핑 하는 법을 알아보고자 한다.