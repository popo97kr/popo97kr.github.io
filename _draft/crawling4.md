### Crawling & Scraping 실습 - Scraping 편(3)



사이트 엑세스를 위해 로그인이 필요한 경우가 있다. 이럴 때 주로 **쿠키**를 사용해 자동으로 로그인 되게 한다.

로그인 페이지를 엑세스 하도록 하자.

http://pythonscraping.com/pages/cookies/login.html

무언가를 입력하기 때문에, form 태그가 존재한다.

<img src="C:\Users\user\Desktop\JHgit\blog\assets\img\crawling31.png" alt="image-20200731134748331" style="zoom:50%;" />

- welcome.php로 입력값이 전달됨을 알 수 있다.

  ![image-20200731134952768](C:\Users\user\Desktop\JHgit\blog\assets\img\crawling32.png)

  <img src="C:\Users\user\Desktop\JHgit\blog\assets\img\crawling33.png" alt="image-20200731135002217" style="zoom: 33%;" />

헤더 확인 결과, POST형태로 request 받고 params인자가 username, password이다. 

따라서 params를 만들어주고 welcome.php에 집어넣자.

![image-20200731135242183](C:\Users\user\Desktop\JHgit\blog\assets\img\crawling34.png)

BUT, 로그인이 제대로 되지 않음

![image-20200731140753411](C:\Users\user\Desktop\JHgit\blog\assets\img\crawling35.png)

**바로 쿠키가 저장이 안됐기 때문** ==> 따로 쿠키를 저장해서 집어넣자!

**1. requests.Session** : params를 쿠키로 집어넣음

~~~python
from requests import Session

session = Session()

#request 보내기 -> 쿠키로 보내짐
resp = session.request('POST', urljoin(url, dom.form['action']), data=params)
~~~

![image-20200731142237741](C:\Users\user\Desktop\JHgit\blog\assets\img\crawling36.png)

성공적. 쿠키가 저장된 것을 확인할 수 있다.

**2. cookies를 request 인자로 집어넣기**

![image-20200731142624339](C:\Users\user\Desktop\JHgit\blog\assets\img\crawling37.png)



**3. session.cookies에 직접 집어넣기**(실습 - 네이버메일 로그인)

mail.naver.com에 직접 로그인 한 뒤, 쿠키를 가져온다.

<img src="C:\Users\user\Desktop\JHgit\blog\assets\img\crawling38.png" alt="image-20200731144615931" style="zoom: 25%;" />

<img src="C:\Users\user\Desktop\JHgit\blog\assets\img\crawling39.png" alt="image-20200731144631880" style="zoom: 33%;" />

<img src="C:\Users\user\Desktop\JHgit\blog\assets\img\crawling40.png" alt="image-20200731144748500" style="zoom:33%;" />

<img src="C:\Users\user\Desktop\JHgit\blog\assets\img\crawling41.png" alt="image-20200731145121840" style="zoom:40%;" />

- 로그인 성공

+) 받은 메일을 스크래핑 해오자.

보아하니 rest API 형태로, 별개 json파일에 내용이 저장되어 있다.

(개발자도구 > 네트워크를 열고 받은메일함 클릭 시 나오는 파일들 중 하나에 내용이 담겨있을 것)

json파일의 전달방식, 파라미터에 맞게 session에 집어넣자.

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200731150131114.png" alt="image-20200731150131114" style="zoom: 33%;" />

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200731150152666.png" alt="image-20200731150152666" style="zoom:33%;" />

- 성공

- params의 page 수를 증가시키며 각 페이지의 내용을 가져올 수 있을 것이다.