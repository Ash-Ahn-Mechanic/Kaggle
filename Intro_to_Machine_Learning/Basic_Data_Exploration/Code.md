# Course 2: Basic Data Exploration (Code)

## Step 1: Loading Data

Iowa housing 데이터를 Pandas DataFrame(`home_data`)로 불러온다.

```python
import pandas as pd

# Path of the file to read
iowa_file_path = '../input/home-data-for-ml-course/train.csv'

# Read the file into a variable home_data
home_data = pd.read_csv(iowa_file_path)

```

---

## Step 2: Review The Data

`describe()`를 사용해 데이터의 요약 통계(개수, 평균, 표준편차, 사분위수 등)를 확인한다.

```python
# Print summary statistics
home_data.describe()
```

### Questions

#### 1) What is the average lot size (rounded to nearest integer)?
- `LotArea` 컬럼의 평균값을 반올림하여 계산한다.

```python
avg_lot_size = 10517
```

#### 2) As of today, how old is the newest home?
- 가장 최근에 지어진 집의 연식 = 현재 연도 − `YearBuilt`의 최댓값

```python
newest_home_age = 16
```

