---
title: "Overfitting과 Underfitting"
categories: MachineLearning
---

## Overfitting(오버피팅)

### Description
Train Data set에서는 우수한 성능을 보이지만 Test Data set에서는 그런 결과를 내지 못하는 경우를 의미합니다.
시각적으로 예를 들면 다음과 같은 경우가 존재합니다.

### Solutions
1. 특성 수를 줄인다. -> 쓸만한 특성만 남겨두기, 하지만 feature을 버리면 문제에 포함된 정보도 같이 버리게 됩니다.
2. Regularization -> 모든 특성을 남기되, 각각의 특성이 갖는 영향 규모를 줄이는 것
3. Dropout -> 사람의 망각과 같은 특성, 뉴런의 일부만을 사용하여 학습을 합니다.
4. Batch Nomalization -> generalization이 가능하지만 되도록이면 그것을 목적으로 사용하지 않는 것이 좋습니다.


## Underfitting(언더피팅)

### Description
High bias라고도 부르며, 모델이 데이터에 맞지 않는 것이다. 즉, 형편없는 결과(loss가 크다)가 도출됩니다.
이번에도 시각적으로 예를 들면 다음과 같은 경우가 존재합니다.

### Solutions
1. 고차원 모델을 사용하기

#### Reference