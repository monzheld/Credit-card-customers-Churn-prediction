# 카드사 고객 이탈 여부 예측 
머신러닝 모델을 활용한 카드사 고객 이탈 여부 예측

<br>

## 제작 기간
2022.01.07 ~ 2022.01.12

<br>

## 프로젝트 기획 배경 
카드사는 신용카드 혜택에 따라 이탈률이 높은 업종이므로 <br>
고객이 이탈하는데 어떤 특성들이 영향을 주는지 알아보고, 어떤 고객이 이탈할 것인지 미리 예측해 고객에게 더 나은 서비스를 제공할 수 있도록 하기 위해 기획

<br>

## 가설 검정 

- 낮은 등급의 카드를 소유한 고객일수록 이탈할 확률이 높다. -> **True**

- 카드사와 오래 거래한 고객일수록 이탈할 확률이 낮다. -> **True**

- 지난 12개월 동안의 총 거래 금액이 높을수록 이탈할 확률이 높다. -> **True**

<br>

## 사용 기술

#### EDA
- pandas
- numpy
- matplotlib
- seaborn

#### 머신러닝 모델링 
- LabelEncoder
- OrdinalEncoder
- SMOTETomek
- StratifiedKFold
- RandomizedSearchCV
- GridSearchCV
- XGBClassifier

#### 머신러닝 모델 해석
- pdpbox
- shap

<br>

## 제작 과정

#### 인코딩
- Gender, Marital_Status -> `LabelEncoder`를 사용한 라벨인코딩
- Customer_Age, Education_Level, Income_Category, Card_Category -> `OrdinalEncoder`를 사용한 ordinal 인코딩

#### 데이터 불균형
- `SMOTE`로 `oversampling`: 소수 범주에서 가상의 데이터를 생성해 분포를 맞추는 방법
- `SMOTETomek`: Oversampling과 Undersampling을 함께 수행하는 방법

=> SMOTETomek 데이터가 성능이 가장 높게 나왔으므로 `SMOTETomek` 데이터 사용


#### 기준 모델
- 이진 분류 문제이므로 타겟의 최빈값을 기준 모델로 설정
- 기준 모델 accuracy: 0.51

#### 모델 비교
- `LogisticRegression` / `DecisionTreeClassifier` / `RandomForestClassifier` / `XGBoost` 모델을 `StratifiedKFold`로 교차 검증
- 평균 정확도가 가장 높게 나온 `RandomForestClassifier`와 `XGBClassifier` 모델 비교 

=> `RandomizedSearchCV`한 XGBClassifier 모델의 성능이 더 높아 `XGBClassifier` 모델 선택

#### 최종 모델 선택 및 검증
- `GridSearchCV` 를 사용해 XGBClassifier 모델의 하이퍼파라미터 튜닝
- 최종 모델의 Test 데이터셋 `Recall` 점수: **0.92**

#### 모델 해석
- `pdpbox`
- `shap`
- 개별 고객 예측 해석

<br>

## 결론

### 머신러닝 모델
- XGBClassifier

### 평가지표 
- 재현율(Recall)

### Test 데이터셋 검증 정확도
- Recall: 0.92

### 고객이 카드사를 이탈하는 데 가장 많은 영향을 미친 특성

- 1위: Total_Trans_Ct(지난 12개월 동안의 총 거래 건수)

- 2위: Total_Trans_Amt(지난 12개월 동안의 총 거래 금액)

- 3위: Total_Revolving_Bal(신용 카드 총 리볼빙 잔액)

- 4위: Total_Relationship_Count(보유한 제품의 수)

=> 지난 12개월 동안의 **총 거래 건수가 적을수록**, 지난 12개월 동안의 **총 거래 금액이 높을수록**, 신용 카드의 **총 리볼빙 잔액이 적을수록**, **보유한 제품의 수가 적을수록** 고객이 카드사를 **이탈할 확률이 높음**.
