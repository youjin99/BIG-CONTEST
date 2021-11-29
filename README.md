# 빅콘테스트 - 수산물 수입가격 예측

# 프로젝트 소개

**수입일, 제품 구분, 제조국, 수출국, 수입 용도, 수산물 분류, 수입형태 를 바탕으로 수산물 수입가격 예측**

# 제공 데이터

![그림1](https://user-images.githubusercontent.com/76585610/143735995-3acb3509-bfbc-4612-baeb-516d4d096b20.png)

- **기준일, 제품구분, 제조국, 수출국, 수입 용도, 중분류, 어종, 상세어종, 수입형태**

# 데이터 전처리

- **P_IMPORT_TYPE 피처**


![그림2](https://user-images.githubusercontent.com/76585610/143736015-feed4d2f-35b9-47ee-b993-f83e52020353.png)


**P_IMPORT_TYPE 피처는 총 78개로 개수가 방대** 


![그림3](https://user-images.githubusercontent.com/76585610/143736027-0d589338-037a-4806-aafc-7735f758da2e.png)

**가격에 영향을 미치는 피처를 정확히 파악 불가능** ![143803162-c4a793b4-748d-46b2-80af-8d22d8b80d84](https://user-images.githubusercontent.com/76585610/143803313-f4f5e23c-3417-4f91-8fdf-84fb833691c9.png)



**따라서 P_IMPORT_TYPE 분할 필요** 
**→ 수산물을 구분하는 방식을 참고하여 5가지 형태로 분할**
![그림5](https://user-images.githubusercontent.com/76585610/143736090-0b45005a-45b6-4128-abe5-cc20d2e8605e.png)
→ **수산물을 구분하는 방식을 참고하여 5가지 형태로 분할**

ex) 저장 방법(Storage), 부위(Part), 가공형태(Process), 횟감(Sashimi), 자숙 유무(Steaming Processing)


![그림6](https://user-images.githubusercontent.com/76585610/143736102-efa8698c-8fac-4d0c-a2f4-b5bdb3a268a7.png)


Storage(저장형태)

: 수산물 보관 형태에 따른 분류

ex) 건조, 냉동, 냉장, 염장, 활 

<img src = "https://user-images.githubusercontent.com/76585610/143736115-9b9e47ce-16f5-46d1-b37b-1017844d9cdc.png">


Process(가공형태)

: 수산물 가공 방법에 따른 분류

ex) 슬라이스, 절단, 캐비아대용, 팔렛, 한쪽껍질붙은, 훈제 

![그림8](https://user-images.githubusercontent.com/76585610/143736120-a0743a03-020e-4cc7-95c2-afcd1fb5f880.png)


Sashimi(횟감)

: 횟감 유무에 따른 분류

ex) 횟감, 포장횟감

<center><img src ="https://user-images.githubusercontent.com/76585610/143736163-a3c58748-0c37-4bed-81b9-c633b6111995.png" width="30%" height="30%"></center>


Steaming Processing(자숙)

: 김으로 쪄서 익힌 것과 익히지 않은 것 

ex) 자숙 

<center><img src = "https://user-images.githubusercontent.com/76585610/143736265-41126ce0-ab4e-4a34-934c-222f70cd179f.png"  width="30%" height="30%"></center>

Part(부위) 

: 수산물 부위에 따른 분류 

ex) 간, 개아지살, 곤이, 껍질, 꼬리_외화획득용, 난포선, 내장, 눈살, 다리, 동체, 턱 살, 머리, 머리_외화획득용, 머리살, 목살, 볼살, 알, 외투막, 지느러미, 집게다리, 창난

- **REG_DATE 피처**

![그림11](https://user-images.githubusercontent.com/76585610/143736350-07f91372-4fe4-4b46-be18-e10c89f13721.png)


**REG_DATE를 YEAR, MONTH, DAY로 나눠 컬럼 추가**

![그림12](https://user-images.githubusercontent.com/76585610/143736501-904391fc-c6f8-40eb-8ea3-f18d0defd939.png)


**데이터의 REG_DATE는 매주 월요일 데이터이므로 각 월의 DAY를 기준으로 1~5주차로 나누어 컬럼 추가** 

- **LABEL ENCODING**
    

    ![그림14](https://user-images.githubusercontent.com/76585610/143736586-547cf1cf-58ad-4245-9251-c521e5bf93dc.png)


**모델에 문자형은 사용할 수 없기 때문에 LabelEncoding을 사용해 문자형을 정수형으로 변경** 

# 모델링 및 결과

- **모델 훈련 과정**
1. **수산물 이름, MONTH, 주차가 같은 것들끼리 묶은 후 제조국, 수출국, 수입목적 3가지 조합 중 가장 많은 조합 TOP3를 이용해 TEST 데이터 생성** 

![그림15](https://user-images.githubusercontent.com/76585610/143803128-dfdad9db-e336-46d5-a1bb-231ae2da80b1.png)


P_NAME = 오징어, MONTH = 12, 주차 = 1의 제조국, 수출국, 목적 조합의 수 

2. **카테고리를 순서대로 예측 - 카테고리를 독립적으로 예측하면 그 날짜에 없는 조합이 나올 수 있기 때문에 카테고리를 순차적으로 예측**

![그림16](https://user-images.githubusercontent.com/76585610/143803148-e5ba3b83-2302-4ebf-a068-8f3ab5a99025.png)


3. **가격예측**
    
![그림18](https://user-images.githubusercontent.com/76585610/143803159-57e948b6-74d2-4fcb-98d1-fc7f742e9caa.png)

    
![그림19](https://user-images.githubusercontent.com/76585610/143803162-c4a793b4-748d-46b2-80af-8d22d8b80d84.png)

    
**결과 : RMSE(평균제곱근 편차) 1.22985**


4. **Feature Importance 확인 / 날짜와 가격의 상관관계 확인**

<img src = "https://user-images.githubusercontent.com/76585610/143803172-be2cf66a-a518-4f21-9850-6e62f4830854.png" width = "30%" height = "30%">

Importance가 가장 낮은 Feature는 P_PURPOSE

![그림21](https://user-images.githubusercontent.com/76585610/143803180-f914e045-8bae-462f-9fae-237c6042d662.png)


가격이 날짜에 따라 규칙적이지 않음 

**P_PURPOSE Feature 제거 + 날짜를 나타내는 주차, MONTH, YEAR Feature 제거** 

→ **RMSE 1.06515**
