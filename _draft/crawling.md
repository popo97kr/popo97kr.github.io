웹은 HTTP Request-Response를 주고받으며 작동하는데, 여러 사이트를 돌아다니며 이를 자동화 하는 기법이 **크롤링**이다.



### 크롤링 기본 개념

#### HTTP Protocol

웹서버-클라이언트 간 HTTP Request - HTTP Response로 정보를 주고받는다.

[FLOW]

1.클라이언트가 서버에게 request를 날린다. 

- HTTP 같은 경우 TCP-IP 사용
- handshaking을 하고난 뒤 정보를 주고받을 준비가 됨
- **Request Header : **
  - Method - GET/POST/PUT/DELETE 등
  - Host (At which server)
  - Accept : 응답 받을 수 있는 형태의 리소스
  - User-agent : request한 사람에 대한 정보 --> 사람인지 아닌지 판별
  - Accept-charset(인코딩), Connection, Port #, Protocol, file name 등

2.서버에서 HTML형태의 response를 보낸다.

- 브라우저 측에서 **렌더링**을 해주면 우리가 보는 화면이 출력됨

- **Response Header :** 

  - Status

    - 200 : 성공
    - 403 : 접근권한X, 404: page not found
    - 500 : 서버 에러

    ![image1](https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling1.png?raw=true)

- Response Body 부분에 html 내용 저장하여 전송
- REST / RESTful API (분산시스템설계)를 사용하는 경우, 접근 방법을 분리해서 사용해야 한다.
  
  - HTML은 틀로써 존재하고, 실제 데이터는 JSON, XML, TEXT, RSS 등의 형태로 받기 때문
  
  - 이런경우, load_json 함수나 xml.dom.minidom 등 함수 활용
  
    

#### Crawling / Scraping

정보수집을 자동화하기 위해선 **크롤링**과 **스크래핑**이 필요하다.

- 크롤링 : 여러 링크들을 타고 들어가는 행위

- 스크래핑 : 링크 내의 데이터를 수집하는 행위 (Request, Parsing, DB에 저장하는 과정이 모두 여기 속해있음)

  ※Parsing 이란? 데이터를 분리해서 원하는 정보만 가져오는 과정

이며, 크롤링/스크래핑을 자동화하는 봇들을 크롤러, 스크래퍼라 부른다.

![image1](https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling2.png?raw=true)

여기서 **바깥 루프(gathering links)는 크롤러**, **안쪽 루프(gathering data)는 스크래퍼**가 맡아서 진행한다.



[주의해야할 점]

- 자동화 봇은 과도한 트래픽을 유발함 - 많은 용량을 잡아먹기 때문

- 서버 자체에서 개인정보 등의 이유로 크롤러를 막아놓은 경우

  - robots.txt : 로봇 접근 제약이 명시된 파일

  <img src="C:\Users\user\Desktop\JHgit\blog\assets\img\crawling6.png" alt="image-20200726204457119" style="zoom:50%;" />

  ​     = 모든 user-agent robot에 대해서 /search는 수집 불허, /search/about은 OK.

크롤링 / 스크래핑 자세한 실습은 [다음 포스트]()에서 진행하도록 한다.

