# 반도체 공정 데이터 분석 프로젝트
## 프로젝트 개요
반도체 공정은 수많은 공정 단계와 장비, 센서 변수가 복합적으로 작용하는 고도의 정밀 산업으로, 미세한 공정 조건의 변화만으로도 제품의 품질과 수율에 영향을 미칠 수 있다. 이에 따라 공정 과정에서 수집되는 데이터를 체계적으로 분석하고, 잠재적인 이상 징후를 사전에 파악하는 것은 불량 발생을 예방하고 생산 수율을 향상시키는 데 중요한 요소이다.

그러나 실제 공정데이터는 수백 개 이상의 센서 변수로 구성되어 있어 변수 간 복잡한 상관관계와 불필요한 정보가 포함될 수 있으며, 정상(Pass)과 불량(Fail) 데이터 간의 비율 차이가 큰 클래스 불균형 문제 또한 존재한다. 특히 불량 데이터가 극히 적은 환경에서는 일반적인 머신러닝 모델이 정상 데이터에 편향되어 실제 불량을 정상으로 오판할 가능성이 있다.

본 프로젝트에서는 Kaggle의 SECOM 반도체 공정데이터를 활용하여 데이터 이해 및 전처리·EDA, Feature Selection, ML 모델링 및 성능평가를 통해 공정 데이터 불량 예측에 영향을 미치는 주 센서 변수를 도출해내는 과정을 진행하였다.

<br><br>

## 프로젝트 진행과정
### - 1주차 | 데이터 이해 
 데이터 구조와 특성을 파악하는 과정은 분석의 정확성과 신뢰성을 높이기 위해 선행되어야 한다. Kaggle SECOM 데이터셋의 변수 구성, 결측치, 데이터 분포 및 정상 · 불량 데이터 비율 등을 확인하여 데이터의 특성을 이해하고, 이후 전처리 및 머신러닝 모델 구축에 필요한 기초를 마련하였다.

- 데이터 구조 파악
<img width="1117" height="307" alt="image" src="https://github.com/user-attachments/assets/e4bc6cb8-4486-415f-abf8-860ac3a8d8b6" />

- EDA (Exploratory Data Analysis, 탐색적 데이터 분석) 
<img width="1126" height="517" alt="image" src="https://github.com/user-attachments/assets/a709412c-0589-47cd-b77b-77a6b7fedf05" />  
*상수 변수는 데이터의 변별 정보를 거의 제공하지 않으므로 후속 전처리 및 Feature Selection 단계에서 제거 여부를 검토할 필요 존재
<br><br>
 본 데이터의 결측치 제거 기준 설정에 있어 결측률이 25% 이상인 경우와 50% 이상인 경우로 구분하여 각각 전처리를 진행하고 이후 두 데이터셋의 정상(Pass)과 불량(Fail)에 대한 변수별 분포 및 Boxplot을 비교 · 분석하는 것으로 결정하였다.  

<br><br>
 
### - 2주차 | 전처리 · EDA 
결측률이 25% 이상인 경우와 50% 이상인 데이터를 각각 전처리를 진행하고 이에 대한 정상(Pass)과 불량(Fail)에 대한 변수별 분포 및 Boxplot을 표시하면 다음과 같다. 

- 결측률 25% dataset 
<img width="1020" height="632" alt="image" src="https://github.com/user-attachments/assets/e4a18329-9efa-40f7-8c32-2a6c43ccff7b" />
<img width="1092" height="777" alt="image" src="https://github.com/user-attachments/assets/2977296c-de58-476f-b764-62690ebc4625" />  
*전체 행 수: 1,565 Feature 수 : 442 평균 결측률: 1.13% 

<br><br>

- 결측률 50% dataset 
<img width="895" height="570" alt="image" src="https://github.com/user-attachments/assets/d8f8ca5b-d101-4fd3-8fc5-99c7c153d104" />  
<img width="896" height="647" alt="image" src="https://github.com/user-attachments/assets/7820b2f6-7e20-4dea-bf0a-709a3f0af476" />    

*전체 행 수: 1,567 Feature 수 : 446 평균 결측률: 1.56%  

<br><br>

- Boxplot
<img width="1207" height="835" alt="image" src="https://github.com/user-attachments/assets/e054cd68-6ebc-465a-a044-9c95a91ce16f" />

<br><br>

