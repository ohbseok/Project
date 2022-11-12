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
- 구독과 duration(이동시간) pearson 상관분석
  - 상관계수 : -0.51, p-value : 0 -> 이동시간이 작을수록 Subscriber

# 데이터 전처리
- 시계열 datetime -> year, month, weekday 변환 
  - 이후 holiday, season, commute feature engineering 진행
- haversine package -> `distance columns` feature engineering
- target 컬럼 right skewed -> log transformation

# 모델링
- baseline (target 평균값)
  - RMSE: 15.24, R2: -0.00
- XGBoostR 학습
  - RMSE 기준 early stopping 20 rounds 로 학습
  - train rmse : 11.486, valid rmse : 12.099  

# 해석
- feature importance 결과 
<img width="794" alt="image" src="https://user-images.githubusercontent.com/94156708/201368910-c2537ba5-2a58-495b-aa9a-f5c242bb51c9.png">



- 기온 특성이 중요도 상위권에 있지 않았지만, 계절과 Rain 유무로 판단했을 때 2차 관계성을 파악할 수 있다.  
- 비가 오면 이동시간에 영향을 미치고, 단거리 비율이 높은지는 다르 해석 기법이 필요하다.
- 주말(요일)보다는 출퇴근 시간인지, 어떤 계절인지가 더 중요한 특성으로 해석된다.
- 구독 유무가 가장 중요한 특성으로 해석되는데 구독 유저일수록 짧은 거리를 자주 이용할 것이다. 


# More than

- 고객 정보와 모빌리티 종류가 있었다면 더 정확한 예측이 가능할 것이다. 
- 실제로 날씨는 이동시간에 큰 영향을 주지 않는다. 


