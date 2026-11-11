# 반도체 공정 데이터 분석 프로젝트
## 1. 프로젝트 개요
ㅇㅇㅇㅇㅇ

## 2. 프로젝트 진행과정
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

 특히 본 데이터는 정상(Pass)과 불량(Fail)의 데이터 비율이 크게 불균형한 상태이므로, 이상치 처리 과정에서 Fail 데이터를 제거할 경우 클래스 불균형이 더욱 심화될 가능성이 있어 이상치 제거 기준을 신중하게 선정할 필요가 있었다.  
 또한 Mean Difference(d)의 기준을 0.3으로 완화하는 경우, 해당 값을 통계적으로 의미 있는 차이로 판단하여 상위 20개 Feature만 선정할지 또는 기준을 충족하는 모든 Feature를 활용할지에 대한 추가적인 분석 방법도 제시하였다. 

### - 4주차 | Machine Learning 
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
