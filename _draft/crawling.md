### 크롤링 개념

#### HTTP Protocol

웹서버-클라이언트 간 HTTP Request - HTTP Response로 정보를 주고받는다.

1.클라이언트가 서버에게 request를 날린다. 

- HTTP 같은 경우 TCP-IP 사용
- handshaking을 하면 정보를 주고받을 준비가 됨
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



