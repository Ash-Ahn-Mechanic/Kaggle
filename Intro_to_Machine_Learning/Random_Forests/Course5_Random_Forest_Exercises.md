# Course 5: Random Forest (Exercises)

## Step 1: Use a Random Forest

이 문제의 목표는 `RandomForestRegressor` 모델을 학습시키고,  
검증(Validation) 데이터에 대한 **MAE(Mean Absolute Error)** 를 계산하는 것이다.

---

## 코드

![Step 1 - Use a Random Forest](images/course5_step1_use_random_forest.png)

```python
from sklearn.ensemble import RandomForestRegressor

# Define the model. Set random_state to 1
rf_model = RandomForestRegressor(random_state=1)

# fit your model
rf_model.fit(train_X, train_y)

# Calculate the mean absolute error of your Random Forest model on the validation data
pred_y = rf_model.predict(val_X)
rf_val_mae = mean_absolute_error(pred_y, val_y)

print("Validation MAE for Random Forest Model: {}".format(rf_val_mae))

# Check your answer
step_1.check()
```

---

## 해석 정리

### 1) 모델 생성
```python
rf_model = RandomForestRegressor(random_state=1)
```
- `RandomForestRegressor`는 여러 개의 Decision Tree를 학습한 뒤  
  각 트리의 예측값을 평균 내어 최종 예측을 만든다.
- `random_state=1`은 결과 재현을 위해 고정한다.

### 2) 학습 (fit)
```python
rf_model.fit(train_X, train_y)
```
- 학습용 데이터(`train_X`, `train_y`)로 모델을 학습한다.

### 3) 예측 (predict)
```python
pred_y = rf_model.predict(val_X)
```
- 검증용 입력 데이터 `val_X`에 대해 예측값을 생성한다.

### 4) MAE 계산
```python
rf_val_mae = mean_absolute_error(pred_y, val_y)
```
- 실제값 `val_y`와 예측값 `pred_y`의 평균 절대 오차를 계산한다.
- MAE는 **낮을수록 성능이 좋다.**

> 참고: 일반적으로 함수 형태는 `mean_absolute_error(y_true, y_pred)`이다.  
> 즉, `mean_absolute_error(val_y, pred_y)` 형태가 더 표준적이다.

### 5) 출력
```python
print("Validation MAE for Random Forest Model: {}".format(rf_val_mae))
```
- 검증 MAE를 출력한다. (예: `21857.159...`)

---

## 핵심 결론

- Random Forest는 단일 Decision Tree보다 **일반화 성능이 좋은 경우가 많다.**
- 별도의 깊이 튜닝 없이도 성능이 잘 나오는 것이 장점이다.
- Validation MAE로 모델 성능을 비교하며 발전시키는 흐름이 중요하다.

---

## 이미지 파일 경로 규칙

md에서 이미지를 보이게 하려면 아래 파일을 `images/` 폴더에 저장해야 한다.

- `images/course5_step1_use_random_forest.png`
