# Ames Housing Price Prediction

## 📌 Competition Overview
주택의 다양한 물리적·환경적 특성을 바탕으로 **주택 판매 가격(SalePrice)**을 예측하는 회귀 문제이다.  
데이터는 미국 아이오와주 **Ames city**의 실제 주택 거래 데이터를 기반으로 구성되어 있다.

---

## 📂 File Descriptions

| 파일명 | 설명 |
|------|------|
| `train.csv` | 학습용 데이터 (SalePrice 포함) |
| `test.csv` | 테스트 데이터 (SalePrice 미포함) |
| `data_description.txt` | 각 변수에 대한 상세 설명 |
| `sample_submission.csv` | 기준 제출 파일 (단순 선형회귀 기반) |

---

## 🎯 Target Variable

- **SalePrice**  
  - 주택의 최종 판매 가격 (USD)
  - 예측해야 할 목표 변수

---

## 🏠 Feature Description

### 1️⃣ 기본 정보 / 토지 특성
- `MSSubClass` : 건물 유형 코드  
- `MSZoning` : 토지 용도 구분  
- `LotFrontage` : 도로 접면 길이 (ft)  
- `LotArea` : 대지 면적 (sqft)  
- `Street` : 도로 접근 유형  
- `Alley` : 골목 접근 유형  
- `LotShape` : 대지 형태  
- `LandContour` : 지형 평탄도  
- `Utilities` : 사용 가능한 유틸리티  
- `LotConfig` : 대지 배치  
- `LandSlope` : 경사도  

---

### 2️⃣ 위치 / 환경 정보
- `Neighborhood` : 주거 지역  
- `Condition1` : 주요 도로·철도 인접 여부  
- `Condition2` : 추가 인접 조건 (있을 경우)  

---

### 3️⃣ 건물 유형 및 구조
- `BldgType` : 주택 유형  
- `HouseStyle` : 주택 스타일  

---

### 4️⃣ 품질 / 상태 관련 변수
- `OverallQual` : 전체 자재 및 마감 품질  
- `OverallCond` : 전반적인 상태 평가  
- `YearBuilt` : 최초 건축 연도  
- `YearRemodAdd` : 리모델링 연도  

---

### 5️⃣ 외관 / 지붕
- `RoofStyle` : 지붕 형태  
- `RoofMatl` : 지붕 재질  
- `Exterior1st` : 외벽 재질(주)  
- `Exterior2nd` : 외벽 재질(보조)  
- `MasVnrType` : 석재 마감 유형  
- `MasVnrArea` : 석재 마감 면적  

---

### 6️⃣ 외부 / 기초 / 지하실
- `ExterQual`, `ExterCond` : 외부 품질 / 상태  
- `Foundation` : 기초 유형  
- `BsmtQual`, `BsmtCond` : 지하실 품질 / 상태  
- `BsmtExposure` : 지하실 노출 정도  
- `BsmtFinType1`, `BsmtFinSF1` : 지하실 마감 1  
- `BsmtFinType2`, `BsmtFinSF2` : 지하실 마감 2  
- `BsmtUnfSF` : 미마감 지하실 면적  
- `TotalBsmtSF` : 전체 지하실 면적  

---

### 7️⃣ 난방 / 설비
- `Heating` : 난방 방식  
- `HeatingQC` : 난방 품질  
- `CentralAir` : 중앙 냉방 여부  
- `Electrical` : 전기 시스템  

---

### 8️⃣ 실내 면적 / 구조
- `1stFlrSF` : 1층 면적  
- `2ndFlrSF` : 2층 면적  
- `LowQualFinSF` : 저품질 마감 면적  
- `GrLivArea` : 지상 생활 면적  

---

### 9️⃣ 욕실 / 방 구성
- `BsmtFullBath`, `BsmtHalfBath` : 지하 욕실  
- `FullBath`, `HalfBath` : 지상 욕실  
- `Bedroom` : 침실 수  
- `Kitchen` : 주방 수  
- `KitchenQual` : 주방 품질  
- `TotRmsAbvGrd` : 총 방 개수  
- `Functional` : 주택 기능성  

---

### 🔟 벽난로 / 차고
- `Fireplaces` : 벽난로 개수  
- `FireplaceQu` : 벽난로 품질  
- `GarageType` : 차고 유형  
- `GarageYrBlt` : 차고 건축 연도  
- `GarageFinish` : 차고 마감 상태  
- `GarageCars` : 주차 가능 대수  
- `GarageArea` : 차고 면적  
- `GarageQual`, `GarageCond` : 차고 품질 / 상태  

---

### 1️⃣1️⃣ 외부 시설
- `PavedDrive` : 포장 진입로  
- `WoodDeckSF` : 데크 면적  
- `OpenPorchSF` : 개방형 현관  
- `EnclosedPorch` : 밀폐형 현관  
- `3SsnPorch` : 3계절 현관  
- `ScreenPorch` : 스크린 현관  

---

### 1️⃣2️⃣ 기타 부대시설
- `PoolArea` : 수영장 면적  
- `PoolQC` : 수영장 품질  
- `Fence` : 울타리 품질  
- `MiscFeature` : 기타 시설  
- `MiscVal` : 기타 시설 가치  

---

### 1️⃣3️⃣ 거래 정보
- `MoSold` : 판매 월  
- `YrSold` : 판매 연도  
- `SaleType` : 거래 유형  
- `SaleCondition` : 거래 조건  

---

## ✅ 문제 핵심 요약
- **문제 유형**: 회귀 (Regression)
- **목표**: `SalePrice` 예측
- **특징**:  
  - 수치형 + 범주형 혼합 데이터  
  - 결측치 다수 존재  
  - Feature Engineering 성능 영향 큼  

---
