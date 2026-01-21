# Course 3: Selecting Data for Modeling (Code)

## Step 1: Specify Prediction Target

예측 대상(target variable)은 주택 판매 가격 컬럼(`SalePrice`)이다.

```python
# Print the list of columns in the dataset to find the name of the prediction target
print(home_data.columns)

# Target
y = home_data['SalePrice']

# Check your answer
step_1.check()
```

---

## Step 2: Create X

모델 입력 데이터 `X`는 일부 예측 feature 컬럼만 선택해 만든다.

사용 feature:
- `LotArea`
- `YearBuilt`
- `1stFlrSF`
- `2ndFlrSF`
- `FullBath`
- `BedroomAbvGr`
- `TotRmsAbvGrd`

```python
# Create the list of features below
feature_names = [
    'LotArea',
    'YearBuilt',
    '1stFlrSF',
    '2ndFlrSF',
    'FullBath',
    'BedroomAbvGr',
    'TotRmsAbvGrd'
]

# Select data corresponding to features in feature_names
X = home_data[feature_names]

# Check your answer
step_2.check()
```

---

## Step 3: Specify and Fit Model

아래는 DecisionTreeRegressor 모델을 정의하고, 앞에서 만든 `X`, `y` 데이터를 이용해 학습(fit)하는 과정이다.
```python
from sklearn.tree import DecisionTreeRegressor

# Specify the model
# For model reproducibility, set a numeric value for random_state
iowa_model = DecisionTreeRegressor(random_state=1)

# Fit the model
iowa_model.fit(X, y)

# Check your answer
step_3.check()
```

---

## Step 4: Make Predictions

학습된 모델을 사용해 입력 데이터 `X`에 대한 예측값을 생성한다.
```python
predictions = iowa_model.predict(X)
print(predictions)

# Check your answer
step_4.check()
```
