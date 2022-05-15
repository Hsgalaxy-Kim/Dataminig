# Dataminig
* 대기오염 분석
* 미세먼지와 오존분석은 같은 모델을 사용하였다.

# 데이터 수집
 * 데이터는 통계청, 에어코리아 등 여러 사이트에서 2020.10~2021.04까지의 데이터를 수집하였다.
 * ★여러 대기오염 요인 중, 미세먼지(10um)와 오존을 사용하였다. 이산화 탄소같이 다른 요소들도 수집하여 데이터 세트에 열만 추가하면 분석이 가능하게 모델을 만들었다.

# 데이터
X = 지역, 중금속(평균값), 교통량, 공장 수, 발전소 수, 인구 수, 1인당 생활폐기물 배출량, 풍속, 풍향, 강수량, 기온
y = 미세먼지, 오존농도

## 데이터 전처리
 * 데이터를 classification하기 위해 엑셀파일에서 평균값을 기준으로 y값이 높으면 1 낮으면 0 이렇게 미세먼지 농도와 오존 농도를 처리하였다. 값이 1을 가질경우 상대적으로 대기가 안좋다는것을 의미한다.
 * 미세먼지와 오존은 target이 다르므로 .iloc[ ]를 통해 y를 다르게 지정하였다.
 * Train, Test, Val을 분리하였고 그 기준을 y 비율에 맞추었다.
 * 최종 예측값에 지역명을 추가하기 위해 .pop( )을 사용하였다.
 * SVM과 Logistic regression에는 표준화를 진행하였다.

## 모델
### Decision Tree
* 하이퍼 파라미터는 max_depth, min_samples_leaf를 사용하였다.
* for문을 통해 값을 바꿔가며 validation accuracy를 측정하여 pandas DataFrame으로 출력하였다.
* 최종 모델링을 한 다음 (미세먼지 : max_depth=7, min_samples_leaf=10, 오존 : max_depth=7, min_samples_leaf=5) test을 사용하여 예측값을 실제값과 지역을 추가하여 출력하였다.
* confusion_matrix와 accuracy_score을 사용하여 정확도를 출력하였다. (정확도 미세먼지 : 78%, 오존 79%)
* 최종 트리 모델을 시각화 하였다.
* Feature importance를 출력하였다. 그 결과 wind_direction, num_of_poeple, wind_speed, rainfall, heavy_metal 순으로 중요하다는 것을 알았다.
### => DT를 통해 예측이 어떻게 이루어 지는지 명확하게 알 수 있었다.

### SVM-StandardScaler
* 전처리로 StandardScaler을 사용하였다.
* 하이퍼 파라미터는 C와 gamma를 사용하였다.
* for문을 통해 값을 바꿔가며 validation accuracy를 측정하여 pandas DataFrame으로 출력하였다.
* 최종 모델링을 한 다음(미세먼지 : C=10, gamma=0.01, 오존 : C=1, gamma=0.01) test을 사용하여 confusion_matrix와 accuracy_score을 사용하여 정확도를 출력하였다. (정확도 미세먼지 :  74.14%, 오존 : 77.59%) 
* 예측값을 실제값과 지역을 추가하여 출력하였다.
### SVM-MinMAxScaler
* 전처리로 MinMAxScaler을 사용하였다.
* 하이퍼 파라미터는 C와 gamma를 사용하였다.
* for문을 통해 값을 바꿔가며 validation accuracy를 측정하여 pandas DataFrame으로 출력하였다.
* 최종 모델링을 한 다음(미세먼지 : C=10, gamma=0.1, 오존 : C=10, gamma=0.1) test을 사용하여 confusion_matrix와 accuracy_score을 사용하여 정확도를 출력하였다. (정확도 미세먼지 : 72.41%, 오존 : 75.86%)
* 예측값을 실제값과 지역을 추가하여 출력하였다.
### => SVM은 StandardScaler가 더 높았고 SVM 정확도가 꽤 나오는 것을 보아 데이터를 신뢰할만하다는 것을 알 수 있다.