> - 결측률  
> > 결측값 개수 ÷ 전체 데이터 개수 × 100
> - IQR 기반 이상치 후보 
> > IQR = Q3 - Q1  
> > 하한 = Q1 - 1.5 × IQR  
> > 상한 = Q3 + 1.5 × IQR  
> > → 하한보다 작거나 상한보다 큰 값은 통계적인 이상치 후보로 센다.
> - 정상/불량 평균 차이 지표  
> > Mean Difference(d) = | Pass 평균 - Fail 평균 | ÷ 두 집단 표준편차의 결합값   
> > → 값이 클수록 정상과 불량의 중심 위치가 상대적으로 많이 떨어져 있다는 뜻으로 시각화 우선순위를 정하는 데 사용

위 그래프를 통해 정상(Pass)과 불량(Fail) 데이터의 분포가 극단적으로 불균형한 것을 시각적으로 확인할 수 있다. 이에 따라 정상과 불량 간 차이가 크게 나타나는 Feature를 중심으로 분석을 진행하였으며, 결측률 25% Dataset을 최종 분석 Dataset으로 선정하였다.  또한 정상과 불량 간 차이가 크게 나타나는 변수를 기존 Top 20에서 Top 50까지 확대하고 차이가 작은 변수 또한 확인하여 보다 다양한 Feature의 분포와 특성을 보고자 하였다.  

<br><br>

### - 3주차 | feature Selection
다음은 정상(Pass)과 불량(Fail) 간 차이가 큰 변수와 작은 변수를 각각 Top 50으로 선정하여 ±3 IQR 기준을 적용한 Boxplot으로 분포를 비교한 결과이다.
<img width="1122" height="810" alt="image" src="https://github.com/user-attachments/assets/db5b76d8-bb6e-47fb-919d-1677532919e0" />
<img width="1122" height="807" alt="image" src="https://github.com/user-attachments/assets/cfd2a0bd-cb47-4abd-ae62-56d39afaaa08" />
<img width="1221" height="851" alt="image" src="https://github.com/user-attachments/assets/a770b47c-aaf2-4d0f-a8c4-416b8beff674" />

 일반적으로 Mean Difference(d)가 0.8보다 큰 Feature를 이상치로 판단하여 제거하는 방법을 고려할 수 있으나, 위 그래프에서는 해당 기준을 만족하는 Feature가 존재하지 않았다. 이에 따라 제시한 이상치 처리 방법은 다음과 같다.

>  ① 6시그마 기준을 적용하여 이상치를 일괄 제거하는 방법  
>  ② 상위 N%의 주요 변수는 이상치를 유지하고 나머지 변수의 이상치를 제거하는 방법  
>  ③ 머신러닝 기반 변수 중요도를 활용하여 주요 Feature를 선정하고 데이터 내 패턴을 분석하는 방법  

<br><br>
 특히 본 데이터는 정상(Pass)과 불량(Fail)의 데이터 비율이 크게 불균형한 상태이므로, 이상치 처리 과정에서 Fail 데이터를 제거할 경우 클래스 불균형이 더욱 심화될 가능성이 있어 이상치 제거 기준을 신중하게 선정할 필요가 있었다.  
 또한 Mean Difference(d)의 기준을 0.3으로 완화하는 경우, 해당 값을 통계적으로 의미 있는 차이로 판단하여 상위 20개 Feature만 선정할지 또는 기준을 충족하는 모든 Feature를 활용할지에 대한 추가적인 분석 방법도 제시하였다. 

<br><br>

- 데이터 전처리 과정에서는 센서 변수에 존재하는 결측치를 해당 변수의 중앙값으로 대체하였다. 중앙값은 평균값과 달리 극단적인 이상치의 영향을 상대적으로 적게 받기 때문에 센서 데이터의 대표값으로 활용하기에 적합하다고 판단하였다.
- 이상치의 경우 단순히 해당 데이터를 제거하지 않고, 상한값과 하한값을 기준으로 범위를 벗어나는 센서값을 각각의 경계값으로 대체하는 방식을 적용하였다. 이는 이상치가 반드시 실제 공정의 불량 상태를 의미하는 것은 아니기 때문이다. 센서의 순간적인 측정 오류나 외부 노이즈 등으로 인해 비정상적으로 큰 값 또는 작은 값이 기록될 수 있으며, 이러한 측정값이 발생하더라도 실제 제품은 정상(Pass)으로 판정될 가능성이 있다.
따라서 이상치를 무조건 제거할 경우 정상 제품에 해당하는 데이터까지 손실될 수 있다고 판단하였다. 이에 데이터의 손실을 최소화하면서 극단적인 센서값이 모델 학습에 미치는 영향을 완화하기 위해 상한값과 하한값을 활용한 이상치 대체 방식을 적용하였다.

