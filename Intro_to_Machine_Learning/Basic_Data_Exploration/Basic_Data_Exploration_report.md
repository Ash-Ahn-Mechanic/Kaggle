# Basic Data Exploration (해석 및 정리)


## 1. 서론

머신러닝 프로젝트에서 가장 먼저 수행해야 할 작업은 모델 선택이나 알고리즘 구현이 아니라 **데이터 탐색(Data Exploration)** 이다.  
데이터의 구조와 분포를 이해하지 못한 상태에서 모델을 적용하면, 결과 해석이 왜곡되거나 성능이 불안정해질 수 있다.

데이터 탐색 단계의 목적은 다음과 같다.

- 데이터의 규모와 구성(열/행, 변수 종류)을 파악한다.
- 결측값(Missing Value) 및 이상치(Outlier) 존재 여부를 확인한다.
- 변수 분포와 스케일을 확인해 전처리 방향을 정한다.
- 모델링에 사용할 입력 변수(feature) 후보를 좁힌다.

본 보고서는 Pandas를 이용해 데이터 탐색을 수행하는 기본 절차를, 예시 데이터(Melbourne Housing Dataset)를 중심으로 정리한다.

---

## 2. Pandas와 DataFrame 개요

### 2.1 Pandas

Pandas는 표(tabular) 형태 데이터를 다루는 Python 라이브러리로, 데이터 로딩·정리·요약·전처리 작업에 사용된다.  
일반적으로 다음과 같이 불러온다.

```python
import pandas as pd
```

### 2.2 DataFrame

Pandas의 핵심 자료구조는 **DataFrame**이며, 엑셀 시트나 SQL 테이블과 동일한 형태를 가진다.

- **행(row)**: 하나의 관측치(샘플)
- **열(column)**: 하나의 변수(특징/피처)

머신러닝 관점에서 DataFrame의 각 열은 입력 변수(feature) 또는 예측 대상(target)로 활용된다.

---

## 3. 분석 대상 데이터 소개

본 보고서에서는 **Melbourne Housing Dataset**을 예제로 사용한다.  
해당 데이터는 호주 멜버른 지역 주택의 가격과 다양한 특성(방 개수, 거리, 토지 면적 등)을 포함하며, 머신러닝 입문 과정에서 데이터 탐색 예제로 자주 활용된다.

### 3.1 주요 변수 예시

- `Price`: 주택 가격(예측 대상이 되는 경우가 많음)
- `Rooms`: 방 개수
- `Distance`: 도심으로부터 거리
- `Landsize`: 토지 면적
- `BuildingArea`: 건물 면적
- `YearBuilt`: 건축 연도

---

## 4. 데이터 로드

데이터는 CSV 형태로 제공되며, `read_csv()`로 DataFrame에 적재한다.

```python
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path)
```

이 단계에서 수행해야 하는 핵심 점검은 다음과 같다.

- 데이터가 정상적으로 로드되었는가?
- 열 이름이 의도한 대로 들어왔는가?
- 데이터 타입(숫자/문자/날짜)이 적절한가?

---

## 5. 데이터 요약: `describe()`의 역할

데이터의 전반적인 통계 특성을 빠르게 파악하기 위해 `describe()`를 사용한다.

```python
melbourne_data.describe()
```

`describe()`는 주로 **수치형 열**에 대해 다음 통계치를 제공한다.

- `count`, `mean`, `std`, `min`, `25%`, `50%`, `75%`, `max`

---

## 6. `describe()` 결과 해석(초심자 관점)

### 6.1 count: 결측값(누락값) 점검의 출발점

`count`는 해당 열에서 **결측값이 아닌 데이터의 개수**를 의미한다.  
열마다 count가 다르면 결측값이 존재한다는 뜻이다.

예시 해석:
- `BuildingArea`의 count가 전체 행 수보다 작다  
→ 건물 면적 정보가 수집되지 않은 주택이 존재한다.

결측값은 다음과 같은 이유로 발생할 수 있다.
- 실제로 값이 존재하지 않음(예: 1침실 주택의 “2nd bedroom” 정보)
- 조사/수집 단계에서 누락
- 입력 오류 또는 파일 병합 과정에서 누락

