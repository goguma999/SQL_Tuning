# [DA] RFM 분석

### 📍RFM

- 고객 분류 기법 중 하나로 CRM 마케팅에서 고객 타겟팅으로 주로 활용
- **R ecency** 얼마나 최근에 구매했는가
- **F requency** 얼마나 자주 구매했는가
- **M onetary** 얼만큼의 규모로 구매했는가

| 사용자 | 최근 구매일 | 최근 1달 간 구매횟수 |  최근 1달 간 구매금액 |
| --- | --- | --- | --- |
| A | 2주 전 | 8회 | 220,000 |
| B | 어제 | 2회 | 180,000 |

사용자별로 RFM 점수를 산출해서 백분위수 개념으로 그룹으로 분류하고, 어떤 그룹이 이탈 가능성이 높을지 알아보자.

### 📍백분위수(percentile)

- **전체를 100으로 보고 작은 것부터(0) 큰 것까지(100) 차례로 나열했을 때 N번째 오는 수**를 의미
- ex
    - 영어 90점 (백분위수 70)
    - 수학 80점 (백분위수 95)
- 파이썬으로 백분위수 구하기
    - `Numpy` 함수
    - `percentil(값, [20,40,60])`

### 📍독립변수와 종속변수의 상관관계

- RFM 점수를 산출할 때 회귀계수를 구해서 각 변수에 곱해줘야 함
- **선형회귀** ?
    - 종속변수가 **연속형**으로 독립변수와 종속변수가 선형관계
    - 회귀계수 = 독립변수와 종속변수가 얼마나 강한 상관관계를 갖는지를 보여줌 = 기울기
    - 이탈율, 주문전환율 등
- **로지스틱 회귀** ? 종속변수가 범주형으로 **binary(0과 1)**로 표현 가능한 데이터
    - 성공/실패, 합격/불합격, 양성/음성, 복귀/이탈 등

### 📍이커머스 고객의 이탈가능성 예측

1. 종속변수: 최근 1달 이내 복귀 여부(복귀: 1 / 이탈: 0)
2. 독립변수: RFM(트레인 기간 내 마지막 주문일로부터 경과 일수 / 총 주문수 / 평균 주문금액)
3. 측정인원: 1천명
4. 분석방법: RFM 점수 산출하여 총 5그룹으로 고객 분류
        
    - 각각의 독립변수를 **백분위수 개념 활용해서 1~4점으로 환산**
    - Total score = 환산한 점수 *** 회귀계수** 들의 총합
        - monetary score는 회귀계수 곱했을 때 거의 0점에 가까움 = 종속변수와 상관성이 굉장히 낮음
        - 1~4 환산점수만 그대로 쓰면 영향력 없는 monetary도 그대로 반영돼서 예측 모델의 정확도 떨어짐
5. 기준 기간
    
    ```sql
             트레인 기간             예측 기간
    60일 전 ------------- 30일 전 ------------ 현재
            RFM 점수 산출           이탈여부 확인
    ```
    
6. python 코드

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

import sklearn
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split

df = pd.read_csv('https://raw.githubusercontent.com/min1112/Data_Analysis_Skill_Exercise/refs/heads/main/Part2.ch03_RFM/rfm_mart.csv')

df
df.info()
```

| mem_no | last_ord_dt | recency | frequency | monetary | is_back |
|--------|------------|---------|-----------|----------|---------|
| 1      | 2023-01-01 | 30      | 1         | 28700    | 0       |
| 2      | 2023-01-12 | 19      | 7         | 26929    | 0       |
| 3      | 2023-01-01 | 30      | 1         | 54800    | 0       |
| 4      | 2023-01-27 | 4       | 3         | 25700    | 0       |
| 5      | 2023-01-30 | 1       | 2         | 6500     | 0       |
| ...    | ...        | ...     | ...       | ...      | ...     |
| 996    | 2023-01-02 | 29      | 3         | 31000    | 1       |
| 997    | 2023-01-30 | 1       | 2         | 16450    | 1       |
| 998    | 2023-01-01 | 30      | 1         | 13600    | 0       |
| 999    | 2023-01-01 | 30      | 1         | 14200    | 1       |
| 1000   | 2023-01-27 | 4       | 2         | 24250    | 0       |



```python
# 로지스틱 회귀계수 구하기
x = df.drop(['mem_no','last_ord_dt','is_back'], axis=1)
y = df['is_back']

