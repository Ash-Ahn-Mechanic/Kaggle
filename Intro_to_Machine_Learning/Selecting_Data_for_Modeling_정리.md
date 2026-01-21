
# Selecting Data for Modeling (해석 및 정리)

## 1. 문제 인식
데이터셋에 변수가 너무 많으면 한 번에 이해하거나 출력하기 어렵다.  
따라서 **직관적으로 중요한 변수 몇 개만 선택**하여 모델링을 시작한다.  
(이후에는 통계적 기법으로 자동 선택 가능)

---

## 2. 데이터 불러오기 및 컬럼 확인

```python
import pandas as pd

melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path)
melbourne_data.columns
```

- `columns` 속성으로 데이터셋에 포함된 모든 변수(열) 목록을 확인한다.
- Melbourne Housing 데이터에는 주택 위치, 크기, 방 개수, 가격 등 다양한 정보가 포함되어 있다.

---

## 3. 결측치 처리

```python
melbourne_data = melbourne_data.dropna(axis=0)
```

- 일부 주택은 특정 변수 값이 기록되지 않았다.
- 현재 단계에서는 가장 단순한 방법으로 **결측치가 있는 행을 제거**한다.
- `dropna(axis=0)` : 결측치가 있는 행(row) 삭제

---

## 4. 예측 대상(Target) 선택

```python
y = melbourne_data.Price
```

- 예측하려는 값은 **집값(Price)**  
- 관례적으로 예측 대상은 `y`로 지정
- 단일 컬럼 → **Series 형태**

---

## 5. 입력 변수(Features) 선택

```python
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'Lattitude', 'Longtitude']
X = melbourne_data[melbourne_features]
```

- 모델의 입력으로 사용할 컬럼들을 **리스트로 명시**
- 이 입력 데이터는 관례적으로 `X`라고 부른다.

선택된 feature 의미:
- Rooms: 방 개수
- Bathroom: 욕실 수
- Landsize: 토지 면적
- Lattitude / Longtitude: 위치 정보

---

## 6. 데이터 확인

```python
X.describe()
X.head()
```

- `describe()` : 통계 요약 (평균, 표준편차, 최소/최대값 등)
- `head()` : 상위 5개 데이터 확인
- 데이터 분포와 이상치 여부를 시각적으로 점검하는 것은 매우 중요하다.

---

## 7. 모델링 개요 (Scikit-learn)

모델링 절차는 다음 4단계로 구성된다.

1. **Define** : 모델 정의
2. **Fit** : 데이터로 학습
3. **Predict** : 예측 수행
4. **Evaluate** : 성능 평가

---

## 8. 결정트리 회귀 모델 정의 및 학습

```python
from sklearn.tree import DecisionTreeRegressor

melbourne_model = DecisionTreeRegressor(random_state=1)
melbourne_model.fit(X, y)
```

- `DecisionTreeRegressor` : 회귀 문제용 결정트리 모델
- `random_state` 고정 → 실행할 때마다 동일한 결과 보장 (재현성)

---

## 9. 예측 수행

```python
print(melbourne_model.predict(X.head()))
```

- 학습에 사용한 데이터 일부에 대해 예측 수행
- 실제 활용 시에는 **새로운 주택 데이터**에 대해 예측하는 것이 목적

예측 결과 예시:
```
[1035000. 1465000. 1600000. 1876000. 1636000.]
```

---

## 10. 핵심 요약

- 데이터가 많을수록 **변수 선택이 중요**
- Target(y)와 Feature(X)를 명확히 구분
- 결측치 처리 → 모델 학습 전 필수 단계
- Scikit-learn 모델은 **Define → Fit → Predict → Evaluate** 구조
- `random_state`는 재현성 확보를 위해 반드시 지정하는 것이 좋다