<br><br>

### - 4주차 | ML modeling
반도체 공정 특성상 불량은 정상 제품에 비해 상대적으로 드물게 발생하므로 본 데이터 역시 극단적인 클래스 불균형(Class Imbalance)을 나타낸다. 이러한 환경에서는 머신러닝 모델이 정상 데이터에 편향되어 학습될 가능성이 있으며, 결과적으로 실제 불량 데이터를 정상으로 판단하는 문제가 발생할 수 있다.  
따라서 본 분석에서는 단순히 전체 예측 정확도를 높이는 것보다 희귀한 불량 데이터를 얼마나 놓치지 않고 검출할 수 있는지, 즉 재현율(Recall)을 중요 평가 기준으로 설정하였다. 
<br><br>
본 분석에서는 총 5가지 머신러닝 모델을 선정하고, 각 모델의 성능을 비교하기 위해 3가지 평가 지표를 선정하였다. 이에 따라 동일한 데이터와 평가 기준을 적용하여 5가지 머신러닝 모델의 성능을 진단하였으며, 그 결과는 다음과 같다. 

> ML 모델
> > - KNN (최근접 이웃)  
> > 새 제품이 들어오면 가장 닮은 과거 제품 5개를 살펴보 고 다수결로 판정하는 직관적 방식 
> > - 의사결정나무  
> > 스무고개 질문 트리로 판별. 희귀 불량품을 찾도록 가중 치 균형(class-weight='balanced') 지시를 적용 
> > - 로지스틱 회귀  
> > 각 센서 측정값의 중요도를 합산해 불량 발생 확률을 수 학적으로 계산하는 전통적 기준 모델
> > - Random Forest
> > 수백 개의 의사결정나무들이 투표하여 결정하는 앙상블. 안정적이지만 불균형 시 다수에 휩쓸리기 쉬움
> > - XGBoost (경사 하강 부스팅)
> > 이전 모델의 오답을 집중적으로 보완하는 최고 성능의 최신 알고리즘. 극단 불균형에는 별 도 튜닝 필요

<br><br>

> ML 평가기준
> > - 재현율 (Recall)  
> > "실제 불량품 중 몇 개를 잡았는가?"  
> > → 공장 입장에서 가장 중요한 지표. 불량품 100개 중 34 개를 잡았다면 재현율은 34% / 0점이면 불량품 이 시중에 그대로 풀린다는 의미
> > - 정밀도 (Precision)  
> > "불량이라 지목한 것 중 진짜 불량은?"  
> > → AI가 불량이라 판단을 내렸을 때 실제로 불량품 일 확률로 너무 낮으면 정상 제품까지 쓸데없이 폐기하게 됨
> > - F1-Score/ PR-AUC  
> > "정밀도와 재현율의 균형 점수"    
> > → 정확도의 착시에 속지 않기 위해 두 지표의 조화 평균을 낸 점수로 불균형 데이터셋에서는 이 점수가 최종 합격 기준이 됨.

<br><br>

> ML 성능 평가
<img width="1512" height="385" alt="image" src="https://github.com/user-attachments/assets/fdfe22dd-c7e8-44d8-9703-1b58d1645b2a" />
초기 머신러닝 모델의 성능을 비교한 결과, 일부 모델에서 높은 Accuracy를 기록하였으나 실제 불량 데이터의 탐지 성능은 전반적으로 낮게 나타났다. 특히 Random Forest와 XGBoost는 각각 0.9336, 0.9343의 높은 정확도를 보였지만, Recall과 Precision이 모두 0으로 나타나 실제 불량 데이터를 하나도 탐지하지 못하는 결과를 보였다.

반면 Decision Tree와 Logistic Regression은 각각 0.3171, 0.3471의 Recall을 기록하여 일부 불량 데이터를 탐지하였다. 특히 Logistic Regression은 34.71%의 가장 높은 불량 탐지율을 보였으나, Precision이 0.0573으로 낮아 불량으로 예측한 데이터 중 실제 불량의 비율이 매우 낮다는 한계가 나타났다. KNN 역시 0.9324의 Accuracy를 기록하였지만 Recall과 Precision이 모두 0으로 나타나 불량 탐지에 실패하였다.
<br><br>

 ### - 5,6주차 | ML 최종 성능 평가 및 불량 변수 도출
