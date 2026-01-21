# Course 3: Model Building (Exercises)

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
