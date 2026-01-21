# Course 4: Model Validation (Code)

---

## Step 1: Split Your Data

`train_test_split`을 사용해 `X`, `y`를 학습/검증 데이터로 분리한다.  
`random_state=1`을 지정해 결과를 고정한다.

```python
# Import the train_test_split function and uncomment
from sklearn.model_selection import train_test_split

# fill in and uncomment
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state=1)

# Check your answer
step_1.check()
```

---

## Step 2: Specify and Fit the Model

`DecisionTreeRegressor(random_state=1)`로 모델을 만들고, `train_X`, `train_y`로 학습한다.

```python
# Specify the model
iowa_model = DecisionTreeRegressor(random_state=1)

# Fit iowa_model with the training data.
iowa_model.fit(train_X, train_y)

# Check your answer
step_2.check()
```

---

## Step 3: Make Predictions with Validation Data

검증 데이터 `val_X`에 대해 예측값을 만든다.

```python
# Predict with all validation observations
val_predictions = iowa_model.predict(val_X)

# Check your answer
step_3.check()
```

---

## Step 4: Calculate the Mean Absolute Error in Validation Data

`mean_absolute_error`로 검증 MAE를 계산한다.

```python
from sklearn.metrics import mean_absolute_error

val_mae = mean_absolute_error(val_y, val_predictions)

# uncomment following line to see the validation_mae
print(val_mae)

# Check your answer
step_4.check()
```
