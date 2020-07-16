---
layout: post
title:  "reading file"
subtitle: "reading file"
categories: programming
tags: python
comments: true
---

- ways to read file

---

### OPEN & READ FILE

- **파일의 기본 구조 : Header, Data, EOF**

  - 파일을 못 읽는 경우 대부분 header 해석 못해서 일어남

- **Flat File** = 간단한 txt파일이나 binary 파일

- **%%writefile** : 밑에있는 셀 전체를 txt로 저장한다는 명령어

  - 주피터노트북에만 사용가능

  

- 파이썬에 내 파일 관련 기본 함수는 1가지 = **open**

  - **readline()** : next와 같으나, 마지막 문장에 도달해도 에러 발생 X
    - try로 에러 막아놨기 때문

  ~~~python
  file = open('jh.txt')
  file.readline() #계속실행
  ~~~

  

- **open; write version**

  ~~~python
  file = open('jihye.txt','w')
  file.write('Hi!\n')
  file.write("I'm Jihye")
  file.close()
  ~~~

  

- **file.close()** : open이 있으면 항상 close 짝이 있어야 ㅇ

  - 더이상 메모리에 할당하지 않는다는 뜻
  - 파이썬 내에선 garbage collection 하기 때문에 알아서 없애주긴 하나, DB에선 매우 중요

  

- **with구문** : "context manager"

  - dir 내 \__enter__, \__exit__가 있는 객체에 모두 사용 가능

  ~~~python
  import PIL import Image
  
  with Image.open('hi.jpg') as image:
      image.save('quality_down.jpg', quality=40)
  ~~~

  ~~~python
  import matplotlib.pyplot as plt
  with plt.xkcd():
      plt.plot([1,2,3,4])
  ~~~

  - 내부적으로 file.close()가 탑재 - 깔끔하게 메모리 삭제해줌
  - 첫문장 실행 시 enter 실행, 마지막문장 실행 시 exit 실행

  

- **인코딩**: 파일문자 깨질때

  ~~~python
  open('hello.txt', 'r', encoding='utf-8')
  ~~~

