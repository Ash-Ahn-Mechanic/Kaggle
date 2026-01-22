# Course 7: Machine Learning Competitions (Lesson Work Flow)

**전체 Lesson의 코드 흐름**을 데이터 로딩 → 모델 검증 → 전체 학습 → 예측 → 제출 파일 생성  
순서로 정리한 md 파일이다.

---

## 1. Introduction

이 Lesson의 목표는 다음과 같다.

- Kaggle 대회용 예측값 생성
- 지금까지 배운 내용을 실제 **제출(submission)** 까지 연결
- Random Forest 모델을 활용한 실전 워크플로우 이해

---

## 2. 코드 체크 및 파일 경로 설정

```python
from learntools.core import binder
binder.bind(globals())
from learntools.machine_learning.ex7 import *

import os
if not os.path.exists("../input/train.csv"):
    os.symlink("../input/home-data-for-ml-course/train.csv",
               "../input/train.csv")
    os.symlink("../input/home-data-for-ml-course/test.csv",
               "../input/test.csv")
```

---

## 3. 라이브러리 불러오기 및 데이터 로딩

```python
import pandas as pd
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error
from sklearn.model_selection import train_test_split

# Load training data
iowa_file_path = "../input/train.csv"
home_data = pd.read_csv(iowa_file_path)

# Target
y = home_data.SalePrice
```

---

## 4. Feature 선택 및 X 생성

```python
features = [
    'LotArea', 'YearBuilt', '1stFlrSF',
    '2ndFlrSF', 'FullBath',
    'BedroomAbvGr', 'TotRmsAbvGrd'
]

X = home_data[features]
```

---

## 5. Train / Validation 분리

```python
train_X, val_X, train_y, val_y = train_test_split(
    X, y, random_state=1
)
```

---

## 6. Random Forest 모델 정의 및 검증

```python
rf_model = RandomForestRegressor(random_state=1)
rf_model.fit(train_X, train_y)

rf_val_predictions = rf_model.predict(val_X)
rf_val_mae = mean_absolute_error(rf_val_predictions, val_y)

print("Validation MAE for Random Forest Model: {:,.0f}".format(rf_val_mae))
```

---

## 7. 전체 데이터로 모델 재학습

```python
rf_model_on_full_data = RandomForestRegressor()
rf_model_on_full_data.fit(X, y)
```

---

## 8. Test 데이터 예측

```python
test_data_path = "../input/test.csv"
test_data = pd.read_csv(test_data_path)

test_X = test_data[features]
test_preds = rf_model_on_full_data.predict(test_X)
```

---

## 9. Submission 파일 생성

```python
output = pd.DataFrame({
    'Id': test_data.Id,
    'SalePrice': test_preds
})

output.to_csv("submission.csv", index=False)
```

---

## 10. 전체 흐름 요약

1. 데이터 로딩 및 Feature 선택  
2. Train / Validation 분리  
3. Random Forest 모델 검증  
4. 전체 데이터로 재학습  
5. Test 데이터 예측  
6. Kaggle 제출 파일 생성  

---

## 핵심 정리

- Random Forest는 **튜닝 없이도 강력한 기본 성능** 제공
- Validation을 통해 모델 신뢰성 확보
- Kaggle 실전 문제의 표준 워크플로우 완성
