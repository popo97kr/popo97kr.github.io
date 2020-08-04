---
layout: post
title:  "크롤링&스크래핑 실습 - Scraping 편(3)"
subtitle: "크롤링&스크래핑 실습 - Scraping 편(3)"
categories: inforetrieval
tags: inforetrieval
comments: true
---



- 쿠키 입력법
- 사이트 엑세스를 위해 로그인이 필요한 경우가 있다. 이럴 때 주로 쿠키를 사용한다.

- 사이트 엑세스를 위해 로그인이 필요한 경우가 있다. 이럴 때 주로 **쿠키**를 사용한다.

---

http://pythonscraping.com/pages/cookies/login.html

해당 페이지에서 로그인을 하려고 한다.



우선 무언가를 입력해서 제출하기 때문에, form 태그가 존재한다.

 <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling31.png?raw=true" alt="image1" style="zoom:50%;" />

- form 태그의 action 부분을 보아, welcome.php로 값이 넘겨짐을 알 수 있다.

  (welcome.php Header)

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling32.png?raw=true" alt="image1" style="zoom:50%;" />

  <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling33.png?raw=true" alt="image1" style="zoom:50%;" />

  헤더 확인 결과, POST 형태로 request를 받고, 

  params 인자는 username, password 이다.



따라서 params를 알맞게 만들어주고 welcome.php에 인자로 넘겨주자.

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling34.png?raw=true" alt="image1" style="zoom:50%;" />

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling35.png?raw=true" alt="image1" style="zoom:50%;" />

BUT, 로그인이 제대로 되지 않는다. 왜냐?

바로 **쿠키가 저장되지 않았기 때문**. ==> 따로 쿠키를 저장해서 집어넣자!



쿠키를 생성하는 방법에는 3가지가 있다.

**1. requests.Session 활용** : params를 쿠키로 집어넣는다.

~~~python
from requests import Session

session = Session()

#request 보내기 -> params가 쿠키로 보내짐
resp = session.request('POST', urljoin(url, dom.formm['action'], data = params))
~~~

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling36.png?raw=true" alt="image1" style="zoom:50%;" />

성공적으로 쿠키가 저장된 것을 알 수 있다.



**2. Request.request에 cookies를 인자로 집어넣기**

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling37.png?raw=true" alt="image1" style="zoom:50%;" />



**3. session.cookies 라는 dictionary에 직접 집어넣기**

(실습 - 네이버메일 로그인)

mail.naver.com에 직접 로그인을 한 뒤, 쿠키를 가져온다.

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling38.png?raw=true" alt="image1" style="zoom:30%;" />

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling39.png?raw=true" alt="image1" style="zoom:50%;" />

session.cookies에 집어넣는다.

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling40.png?raw=true" alt="image1" style="zoom:50%;" />

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling41.png?raw=true" alt="image1" style="zoom:50%;" />

성공적으로 로그인이 됐음을 알 수 있다.





번외) 받은 메일을 스크래핑 해오자.

보아하니 rest API 형태로, 별개 json파일에 내용이 저장되어 있다.

(개발자도구 > 네트워크를 열고 받은메일함 클릭 시 나오는 파일들 중 하나에 내용이 담겨있을 것)

json파일의 전달방식, 파라미터에 맞게 session에 집어넣자.

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling42.png?raw=true" alt="image1" style="zoom:50%;" />

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling43.png?raw=true" alt="image1" style="zoom:50%;" />

- 성공

- params의 page 수를 증가시키며 각 페이지의 내용을 가져올 수 있을 것이다.