본 데이터셋은 Feature(변수)의 수가 많고 정상(Pass)과 불량(Fail) 간의 클래스 불균형이 심하다는 특징이 있다. 이러한 데이터 환경에서는 모델이 다수 클래스인 정상 데이터에 편향되어 모든 제품을 정상으로 분류하고, 실제 불량품을 탐지하지 못하는 문제가 발생할 수 있다. 이를 방지하기 위해 데이터 정제부터 모델 학습 및 성능 평가에 이르는 전반적인 머신러닝 파이프라인 구조를 설계하였다.

> 1. 방어 (Robustness)
> > 누수 방지 설계
> > > Data Leakage 원천 차단. 5-Fold 교차 검증의 Train FoId 내부에 전처리 과정(Scaling, lmputation)을 완전 격리.
> 2. 정제 (Precision)
> > 다단계 변수 압축
> > > 3단계 Feature SeIection. 590개의 노이즈 섞인 센서 데이터 중 불량 판별의 핵심이 되는30개 센서만 정밀 타겟팅.
> 3. 대응 (Countermeasure)
> > 불균형 집중 타격
> > > 소수 클래스 증강 및 감도 조절. Train 내부 SMOTE 적용 및 임계값(Threshold) 하향을 통한 미탐방지.

<br><br>

- 학습 데이터(Train)가 검증 및 평가 데이터에 유입되는 데이터 누수(Data Leakage)를 방지하기 위해, 학습 데이터와 평가 데이터의 독립성을 유지하도록 다음과 같은 구조로 설계하였다.
<img width="2094" height="751" alt="image" src="https://github.com/user-attachments/assets/abfaadda-1c2a-48b5-91ab-424154a3a2d1" />
<br><br>

```python
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

for train_idx, val_idx in skf.split(X, y):
    # [Data Leakage 방지] Train 데이터 기준으로만 fit 수행
    imputer = SimpleImputer(strategy='median')
    scaler = StandardScaler()

    X_train_imp = pd.DataFrame(imputer.fit_transform(X_train), columns=X.columns)
    X_val_imp = pd.DataFrame(imputer.transform(X_val), columns=X.columns)
```

<br><br>

- 총 590개의 센서 변수(Feature)를 모델 학습에 활용할 경우 중복된 정보나 불필요한 노이즈로 인해 모델의 복잡도가 증가하고 학습 효율이 저하될 수 있다. 이에 따라 정상(Pass)과 불량(Fail)의 구분에 유의미한 영향을 미치는 핵심 변수를 선별하기 위해 3단계의 변수 선택(Feature Selection) 과정을 설계하였다.
<br><br>
<img width="1302" height="585" alt="image" src="https://github.com/user-attachments/assets/fa06412e-df40-435f-8ef6-c10669f27887" />
<img width="797" height="502" alt="image" src="https://github.com/user-attachments/assets/a9dc78ac-955d-4bcb-b62b-7d0994f350a1" />
<br><br>

```python
def apply_feature_selection(X_train, y_train, X_val, n_features=30):
# 1단계: 중복 정보 센서 제거
    corr_matrix = X_train.corr().abs()
    upper = corr_matrix.where(np.triu(np.ones(corr_matrix.shape), k=1).astype(bool))
    to_drop = [column for column in upper.columns if any(upper[column] > 0.95)]
    X_train_filter = X_train.drop(columns=to_drop)

# 2단계: 불량 관련성 100개픽
    k_anova = min(100, X_train_filter.shape[1])
    selector_f = SelectKBest(score_func=f_classif, k=k_anova)
    selector_f.fit(X_train_filter, y_train)
    cols_anova = X_train_filter.columns[selector_f.get_support()]
    X_train_f = X_train_filter[cols_anova]

#3단계: 머신러닝기반 30개 최종 압축
    rfe_estimator = LogisticRegression(class_weight='balanced', max_iter=1000,
                                        random_state=42)
    k_rfe = min(n_features, X_train_f.shape[1])
    rfe = RFE(estimator=rfe_estimator, n_features_to_select=k_rfe, step=0.1)
    rfe.fit(X_train_f, y_train)
    selected_cols = X_train_f.columns[rfe.get_support()]
    return X_train_f[selected_cols], X_val_f[selected_cols], list(selected_cols)
```

<br><br>

