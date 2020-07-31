### Crawling & Scraping 실습 - Scraping 편(2)



이전 포스트에서 download()라는 스크래핑 함수를 생성했다.

스크래핑 함수를 이용하여 다양한 사이트의 소스를 가져와보고, 여러가지 상황에 어떻게 대처할 수 있는지 알아보자.

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



#### 1. REST API 형태 사이트

**[사이트 내용이 별개의 json/xml형태로 이루어진 경우]**

- 고용노동부의 검색결과를 가져와본다.
  - http://www.moel.go.kr/local/seoul/search.do
- 우선, 개발자도구의 search.do파일의 헤더를 봤더니, method가 POST이다.

<img src="C:\Users\user\Desktop\JHgit\blog\assets\img\crawling4.png" style="zoom: 25%;" />



~~~python
url = 'http://www.moel.go.kr/local/seoul/search.do'
#header에서 params를 가져온다.
params = {
    'prefixQuery': 'LOCAL1|LOCAL2|LOCAL3|LOCAL4|LOCAL5|LOCAL6',
    'requery': '',
    'sort': 'RANK',
    'sortOrder': 'DESC',
    'sfield': '',
	...
    'edate': ''
}

params['tmpQuery'] = params['query'] = '최저임금'

headers = {
    'user-agent':'Mozilla/5.0 (Wind .... Chrome/84.0.4147.89 Safari/537.36'
}

resp = download(url+'?query='+params['query'], params, headers, 'POST')
~~~

**BUT**, resp.text 출력 결과 데이터가 들어있지 않다.

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728132413608.png" alt="image-20200728132413608" style="zoom:33%;" />

실제로 search.do의 preview를 찾아봤더니,

<img src="C:\Users\user\Desktop\JHgit\blog\assets\img\crawling4.png" alt="image2" style="zoom: 25%;" />

DB에 있는 내용이 아닌 틀만 제공하고 있는 상태다.

이런 경우, 진짜 내용은 주로 **json, xml**에 위치하고 있다.

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728133437688.png" alt="image-20200728133437688" style="zoom:25%;" />



보아하니, search_json.jsp 에 내용이 모두 저장되어 있다.

- 가장 먼저 search.do가 호출된 뒤, 각종 js가 호출이 됨

- css, js등 통해 이미지와 그래픽을 다 준비한 뒤에,

- json 파일이 호출되어 쿼리를 통해 DB에서 내용을 가져옴
  - search_json 헤더의 referer가 search.do임을 보아 search.do가 json을 불러왔음을 알 수 있음

이러한 파일의 type : **xhr = 데이터를 분리해서 저장해 놓은 부분**

그래서 xhr에서 데이터를 가져와보자.

~~~python
url = 'http://www.moel.go.kr/searchapi/search_json.jsp'

params = { #Headers의 Form Data를 params로 입력
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

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728135102012.png" alt="image-20200728135102012" style="zoom: 33%;" />![image-20200728135246875](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728135246875.png)

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728135246875.png" alt="image-20200728135246875" style="zoom:33%;" />

원하는 내용을 가져왔음을 알 수 있다.



**[form 태그의 input query가 jquery로 넘겨지는 경우]**

- http://example.webscraping.com/places/default/search

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200730004142286.png" alt="image-20200730004142286" style="zoom:50%;" />

- js확인 결과, input 입력 시 결과가 search.json파일에서 넘겨짐을 알 수 있음

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200730004236777.png" alt="image-20200730004236777" style="zoom:50%;" />

- OR another tip : 네트워크를 열어두고, 제출 버튼 누를 때 추가되는 파일(주로 xhr파일)이 있음 --> 얘가 data 가져오는 애로 볼 수 있음



search.json파일 헤더 정보

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200730004348156.png" alt="image-20200730004348156" style="zoom:50%;" />

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200730004554249.png" alt="image-20200730004554249" style="zoom:50%;" />

contents-type : json

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200730004627170.png" alt="image-20200730004627170" style="zoom:50%;" />

ajax로 넘기는 url form은 다음과 같음

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

HTML에 데이터가 존재한다면, **BeautifulSoup**을 활용해 태그를 기반으로 내용을 가져올 수 있다.

- 해당 모듈은, HTML이나 XML을 parsing하여 **DOM구조의 데이터를 리턴**한다.

  - 파서 종류

    - html.parser (추천) : 내장된 패키지로서 별다른 패키지의 도움 필요 X

    - lxml (추천): 빠름
    - lxml-xml, xml : xml parser
    - html5lib  : 느리나 완성도↑

  - 파서들은 짝이 맞지 않는 태그들에 대해서 왠만하면 잘 수정해줌

- DOM이란 : 문서의 객체 기반 표현 방식 ==> "노트 트리" 형식

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728143434467.png" alt="image-20200728143434467" style="zoom: 25%;" />

​								부모노드, 자식노드, 형제노드로 호출 가능



- **dom.prettify()**

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728195819184.png" alt="image-20200728195819184" style="zoom:;" />

- tag호출방법1 : **attribute 방식**

  ![image-20200728200036557](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728200036557.png)

- tag 호출방법2 : **find_all() 방식**

  - 원하는 요소 한꺼번에 찾을 수 있음

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728200209645.png" alt="image-20200728200209645" />

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728200209645.png" alt="image-20200728200209645" style="zoom:80%;" />

- **find_parents() / find_sibling()**

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728200323798.png" alt="image-20200728200323798" style="zoom:80%;" />

- tag 호출방법2 : **selector 방식**
  - select_one, select
  - <u>#id</u>, <u>.class</u>로 호출. 
  - parent > children
  - sibling_previous + sibling_to_find

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200730021556683.png" alt="image-20200730021556683" style="zoom:50%;" />

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200730021647325.png" alt="image-20200730021647325" style="zoom:50%;" />

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200730021720443.png" alt="image-20200730021720443" style="zoom: 40%;" />

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200730021941791.png" alt="image-20200730021941791" style="zoom: 40%;" />



[dom 실습: scrap http://pythonscraping.com/pages/page3.html]

- 개발자도구에서 html태그 확인해보자.

~~~python
resp = download('http://pythonscraping.com/pages/page3.html')
dom = BeautifulSoup(resp.content, 'html.parser')
~~~

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728201213429.png" alt="image-20200728201213429" style="zoom:50%;" />

목표1) 테이블 내 두번째 행의 이미지를 가져온다.

1. 태그가 img인건 알았다. 단, 이 img2.jpg라는 specific한 이미지를 가져와야한다.
2. 우선 명확한 id나 class이름을 가진 부모태그를 찾는다.
   - 바로 위 tr태그에 id="gift2"라는 이름으로 되어있다
   - 바로 위 태그에 없다면 이름 나올때 까지 계속해서 부모 태그 찾는다.
3. 이름있는 부모태그를 찾았으면 children list 중 해당되는 태그를 indexing
4. attrs를 통해 정확한 주소 알아온다.

![image-20200728202544739](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728202544739.png)

5. 사이트 url과 합친다.

![image-20200728235525011](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200728235525011.png)



목표2) 금액 추출

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



text => 태그 제외한 내용 추출

strip() => 공백 제거