### Logistic Regression
* 전처리로 StandardScaler를 사용하였다.
* 하이퍼 파라미터로 C를 사용하였다.
* for문을 통해 값을 바꿔가며 validation accuracy를 측정하여 pandas DataFrame으로 출력하였다.
* 최종 모델링을 한 다음(미세먼지, 오존 : C=0.01) test을 사용하여 confusion_matrix와 accuracy_score을 사용하여 정확도를 출력하였다. (정확도 미세먼지 : 74.14%, 오존 : 67.24%)
* 예측값을 실제값과 지역을 추가하여 출력하였다.
* 회귀계수와 상수항을 출력하였다. 회귀계수(미세먼지): [[-0.    -0.014  0.001  0.     0.    -0.    -0.002 -0.019 -0.016 -0.005]]  상수항: [-0.], 회귀계수(오존): [[-0.     0.005 -0.001 -0.    -0.     0.     0.001  0.007  0.005  0.002]]  상수항: [0.]
### => SVM과 비슷한 결과를 출력하였다. 회귀계수를 통해 heavy_metal, traffic, wind_speed, wind_directoin, rainfall, 온도(C)가 어떻게 영향을 주는지 알 수 있었다. 나머지(num_of_factories, num_of_power_plants, num_of_people, household_waste_discharge_per_person (kg/days)) 큰 영향을 주지 않는다는것을 알 수 있다.

### Linear Regression
* 모델을 생성하여 test을 하였다.
* 회귀계수와 상수항을 출력하였다. 회귀계수: [ 2.199 -0.072  0.001  0.457  0.     0.144 -1.238 -0.041 -0.055 -0.709]  상수항: 59.38395133823418
* 정확도를 출력하였다. (정확도 47.46%)
* 예측값을 실제값과 지역을 추가하여 출력하였다.
### => Linear Regression을 통해 정확한 값을 출력하기에는 조금 무리가 있다는 것을 알게되었다.

# 결과
미세먼지는 SVM(StandardScaler) > Decision Tree > SVM(MinMaxSacler) > Logistic Regression > Lienar Regression 순으로 정확도가 높게 나왔고
오존은 SVM(StandardScaler) > SVM(MinMaxSacler) > Decision Tree > Logistic Regression 순으로 높게 나왔다.

# 기대효과
1. 수도권에 가까울 수록 미세먼지, 오존농도가 높은 것으로 보아 미세먼지, 오존을 줄이기 위해서는 아파트와 같은 밀집된 공간을 줄이고 다른 지역을 홍보해 인구 밀집을 분산시키는 방안이 필요
2. 우리나라의 미세먼지는 북서풍의 영향을 많이 받는 것으로 보아 중국의 영향이 크기 때문에 이에 대한 대책이 필요
3. 더 많은 X변수나 새로운 y변수를 추가해 데이터분석을 한다면 또 다른 대기오염의 영향을 주는 다른 요인 또한 파악하고 개선해 나갈 수 있을 것

# 한계점 및 추후 개선점
모델링 할 때 y targe으로 continuous value를 다루기 쉽지 않아 평균값으로 대체한 점에서 데이터의 손실이 이루어질 수 있음
-> continuous value에 관한 새로운 모델 이용

전국 시군구의 데이터 수가 229개라 train, test, validation 세개로 나누었을 때, test, validation 데이터가 상대적으로 적어서 충분치 못함
  -> 시군구 -> 읍면동까지 넓히기

# 참고자료
두산백과 – 미세먼지 https://terms.naver.com/entry.naver?docId=1080596&cid=40942&categoryId=32412 
두산백과 – 오존 https://terms.naver.com/entry.naver?docId=1128529&cid=40942&categoryId=32251 
오존 분석배경 http://www.climate.go.kr/home/10_wiki/index.php/%EC%84%B1%EC%B8%B5%EA%B6%8C_%EC%98%A4%EC%A1%B4 
https://blog.naver.com/momomo0505/221306912948 
미세먼지 분석배경
https://www.yeongnam.com/web/view.php?key=20211129010003519 
