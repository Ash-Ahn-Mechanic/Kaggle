# Course 5: Random Forests (Introduction & Example)

## 1. Introduction: 왜 Random Forest인가?

Decision Tree는 구조가 단순하고 해석이 쉽지만, **과소적합(Underfitting)** 과 **과적합(Overfitting)** 사이에서 항상 트레이드오프 문제가 발생한다.

- 🌳 **깊은 트리 (leaf가 많음)**  
  → 각 leaf에 포함된 데이터가 매우 적음  
  → 훈련 데이터에는 잘 맞지만, 새로운 데이터에는 성능 저하  
  → **과적합**

- 🌱 **얕은 트리 (leaf가 적음)**  
  → 데이터 구분이 충분하지 않음  
  → 훈련 데이터에서도 오차 큼  
  → **과소적합**

이러한 문제는 Decision Tree뿐 아니라, **대부분의 머신러닝 모델이 공통적으로 겪는 문제**다.

---

## 2. Random Forest의 핵심 아이디어

**Random Forest**는 이러한 문제를 해결하기 위한 대표적인 앙상블(Ensemble) 기법이다.

### 기본 개념
- 여러 개의 Decision Tree를 동시에 학습
- 각 트리는 서로 **조금씩 다른 데이터/feature**를 사용
- 최종 예측은 **각 트리의 예측값을 평균**하여 계산

📌 핵심 효과  
> 개별 트리의 과적합 경향을 서로 상쇄하여,  
> **일반화 성능이 뛰어난 모델**을 만든다.

---

## 3. 데이터 준비 (복습)

아래 과정은 이전 코스에서 반복적으로 사용한 데이터 로딩 및 전처리 과정이다.

```python
import pandas as pd

# Load data
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path)

# Filter rows with missing values
melbourne_data = melbourne_data.dropna(axis=0)

# Choose target and features
y = melbourne_data.Price
melbourne_features = [
    'Rooms', 'Bathroom', 'Landsize', 'BuildingArea',
    'YearBuilt', 'Lattitude', 'Longtitude'
]
X = melbourne_data[melbourne_features]
```

---

## 4. Train / Validation 데이터 분리

모델 성능을 공정하게 평가하기 위해,  
데이터를 학습용과 검증용으로 분리한다.

```python
from sklearn.model_selection import train_test_split

train_X, val_X, train_y, val_y = train_test_split(
    X, y, random_state=0
)
```

- `random_state=0`  
  → 항상 동일한 데이터 분할 보장 (재현성)

---

## 5. Random Forest 모델 학습

Decision Tree 대신  
`RandomForestRegressor` 클래스를 사용한다.

```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error

forest_model = RandomForestRegressor(random_state=1)
forest_model.fit(train_X, train_y)

melb_preds = forest_model.predict(val_X)
print(mean_absolute_error(val_y, melb_preds))
```

### 출력 결과
```
191669.7536453626
```

---

## 6. 결과 해석

- 📉 **MAE ≈ 191,670**
- 이전에 최적화한 Decision Tree의 MAE ≈ **250,000**
- **Random Forest가 약 6만 이상 오차 감소**

📌 중요한 점
- 별도의 하이퍼파라미터 튜닝 없이도
- 단일 Decision Tree보다 **훨씬 안정적이고 정확**

---

## 7. Random Forest의 장점 요약

- ✅ 과적합에 강함
- ✅ 기본 설정만으로도 성능이 좋음
- ✅ Decision Tree의 단점을 효과적으로 보완
- ❌ 모델 구조 해석은 상대적으로 어려움

---

## 8. 결론

- 과소적합 / 과적합 문제는 모든 모델이 직면
- Random Forest는 **여러 트리의 평균화**로 이를 완화
- 실무 및 Kaggle 입문 단계에서 **가장 강력한 기본 모델 중 하나**
- 추가적인 성능 향상은 하이퍼파라미터 튜닝으로 가능

---

## 다음 단계 (Your Turn)

- `n_estimators`, `max_depth`, `max_leaf_nodes` 조정
- Gradient Boosting, XGBoost 등 다른 앙상블 모델 학습
- Feature Engineering을 통한 성능 개선
