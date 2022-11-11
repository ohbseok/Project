# 시계열 및 날씨 기반 모빌리티 이동시간 예측 

- 날씨는 모빌리티 이동시간에 어떤 영향을 미칠까? 
- 시간과 요일에 따라 사용자 이동 패턴이 다를까?

# 가설
- 기온이 높을수록 이동시간이 짧을 것이다. 
- 강수량이 많으면 단거리 이동 비율이 많아질 것이다.
- 주말에는 장거리 이용 고객이 많을 것이다. 


# EDA
- 최대바람속도와 최대돌풍속도 상관분석
  - pearson : 0.57, p-value : 0.0002
  - 최대돌풍속도 결측치를 최대바람속도 groupby 후 median 으로 채움
- 구독과 다른 도시로 이동하는지 유무 kendal-tau 상관분석
  - tau : 0.04, p-value : 0 -> 상관 거의 없음
- 구독과 duration(이동시간) pearson 상관분석
  - 상관계수 : -0.51, p-value : 0 -> 이동시간이 작을수록 Subscriber

# 데이터 전처리
- 시계열 datetime -> year, month, weekday 변환 후 holiday, season, commute feature 생성
- haversine package -> `distance columns` feature engineering
- target 컬럼 right skewed -> log transformation

# 모델링
- baseline (target 평균값)
  - RMSE: 15.24, R2: -0.00
- XGBoostR 학습
  - RMSE 기준 early stopping 20 rounds 로 학습
  - train rmse : 11.486, valid rmse : 12.099

- 튜닝 및 교차검증 후 최종 lgbmC의 f1 score : 0.9535
  
  

# 해석
- feature importance 결과 
<img width="794" alt="image" src="https://user-images.githubusercontent.com/94156708/201368910-c2537ba5-2a58-495b-aa9a-f5c242bb51c9.png">

### 구독자일수록 이동시간 어떤지 해석 검색!!


- 이용시간이 짧을수록 구독자일 가능성이 높다 -> 가설 일치
- 이용거리가 길수록 구독자일 가능성이 높다 -> 가설 일치
- 평일에 이용할수록 구독자일 가능성이 높다 -> 가설 기각

# More than
- oversampling 후 f1 score 가 떨어졌는데 무슨 이유일까?
- 고객 정보와 가격 데이터가 있었다면 더 정확한 추천이 가능할 것이다. 
- 매출이 비슷하더라도 구독으로 장기고객 전환하는 게 비즈니스에 도움 될까?
