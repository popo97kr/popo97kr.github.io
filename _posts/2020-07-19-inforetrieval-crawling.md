---
layout: post
title:  "크롤링 개요"
subtitle: "크롤링 개요"
categories: inforetrieval
tags: inforetrieval
comments: true
---

- 크롤링의 기본 개념을 알아보자.
- 웹은 HTTP Request-Response를 주고받으며 작동하는데, 여러 사이트를 돌아다니며 이를 자동화 하는 기법이 **크롤링**이다.

---

### 크롤링 기본 개념

#### [HTTP Protocol]

클라이언트와 웹서버는 HTTP Request - HTTP Response로 정보를 주고받는다.

<u>PROTOCOL FLOW</u>

1. 클라이언트가 서버에게 request를 날린다.

   - HTTP 같은 경우 TCP-IP 사용
   - handshaking을 하고난 뒤 정보를 주고받을 준비가 됨
   - **Request Header :**
     - Method - GET/POST/PUT/DELETE 등
     - Host (At which server)
     - Accept : 응답을 받을 수 있는 리소스 형태
     - User-Agent : request한 사람에 대한 정보 -> 직접 접속했는지, 로봇인지 판별
     - 그 외 Accept-Charset(인코딩관련), Connection, Port #, File Name, etc.

2. 서버에서 HTML 형태의 response를 보낸다.

   - 브라우저 측에서 **렌더링**을 해주면 우리가 보는 화면이 출력됨

   - **Response Header : **

     - Status : 200(성공), 403(접근제한), 404(page not found), 500(서버에러), etc.

       <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling1.png?raw=true" alt="image1" style="zoom: 33%;" />

   - Response Body에 HTML 내용을 전송한다.

   - 단, REST / RESTful API (분산시스템설계)를 사용하는 경우, 접근을 분리하여 사용해야 한다.

     - HTML은 틀로서 존재하고, 실제 데이터는 json, xml 등의 형태로 받기 때문
     - 이러한 경우, load_json 함수나 xml.dom.minidom 등 함수 활용



이렇게 정보를 주고받는 과정을 자동화 하는 것이 **크롤링/스크래핑** 이다.



#### [Crawling / Scraping]

정보수집을 자동화하기 위해선 **크롤링**과 **스크래핑**이 필요하다.

- **크롤링** : 여러 링크들을 타고 돌아다니는 행위
- **스크래핑** : 링크 내의 데이터를 수집하는 행위
  - Request, Parsing, DB저장이 모두 여기 속해있음

이며, 크롤링/스크래핑을 하는 봇들을 크롤러, 스크래퍼라 부른다.

<img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling2.png?raw=true" alt="image1" style="zoom:50%;" />

위 그림 내 **바깥 루프(gathering links)는 크롤러, 안쪽 루프(gathering data)는 스크래퍼**가 맡아서 진행한다.



단, 주의사항이 있다.

- 자동화 봇은 과도한 트래픽을 유발한다. 많은 용량을 잡아먹기 때문이다. 따라서 사용 시 메모리를 유의해야 한다.

- 서버 자체에서 개인정보 등의 이유로 크롤러를 막은 경우가 다수 존재

  - robots.txt : 로봇 접근 제약이 명시된 파일

    <img src="https://github.com/popo97kr/popo97kr.github.io/blob/master/assets/img/crawling6.png?raw=true" alt="image1" style="zoom:50%;" />

  ​			= 모든 user-agent robot에 대해 /search는 수집 불허, /search/about은 OK



크롤링 / 스크래핑 자세한 실습은 [다음 포스트](https://popo97kr.github.io/inforetrieval/2020/07/19/inforetrieval-crawling2/)에서 진행하도록 한다.