- 또한 데이터셋에 정상(Pass) 데이터에 비해 불량(Fail) 데이터의 수가 극히 적은 클래스 불균형 문제가 존재한다. 이러한 불균형은 모델이 다수 클래스인 정상 데이터에 편향되어 소수 클래스인 불량 데이터를 제대로 학습하지 못하는 원인이 될 수 있다. 이를 개선하기 위해 SMOTE 증강으로 소수의 불량 데이터에 가상의 유효한 data를 추가하는 오버샘플링을 통해 학습 데이터의 비율을 보완하였다. 이때 증강은 학습 데이터(Train)에만 적용하였으며, 평가 데이터(Test)는 원본 분포를 유지하도록 별도의 증강 과정을 수행하지 않았다. 이를 통해 실제 불량 데이터에 대한 모델의 탐지 및 예측 성능을 객관적으로 평가하고자 하였다.
<br><br>
<img width="1292" height="472" alt="image" src="https://github.com/user-attachments/assets/bdf20693-55fd-4fbc-96c2-6fa639ab13b9" />
<br><br>

```python
smote = SMOTE(random_state=42)
X_train_res, y_train_res = \
    smote.fit_resample(X_train_proc, y_train)
```

<br><br>

- 본 프로젝트에서는 반도체 공정 데이터의 불량 여부를 예측하기 위해 총 5가지 머신러닝 분류 모델(Logistic Regression, Decision Tree, Random Forest, XGBoost, kNN)을 구축하였다. 각 모델은 선형 분류, 의사결정 규칙, 앙상블 학습 및 거리 기반 분류 등 서로 다른 알고리즘적 특성을 가지고 있어, 다양한 관점에서 데이터의 분류 성능을 비교하고자 선정하였다.
 또한 정상(Pass)과 불량(Fail) 간의 극심한 클래스 불균형 문제를 고려하여 소수 클래스인 불량 데이터에 상대적으로 높은 학습 가중치를 부여하도록 설계하였다. 이를 통해 다수 클래스인 정상 데이터에 대한 모델의 편향을 완화하고, 불량 데이터의 학습 비중을 높여 불량 탐지 성능을 개선하고자 하였다.
최종적으로 각 모델의 예측 결과를 다양한 평가지표를 통해 비교·분석하고, 실제 불량 제품을 얼마나 효과적으로 탐지할 수 있는지 평가하여 최적의 분류 모델을 선정하고자 하였다.

> - Logistic Regression  
선형  BaseIine / 계수를 통한 동한 높은 해석력 확보  
> - Decision Tree  
기초 비선형 패턴 파악 / 직관적 S(RuIe) 분기  
> - Random Forest  
Bagging 앙상블 / 이상치에 강건한 다수결 판정  
> - XGBoost  
Boosting 기반 고성능 / 불량 탐지 성능 극대화 후보
> - kNN  
거리 기반 유사도 측정

```python
scale_pos_weight = (len(y) - sum(y)) / sum(y)
models = {
    'kNN': KNeighborsClassifier(n_neighbors=5),
    'Decision Tree': DecisionTreeClassifier(class_weight='balanced', max_depth=5, random_state=42),
    'Logistic Regression': LogisticRegression(class_weight='balanced', max_iter=1000, random_state=42),
    'Random Forest': RandomForestClassifier(class_weight='balanced', n_estimators=100, random_state=42),
    'XGBoost': XGBClassifier(scale_pos_weight=scale_pos_weight, eval_metric='logloss', random_state=42)
}
```

<br><br>

- 불량 제품을 정상으로 판정하는 'False Negative'는 실제 불량품이 고객사에 인도될 수 있다는 점에서 가장 치명적인 위험 요소로 판단하였다. 이에 따라 일반적인 분류 기준인 50%보다 낮은 30%를 불량 판정 임계값(CUSTOM_THRESHOLD = 0.3)으로 하향 조정하였다. 즉, 모델이 예측한 불량 확률이 30% 이상일 경우 불량으로 판정하도록 기준을 조정하였다. 이를 통해 불량 제품에 대한 탐지 민감도를 높이고, 실제 불량품을 정상으로 오인하는 False Negative 발생을 최소화하고자 했다.
<br><br>
<img width="1046" height="425" alt="image" src="https://github.com/user-attachments/assets/966c3103-a037-4c64-851a-bed11cad2d42" />
<br><br>

