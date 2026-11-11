# 반도체 공정 데이터 분석 프로젝트
ㅇㅇㅇㅇㅇ

## 진행과정
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

> 결측률


