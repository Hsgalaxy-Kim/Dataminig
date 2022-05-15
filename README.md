# Dataminig
* 대기오염 분석

## 데이터 수집
 * 데이터는 통계청, 에어코리아 등 여러 사이트에서 2020.10~2021.04까지의 데이터를 수집하였다.
 * 여러 대기오염 요인 중, 미세먼지(10um)와 오존을 사용하였다. 이산화 탄소같이 다른 요소들도 수집하여 데이터 세트에 열만 추가하면 분석이 가능하게 모델을 만들었다.

## 데이터 전처리
 * 데이터를 classification하기 위해 엑셀파일에서 평균값을 기준으로 값이 높으면 1 낮으면 0 이렇게 미세먼지 농도와 오존 농도를 처리하였다. 값이 1을 가질경우 상대적으로 대기가 안좋다는것을 의미한다.
 * 미세먼지와 오존은 target이 다르므로 .iloc[ ]를 통해 X, y를 다르게 지정하였다.
 * Train, Test, Val을 분리하였고 그 기준을 y 비율에 맞추었다.
 * 최종 예측값에 지역명을 추가하기 위해 .pop( )을 사용하였다.
 * SVM과 Logistic regression에는 표준화를 진행하였다.

## 모델
### Decision Tree
* 하이퍼 파라미터는 max_depth, min_samples_leaf를 사용하였다.
* for문을 통해 값을 바꿔가며 validation accuracy를 측정하여 pandas DataFrame으로 출력하였다.
* 최종 모델링을 한 다음 (max_depth=7, min_samples_leaf=10) test을 사용하여 예측값을 실제값과 지역을 추가하여 출력하였다.
* confusion_matrix와 accuracy_score을 사용하여 정확도를 출력하였다. (정확도 78%)
* 최종 트리 모델을 시각화 하였다.
* Feature importance를 출력하였다. 그 결과 wind_direction, num_of_poeple, wind_speed, rainfall, heavy_metal 순으로 중요하다는 것을 알았다.

### SVM-StandardScaler
* 전처리로 StandardScaler을 사용하였다.
* 하이퍼 파라미터는 C와 gamma를 사용하였다.
* for문을 통해 값을 바꿔가며 validation accuracy를 측정하여 pandas DataFrame으로 출력하였다.
* 최종 모델링을 한 다음(C=10, gamma=0.01) test을 사용하여 confusion_matrix와 accuracy_score을 사용하여 정확도를 출력하였다. (정확도 74.14%)
* 예측값을 실제값과 지역을 추가하여 출력하였다.
### SVM-MinMAxScaler
* 전처리로 MinMAxScaler을 사용하였다.
* 하이퍼 파라미터는 C와 gamma를 사용하였다.
* for문을 통해 값을 바꿔가며 validation accuracy를 측정하여 pandas DataFrame으로 출력하였다.
* 최종 모델링을 한 다음(C=10, gamma=0.1) test을 사용하여 confusion_matrix와 accuracy_score을 사용하여 정확도를 출력하였다. (정확도 72.41%)
* 예측값을 실제값과 지역을 추가하여 출력하였다.
## => SVM은 StandardScaler가 더 높았고 SVM 정확도가 꽤 나오는 것을 보아 데이터를 신뢰할만하다는 것을 알 수 있다.

### Logistic Regression
* 전처리로 StandardScaler를 사용하였다.
* 하이퍼 파라미터로 C를 사용하였다.
* for문을 통해 값을 바꿔가며 validation accuracy를 측정하여 pandas DataFrame으로 출력하였다.
* * 최종 모델링을 한 다음(C=0.01) test을 사용하여 confusion_matrix와 accuracy_score을 사용하여 정확도를 출력하였다. (정확도 74.14%)
* 예측값을 실제값과 지역을 추가하여 출력하였다.
* 회귀계수와 상수항을 출력하였다. 회귀계수: [[-0.    -0.014  0.001  0.     0.    -0.    -0.002 -0.019 -0.016 -0.005]]  상수항: [-0.]