---

### 6.2 mean(평균)과 std(표준편차): 분포의 중심과 퍼짐

- `mean`(평균): 값들의 산술 평균
- `std`(표준편차): 값들이 평균 주변에서 얼마나 퍼져 있는지(분산의 정도)

주의점:
- 주택 가격처럼 극단적으로 큰 값(고가 주택)이 존재하면 **평균이 왜곡**될 수 있다.
- 표준편차가 크면 값의 범위가 넓고 스케일 차이가 크다는 의미이며, 모델 학습 전에 스케일링이 필요할 수 있다.

---

### 6.3 min / max: 이상치 후보 탐색

- `min`: 최솟값
- `max`: 최댓값

`max`가 지나치게 크거나 `min`이 비현실적으로 작다면 이상치 또는 입력 오류를 의심해야 한다.

예시:
- `Landsize`가 0인 경우  
→ 실제로 0일 수도 있으나, 입력 누락이 0으로 기록된 케이스일 수 있다.
- `BuildingArea`가 비정상적으로 큰 경우  
→ 단위(m² vs ft²) 문제 또는 입력 오류 가능성 존재.

---

### 6.4 25% / 50% / 75%: 사분위수(Percentile)의 의미

사분위수는 데이터를 정렬했을 때 특정 위치의 값을 의미한다.

- `25%`: 하위 25% 지점 값(1사분위)
- `50%`: 중앙값(Median)
- `75%`: 하위 75% 지점 값(3사분위)

실무적으로 중요한 이유:
- 평균은 이상치에 약하지만, 중앙값은 상대적으로 안정적이다.
- 중앙값과 평균이 크게 다르면 분포가 한쪽으로 치우친(왜도, skewness) 가능성이 있다.

---

## 7. `describe()` 이후 권장 점검(흐름 유지용 확장)

`describe()`는 “요약 통계”이므로, 다음 점검을 함께 수행하면 데이터 이해가 훨씬 명확해진다.

### 7.1 데이터 타입/결측 현황 확인

```python
melbourne_data.info()
melbourne_data.isnull().sum()
```

- `info()`로 열별 타입과 결측 여부를 개괄적으로 확인
- `isnull().sum()`으로 열별 결측값 개수 확인

### 7.2 분포 시각화(이상치/왜도 확인)

```python
melbourne_data['Price'].hist()
```

- 히스토그램은 값 분포가 치우쳤는지, 이상치가 있는지 확인하는 가장 기본 도구이다.

### 7.3 타깃(Price)과 입력 변수 관계의 기초 확인

```python
melbourne_data[['Rooms', 'Distance', 'Landsize', 'Price']].corr()
```

- 상관계수는 변수 간 선형 관계를 빠르게 점검할 수 있으나,
- 상관이 낮다고 무의미한 변수가 되는 것은 아니며, 비선형 관계는 다른 방법이 필요하다.

---

## 8. 같이 보면 좋은 참고 이미지/자료 링크

아래 링크들은 본 보고서 흐름(통계 요약 → 분포/결측/이상치 이해)에 직접 도움이 되는 자료들이다.

- Pandas DataFrame 튜토리얼(공식): https://pandas.pydata.org/docs/getting_started/intro_tutorials/01_table_oriented.html  
- `describe()` 공식 문서: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html  
- 사분위수/박스플롯 개념: https://en.wikipedia.org/wiki/Box_plot  
- 평균 vs 중앙값(왜도 분포 이해): https://en.wikipedia.org/wiki/Skewness  
- 결측값 처리 개요(공식 가이드): https://scikit-learn.org/stable/modules/impute.html  

---

## 9. 결론

Pandas의 `describe()`는 데이터의 **기본 통계 요약을 통해 데이터 품질과 분포 특성을 빠르게 진단**하는 도구이다.  
`count`를 통해 결측값 존재를 확인하고, `mean/std`로 스케일과 분산을 파악하며, 사분위수와 min/max를 통해 분포 형태와 이상치 가능성을 점검할 수 있다.

머신러닝에서 좋은 모델은 좋은 알고리즘보다 먼저 **좋은 데이터 이해**에서 시작된다.

---