x_train, x_test, y_train, y_test = sklearn.model_selection.train_test_split(x, y)

model = LogisticRegression()
model.fit(x_train, y_train)

# test data로 모델의 예측도 확인
model.score(x_test, y_test)  #0.712 낮은 건 아니지만 실무에선 80 이상 만들기 위해 독립변수를 넣고 빼고 연구

coef = pd.DataFrame({'features': x.columns, 'coefficient':model.coef_[0]})
coef
```

| features | coefficienc |
| --- | --- |
| recency | -0.060135 |
| frequency | 0.082922 |
| monetary | -0.000001 |

```python
# 백분위수(Percencile) 기반으로 RFM 점수 산출하기
a1, a2, a3 = np.percentile(df['recency'],[20,40,60])
a1, a2, a3

def percent(x):
  if x <= a1:
    return 4
  elif x > a1 and x <= a2:
    return 3
  elif x > a2 and x < a3:
    return 2
  elif x >= a3:
    return 1
    
df['recency_score'] = df['recency'].apply(percent)*-coef.iloc[0,1]

# frequency, monetary score도 위와 같이 만들어줌

df['total_score'] = df['recency_score'] + df['frequency_score'] + df['monetary_score']
df
```
| mem_no | last_ord_dt | recency | frequency | monetary | is_back | recency_score | frequency_score | monetary_score | total_score |
|--------|------------|---------|-----------|----------|---------|---------------|----------------|----------------|-------------|
| 1      | 2023-01-01 | 30      | 1         | 28700    | 0       | 0.060135      | 0.082922       | 0.000004       | 0.143061    |
| 2      | 2023-01-12 | 19      | 7         | 26929    | 0       | 0.180405      | 0.331687       | 0.000003       | 0.512095    |
| 3      | 2023-01-01 | 30      | 1         | 54800    | 0       | 0.060135      | 0.082922       | 0.000004       | 0.143061    |
| 4      | 2023-01-27 | 4       | 3         | 25700    | 1       | 0.240540      | 0.248765       | 0.000003       | 0.489308    |
| 5      | 2023-01-30 | 1       | 2         | 6500     | 0       | 0.060135      | 0.165843       | 0.000001       | 0.225979    |
| ...    | ...        | ...     | ...       | ...      | ...     | ...           | ...            | ...            | ...         |
| 996    | 2023-01-02 | 29      | 3         | 31000    | 1       | 0.120270      | 0.248765       | 0.000004       | 0.369039    |
| 997    | 2023-01-30 | 1       | 2         | 16450    | 1       | 0.240540      | 0.165843       | 0.000003       | 0.406384    |
| 998    | 2023-01-01 | 30      | 1         | 13600    | 0       | 0.060135      | 0.082922       | 0.000002       | 0.143058    |
| 999    | 2023-01-01 | 30      | 1         | 14200    | 1       | 0.060135      | 0.082922       | 0.000002       | 0.143058    |
| 1000   | 2023-01-27 | 4       | 2         | 24250    | 0       | 0.240540      | 0.165843       | 0.000003       | 0.406386    |



```python
# total_score 기준 5그룹 분류
t1, t2, t3, t4 = np.percentile(df['total_score'],[20,50,70,90])
t1, t2, t3, t4

def level(x):
  if x <= t1:
    return 5
  elif x > t1 and x <= t2:
    return 4
  elif x > t2 and x <= t3:
    return 3
  elif x > t3 and x <= t4:
    return 2
  elif x > t4:
    return 1

df['level'] = df['total_score'].apply(level)

df.level.value_counts()
```

| level | count |
| --- | --- |
| 5 | 274 |
| 4 | 228 |
| 3 | 214 |
| 2 | 185 |
| 1 | 99 |

```python
# 5그룹별로 리텐션(1-리텐션) 구하기
pivot = df.groupby('level').agg({'is_back':'sum','mem_no':'count'})
pivot['retention'] = pivot.is_back/pivot.mem_no

pivot
```

| **level** | is_back | mem_no | retention |
| --- | --- | --- | --- |
| **1** | 88 | 99 | 0.888889 |
| **2** | 90 | 185 | 0.486486 |
| **3** | 107 | 214 | 0.500000 |
| **4** | 59 | 228 | 0.258772 |
| **5** | 74 | 274 | 0.270073 |