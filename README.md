# Dataminig Project
* 주제 : 시군구에 따른 대기오염 분석
* 미세먼지와 오존분석은 같은 모델을 사용하였다.
* 김현석 : 20101340
* 박지원 : 20100593
* 서지흔 : 20102020

# 분석배경 
* 대기오염은 인위적 발생원에서 배출된 물질이 생물이나 기물에 직접적으로 해를 끼칠 만큼 다량으로 대기 중에 존재하는 상태이다.
* 그중 대표적인 요소인 미세먼지와 오존의 발생 원인과 대기오염에 취약한 지역을 찾아보고자 한다.
##### 미세먼지
* 대기 중에 떠다니거나 흩날려 내려오는 입자상 물질로 그 크기가 10μm이하인 것을 미세먼지, 2.5μm이하인 것을 초미세먼지라고 한다.
* 우리나라의 미세먼지는 30%~50%가 중국발 미세먼지이며 50%~70%는 국내 환경오염, 중금속-석유 석탄 등 화석연료 사용, 공장의 매연 등으로 발생한다. 
* 미세먼지는 협심증, 뇌졸증, 심혈관질환 등을 유발하여 혈관건강을 위협하고, 후두염, 호흡곤란, 기관지염, 천식, 폐렴 등을 유발하여 호흡기의 건강을 위협한다. 그 외에 안구와 피부 등에도 영향을 주는 1급 발암물질이다.
##### 오존
* 산소 원자 3개로 이루어진 산소의 동소체로 우리가 마시는 산소(O2)와 달리 독성을 가지고 있다.
* 오존 또한 자동차, 공장 등에서 배출되는 질소산화물과 휘발성유기화합물질이 반응하여 생성되는 대기오염 물질이다. 도심, 공장지대에서 더욱 잘 발생하고 대기가 정체일 때 수치가 상승할 확률이 높다.
* 오존은 자극성과 산화력이 강해 사람의 눈과 피부를 자극하고 호흡기질환을 유발하며 식물의 수확량감소, 건축물 부식, 스모그에 의한 대기오염 등 생태계 및 산업활동 전반에 악영향을 미친다. 
* 오존전량은 지상 어느 위치든 그 위치 상공에 존재하는 오존 분자의 총량으로 정의되며, 위도, 경도, 계절에 따라 달라진다.  
* 북반구의 오존전량은 봄에 뚜렷한 최댓값을 갖고 여름부터 가을까지 값이 낮아지는 계절변동을 보인다. 이는 늦가을과 겨울동안 열대지방에서 극지방으로 오존 수송이 증가하여 봄철 고위도에서 최댓값을 보인다.
 
 # 분석 목적
1. 여러 모델들을 통해 대기환경에 가장 영향을 많이 주는 요소를 추출할 수 있다.
2. 추출한 데이터를 바탕으로 대기오염이 심한 지역의 특징을 파악할 수 있다.
3. 최종적으로 지역별 대기오염의 해결방안을 탐색할 수 있고 해결방안을 탐색할 때 모델이 대기오염의 원인을 찾아주므로 효과적은 해결방안을 모색할 수 있다.

# 데이터 수집
 * 데이터는 통계청, 에어코리아, 한국전력기술, 공공데이터 포털 등 여러 사이트에서 2020.10~2021.04까지의 데이터를 수집하였다.
 * ★여러 대기오염 요인 중, 미세먼지(10um)와 오존을 사용하였다. 이산화 탄소같이 다른 요소들도 수집하여 데이터 세트에 열만 추가하면 분석이 가능하게 모델을 만들었다.

# 데이터
* X = 지역, 중금속(평균값), 교통량, 공장 수, 발전소 수, 인구 수, 1인당 생활폐기물 배출량, 풍속, 풍향, 강수량, 기온
* y = 미세먼지, 오존농도

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
1. 수도권에 가까울 수록 미세먼지, 오존농도가 높은 것으로 보아 미세먼지, 오존을 줄이기 위해서는 아파트와 같은 밀집된 공간을 줄이고 다른 지역을 홍보해 인구 밀집을 분산시키는 방안이 필요하다.
2. 우리나라의 미세먼지는 북서풍의 영향을 많이 받는 것으로 보아 중국의 영향이 크기 때문에 이에 대한 대책이 필요하다.
3. 더 많은 X변수나 새로운 y변수를 추가해 데이터분석을 한다면 또 다른 대기오염의 영향을 주는 다른 요인 또한 파악하고 개선해 나갈 수 있을 것이다.

# 한계점 및 추후 개선점
* 모델링 할 때 y targe으로 continuous value를 다루기 쉽지 않아 평균값으로 대체한 점에서 데이터의 손실이 이루어질 수 있다.
-> continuous value에 관한 새로운 모델 이용.

* 전국 시군구의 데이터 수가 229개라 train, test, validation 세개로 나누었을 때, test, validation 데이터가 상대적으로 적어서 충분하지 못했다. -> (시군구 -> 읍면동까지 확장)

# 참고자료
* 두산백과 – 미세먼지 https://terms.naver.com/entry.naver?docId=1080596&cid=40942&categoryId=32412 
* 두산백과 – 오존 https://terms.naver.com/entry.naver?docId=1128529&cid=40942&categoryId=32251 
* 오존 분석배경 ** http://www.climate.go.kr/home/10_wiki/index.php/%EC%84%B1%EC%B8%B5%EA%B6%8C_%EC%98%A4%EC%A1%B4 
** https://blog.naver.com/momomo0505/221306912948 
* 미세먼지 분석배경
https://www.yeongnam.com/web/view.php?key=20211129010003519 