```python
CUSTOM_THRESHOLD = 0.3

model.fit(X_train_res, y_train_res)
y_proba = model.predict_proba(X_val_proc)[:, 1]
y_pred_custom = (y_proba >= CUSTOM_THRESHOLD).astype(int)
```

<br><br>

- 일반적인 정확도(Accuracy)는 정상 데이터의 비율이 높은 불균형 데이터 환경에서 실제 불량 탐지 성능을 제대로 반영하지 못하는 한계가 있다. 이에 따라 본 프로젝트에서는 불량 데이터와 같은 소수 클래스의 예측 성능을 중점적으로 평가하기 위해 PR-AUC(Precision-Recall AUC)를 주요 평가 지표로 선정하였다. PR-AUC는 불량 탐지율(Recall)과 불량 예측의 정확도(Precision) 간의 관계를 종합적으로 평가하며, 실제 불량 데이터를 얼마나 효과적으로 식별하는지 확인할 수 있다. 이를 통해 단순히 전체 예측 정확도를 높이는 것보다 불량 데이터의 탐지 성능을 우선적으로 고려하여 모델을 평가하고자 하였다.
<br><br>
<img width="1292" height="515" alt="image" src="https://github.com/user-attachments/assets/35789f20-ec19-4aeb-9687-25f8c75b856a" />
<br><br>

```python
precision_vals, recall_vals, _ = precision_recall_curve(y_val, y_proba)
pr_auc = auc(recall_vals, precision_vals)
metrics['Recall'].append(recall_score(y_val, y_pred_custom, zero_division=0))
metrics['PR-AUC'].append(pr_auc)
```

<br><br>

- Feature Selection과 SMOTE를 통한 데이터 불균형 보완, 분류 임계값(Threshold) 조정 과정을 적용한 후 5가지 머신러닝 모델의 최종 성능을 비교·평가하였다. 각 모델의 Accuracy, Precision, Recall, F1-Score 및 PR-AUC를 산출하여 전반적인 분류 성능과 불량 탐지 능력을 분석하였으며, 특히 실제 불량 데이터의 탐지 성능을 중점적으로 확인하기 위해 Recall과 PR-AUC를 주요 평가 지표로 설정하였다. 또한 Precision-Recall Curve를 시각화하고, 불량 데이터의 전체 비율을 기준선(Baseline)으로 설정하여 각 모델의 성능을 비교하였다.
<br><br>

```python
plt.figure(figsize=(9, 6))[cite: 9]
for model_name, (rec, prec, score) in pr_curves_data.items():
    plt.plot(rec, prec, label=f'{model_name} (PR-AUC = {score:.4f})')
baseline_rate = sum(y) / len(y)[cite: 9]
plt.axhline(y=baseline_rate, color='r', linestyle='--', label=f'Baseline ({baseline_rate:.4f})')
plt.show()
```

<br><br>
<img width="1012" height="405" alt="image" src="https://github.com/user-attachments/assets/dab9b9ca-6a3d-476b-8fe8-6fc8b895b58b" />
<img width="1036" height="670" alt="image" src="https://github.com/user-attachments/assets/1e58582f-8c41-47ad-9a11-1636264050dd" />
그 결과, KNN 모델이 PR-AUC 0.2511로 가장 높은 값을 기록하였으며, Recall 또한 0.6519로 가장 우수한 불량 탐지율을 나타냈다. 이는 불량 데이터 비율을 기준으로 설정한 Baseline(0.0665)보다 약 3.77배 높은 수치로, 다른 모델에 비해 불량 데이터 탐지 측면에서 상대적으로 우수한 성능을 보였다. 반면 XGBoost는 Accuracy(0.8620)와 Precision(0.1834)에서 가장 높은 값을 기록하였으나, Recall은 0.3076으로 상대적으로 낮아 실제 불량 데이터를 탐지하는 비율에서 한계를 보였다.

<br><br>

- 최종 모델의 학습 결과를 바탕으로 공정 불량 예측에 영향을 미치는 주요 센서 변수를 확인하기 위해 Feature Importance 분석을 수행하였다. 분석 결과, Sensor_45, Sensor_82, Sensor_12가 상위 3개 변수로 도출되었으며, 나머지 센서 변수에 비해 상대적으로 높은 중요도를 나타냈다. 이는 해당 센서들이 모델의 불량 여부 예측에 큰 영향을 미치는 주요 변수임을 의미한다.
<img width="1024" height="765" alt="image" src="https://github.com/user-attachments/assets/4bd8c18d-7119-4290-9cc1-55057fdd063f" />

