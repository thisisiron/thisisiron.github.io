---
title: "Overfitting과 Underfitting"
categories: MachineLearning
---

## Overfitting(오버피팅)

### Description
**High Variance**라고도 부르며, Train Data set에서는 우수한 성능을 보이지만 Test Data set에서는 그런 결과를 내지 못하는 경우를 의미합니다. 시각적으로 예를 들면 다음과 같은 경우가 존재합니다.


Appropriate | Overfitting | 
----- | ----- | 
<img src="/assets/images/appropriate_capacity.PNG"> | <img src="/assets/images/overfitting.png"> | 

### Solutions
1. 특성 수를 줄인다 -> 쓸만한 특성만 남겨두기, 하지만 feature을 버리면 문제에 포함된 정보도 같이 버리게 됩니다.
2. Regularization -> 모든 특성을 남기되, 각각의 특성이 갖는 영향 규모를 줄이는 방법입니다.
3. Dropout -> 사람의 망각과 같은 특성, 뉴런의 일부만을 사용하여 학습을 합니다.
4. [Batch Nomalization](https://thisisiron.github.io/machinelearning/Batch-Nomalization/) -> Regularization이 가능하지만 되도록이면 그것을 목적으로 사용하지 않는 것이 좋습니다.
5. 데이터를 더 확보합니다 -> 데이터를 더 확보하거나 Data Augmentation방법 같이 기존의 데이터를 변형하여 데이터 수를 늘립니다.


## Underfitting(언더피팅)

### Description
**High bias**라고도 부르며, 모델이 데이터에 맞지 않는 것이다. 즉, 형편없는 결과(loss가 크다)가 도출됩니다.
이번에도 시각적으로 예를 들면 다음과 같은 경우가 존재합니다.

Appropriate | Underfitting | 
----- | ----- | 
<img src="/assets/images/appropriate_capacity.PNG"> | <img src="/assets/images/underfitting.png"> | 

### Solutions
1. 고차원 모델을 사용하기 -> 현재 모델이 데이터를 잘 표현 못하는 것이므로 

## 모델의 선택
### Low Capacity
일반화 능력은 좋으나, 학습 데이터를 충분히 표현하기 힘들 수 있습니다. 간단한 모델을 선택하면 일반화는 좋으나 Underfitting 문제가 발생합니다.

### High Capacity
표현 능력은 좋으나, Train Data를 Memorize할 가능성이 높습니다. 이 경우에는 Overfitting 문제가 발생합니다.

### Reference