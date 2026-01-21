# Course 4: Model Validation (Report)

## 1. 왜 모델 검증(Model Validation)이 필요한가

모델을 한 번 만들었다고 끝이 아니다. **모델이 얼마나 잘 작동하는지 측정**하는 과정이 반드시 필요하다. 이 과정을 *모델 검증(Model Validation)* 이라고 한다.

대부분의 머신러닝 문제에서 모델 품질을 판단하는 핵심 기준은 **예측 정확도(Predictive Accuracy)** 이다. 즉,

> 모델의 예측값이 실제 값과 얼마나 가까운가?

를 정량적으로 측정해야 한다.

---

## 2. 모델 품질을 숫자로 요약해야 하는 이유

집 10,000채에 대해
- 실제 가격
- 예측 가격

을 하나하나 비교하는 것은 의미가 없다. 따라서 **모델 성능을 하나의 숫자로 요약하는 지표(metric)** 가 필요하다.

여러 지표가 있지만, 이 코스에서는 가장 직관적인 지표인 **MAE (Mean Absolute Error)** 를 사용한다.

---

## 3. MAE (Mean Absolute Error)란?

### (1) Error 정의

각 샘플에 대해 예측 오차(error)는 다음과 같이 정의된다.

```
error = actual − predicted
```

예시:
- 실제 집값: 150,000
- 예측 집값: 100,000
- 오차: 50,000

---

### (2) Absolute Error

오차는 음수가 될 수도 있으므로, **절댓값**을 취한다.

---

### (3) Mean Absolute Error

모든 샘플의 절댓값 오차를 평균낸 값이 MAE이다.

즉, MAE는 다음과 같이 해석할 수 있다.

> 평균적으로, 우리의 예측은 실제 값에서 약 X만큼 벗어나 있다.

---

## 4. In-Sample 평가의 문제점

### 잘못된 접근

많은 초보자들이 다음과 같은 실수를 한다.

- 훈련 데이터로 모델 학습
- **같은 데이터로 예측 및 성능 평가**

이렇게 얻은 성능을 **In-Sample Score** 라고 한다.

---

### 왜 문제가 되는가?

모델은 훈련 데이터에서 **우연히 나타난 패턴**까지 학습할 수 있다.

예시:
- 실제로는 집값과 무관한 "문 색깔"
- 훈련 데이터에서는 우연히 초록색 문 집들이 비쌌음

→ 모델은 "초록색 문 = 비싼 집" 이라는 **가짜 패턴**을 학습

훈련 데이터에서는 정확해 보이지만,
**새로운 데이터에서는 완전히 실패**하게 된다.

---

## 5. 해결책: Validation Data

모델의 진짜 성능은 **보지 않은 데이터(new data)** 에서 평가해야 한다.

이를 위해 데이터를 다음과 같이 나눈다.

- Training Data: 모델 학습용
- Validation Data: 모델 평가용

이렇게 하면 **실제 사용 상황과 유사한 성능 평가**가 가능하다.

---

## 6. 데이터 분할과 MAE 계산 코드

### (1) 데이터 로딩 및 전처리

```python
import pandas as pd

melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path)

# 결측치 제거
filtered_melbourne_data = melbourne_data.dropna(axis=0)

# Target과 Features 정의
y = filtered_melbourne_data.Price
melbourne_features = [
    'Rooms', 'Bathroom', 'Landsize', 'BuildingArea',
    'YearBuilt', 'Lattitude', 'Longtitude'
]
X = filtered_melbourne_data[melbourne_features]
```

---

### (2) Train / Validation 분리

```python
from sklearn.model_selection import train_test_split

train_X, val_X, train_y, val_y = train_test_split(
    X, y, random_state=0
)
```

---

### (3) 모델 학습 및 검증

```python
from sklearn.tree import DecisionTreeRegressor
from sklearn.metrics import mean_absolute_error

melbourne_model = DecisionTreeRegressor()
melbourne_model.fit(train_X, train_y)

val_predictions = melbourne_model.predict(val_X)
print(mean_absolute_error(val_y, val_predictions))
```

출력 결과:

```
265806.91478373145
```

---

## 7. 결과 해석

- In-Sample MAE: 약 **500 달러**
- Validation MAE: 약 **250,000 달러**

이는 훈련 데이터에서의 성능이 **현실을 전혀 반영하지 못했다**는 뜻이다.

참고로:
- 검증 데이터 평균 집값 ≈ 1,100,000 달러
- 오차가 평균 가격의 약 **25%** 수준

→ 실무에서는 **사용 불가능한 모델**

---

## 8. 핵심 정리

- 훈련 데이터로 평가한 성능은 **신뢰할 수 없다**
- 반드시 **훈련 / 검증 데이터 분리** 필요
- MAE는 예측 오차를 직관적으로 해석할 수 있는 지표
- 모델 개선은 **검증 성능 기준**으로 진행해야 한다

---

## 9. 다음 단계

모델 성능을 개선하는 방법 예시:
- 더 좋은 feature 선택
- 모델 복잡도 조절
- 다른 모델 사용 (RandomForest, Gradient Boosting 등)

