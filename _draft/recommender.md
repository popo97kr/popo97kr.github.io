# 추천시스템

### Contents based Filtering (CBF) - 컨텐츠 중심

- "내가 좋아하는 컨텐츠"와 비슷한 속성인 컨텐츠 추천
  - 내가 A에 높은 평점을 주었는데, 그게 'J'감독의 액션 영화이다 ==> 'J'감독의 다른 액션 영화를 추천



### Collaborative Filtering (CF) - 사람중심

- 사용자의 행동 양식을 기반으로 추천
- nearest neighbor based / latent factor based 로 구성

#### [Nearest Neighbor(memory) based Collaborative Filtering(최근접 이웃 기반 협업 필터링)]

- user-item 평점 행렬이 필요

  ![image-20200718142952104](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200718142952104.png)

  pandas 에서 pivot table로 변환

  - 위의 문제 : sparsity 발생

- 사용자가 아직 평가하지 않은 아이템(빈칸)을 예측

- 두갈래 : 1. 사용자 기반, 2. 아이템 기반

  **1. 사용자 기반**

  - 나와 비슷한 유형의 고객들이 소비한 컨텐츠를 추천

  - 비슷한 유형인건 어떻게 찾느냐?

    ![image-20200718143740517](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200718143740517.png)

    같은 아이템에 비슷한 평점을 매긴 User1과 User2는 유사하다 판단

  **2. 아이템 기반(more accurate)**

  - {"내가 소비한 컨텐츠"을 소비한 유저}가 소비한 다른 컨텐츠를 추천

    ![image-20200718144045302](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20200718144045302.png)

    (item-user 행렬)

    ItemA와 ItemB는 유사한 평점 분포를 가지고 있다 =>user4에게 itemA를 추천한다



​	아이템 기반보다는 사용자 기반을 더 정확도가 높음

​	=>명작의 경우 모든 평점이 높기 때문