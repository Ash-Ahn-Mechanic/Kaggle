# Course 4: Model Tuning with max_leaf_nodes (Exercises)

본 문서는 **Course 4 (Model Validation & Tuning)** 의  
`max_leaf_nodes`를 활용한 **코드 문제 풀이 전체 흐름**을 정리한 md 파일이다.

---

## 제공 함수: get_mae

각 트리 크기(`max_leaf_nodes`)에 대해  
Validation MAE를 계산하기 위한 함수가 이미 제공된다.

![get_mae 함수](images/course4_ex_get_mae.png)

```python
def get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y):
    model = DecisionTreeRegressor(
        max_leaf_nodes=max_leaf_nodes,
        random_state=0
    )
    model.fit(train_X, train_y)
    preds_val = model.predict(val_X)
    mae = mean_absolute_error(val_y, preds_val)
    return mae
```

---

## Step 1: Compare Different Tree Sizes

여러 후보 트리 크기 중 **Validation MAE가 가장 작은 값**을 찾는다.

![Step 1 - Compare Tree Sizes](images/course4_ex_step1_compare.png)

```python
candidate_max_leaf_nodes = [5, 25, 50, 100, 250, 500]

min_loss = float('inf')
min_size = None

for depth in candidate_max_leaf_nodes:
    mae = get_mae(depth, train_X, val_X, train_y, val_y)
    if mae < min_loss:
        min_loss = mae
        min_size = depth

best_tree_size = min_size
step_1.check()
```

---

## Step 1 (Solution): Dictionary Comprehension

![Step 1 - Solution](images/course4_ex_step1_solution.png)

```python
scores = {
    leaf_size: get_mae(leaf_size, train_X, val_X, train_y, val_y)
    for leaf_size in candidate_max_leaf_nodes
}

best_tree_size = min(scores, key=scores.get)
```

---

## Step 2: Fit Model Using All Data

![Step 2 - Final Model](images/course4_ex_step2_final_model.png)

```python
final_model = DecisionTreeRegressor(
    max_leaf_nodes=best_tree_size,
    random_state=0
)

final_model.fit(X, y)
step_2.check()
```

---

## 요약

- Validation MAE 기준으로 최적 트리 크기 선택
- `max_leaf_nodes`는 과소/과적합을 제어하는 핵심 파라미터
- 모델 선택 후 전체 데이터로 재학습하여 최종 모델 생성

---

## 이미지 파일 목록

- images/course4_ex_get_mae.png  
- images/course4_ex_step1_compare.png  
- images/course4_ex_step1_solution.png  
- images/course4_ex_step2_final_model.png  
