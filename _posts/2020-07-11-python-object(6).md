---
layout: post
title:  "객체지향(6) - testing"
subtitle: "객체지향(6) - testing"
categories: programming
tags: python
comments: true
---

- Testing, Unittest

---

### TESTING

- 객체지향은 유지보수에 용이하기 때문에, test 기법이 잘 발달되어 있음
- 중간중간에 자동화 테스트를 배치하여 사용



#### [ASSERT]

- 자동화 테스트의 가장 기본

  ~~~python
  assert sum([1,1,1])==6, "에러확인"
  #AssertionError: 에러확인 
  ~~~

  

#### [UNITTEST]

- 객체를 사용해 프로그램들의 가장 간단한 조각들을 테스트 

- type(unittest) = module

  - 객체이기 때문에 dir()하면 쓸수있는 것들이 다 나옴

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200711134009169.png" alt="image-20200711134009169" style="zoom:50%;" />

  - assert의 종류가 다양하며, 여러 테스트기능을 지원함

~~~python
from unittest import FunctionTestCase, TestCase
issubclass(FunctionTestCase, TestCase) #TestCase가 가장 부모
~~~

- TestCase모듈들을 상속해 test클래스를 생성한다.
  - test 클래스 생성 방법 2가지 : 1.파일기반, 2.주피터노트북 기반

1. 파일기반 테스트

~~~python
%%writefile test.py

class TestSum(unittest.TestCase):
    def test_sum(self):
        self.assertEqual(sum[1,2,3],6) #assert sum([1,2,3])==6 과 동일
        
if __name__ == '__main__':
    unittest.main() #파일생성
'''
Overwriting test.py
'''

!python test.py #실행
~~~



		2. 주피터노트북 내 테스트

~~~python
class TestSum(unittest.TestCase):
    def test_sum(self):
        self.assertEqual(sum([1,2,3],6))
        
#암기 필요
s = unittest.TestLoader().loadTestsFromTestCase(TestSum)
unittest.TextTestRunner(verbosity=2).run(s) #바로 test 실행
~~~



예제)

~~~python
class Book:
    def __init__(self, name):
        self.name = name
        self._genre = 'SF' #descriptor 사용 (mangling)
        self._world = 'dark'
        
    def can_read(self, person): #person 객체를 인자로 받음
        if person.role == 'can':
            return True
        else:
            return False
        
class Person:
    def __init__(self, role):
        self.role = role
        
#Test Class        
class TestBook(unittest.TestCase):
    def setUp(self): #기본세팅 - 전체 테스트에 적용할 객체 미리 정의
        self.book = Book('jihye')
        
    def test_canread(self): #여기부터 사용자정의 - 원하는테스트 추가
        person = Person('can')
        self.assertFalse(self.book.can_read(person)) #F면 성공
        
s = unittest.TestLoader().loadTestsFromTestCase(TestBook)
unittest.TextTestRunner(verbosity=2).run(s) #리턴값 True니깐 Failed뜸
~~~



+) 이 외에도 doctest, regression test 등의 테스트모듈이 존재함

​	https://docs.python.org/ko/3/library/unittest.html 참고

