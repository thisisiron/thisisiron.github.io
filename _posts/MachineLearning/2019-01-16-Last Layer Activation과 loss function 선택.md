---
title: "Last Layer Activation과 Loss Function 선택"
categories: MachineLearning
---

## Last Layer Activation Function와 Loss Function 선택 방법
다음은 Last Layer에 선택해야할 Activation 함수와 Loss 함수 선택할 기준을 보여주는 표입니다. 


Promblem Type | Last Layer Activation | Loss Function | Example |
-------- | ---------------------- | ------------- | ------ |
Binary classification | sigmoid | binary_crossentropy | Dog vs cat, Sentiemnt analysis(pos/neg)
Multi-class, single-label classification | softmax | categorical_crossentropy |MNIST has 10 classes single label (one prediction is one digit)
Multi-class, multi-label classification | sigmoid | binary_crossentropy | News tags classification, one blog can have multiple tags
Regression to arbitrary values | None | mse | Predict house price(an integer/float point)
Regression to values between 0 and 1 | sigmoid | mse or binary_crossentropy | Engine health assessment where 0 is broken, 1 is new

## Multitask Learning(다중 학습)
추가적으로 Multitask Learning에 대해 소개하겠습니다. Andrew Ng 교수님 강의에서는 **한 신경망이 여러 작업을 동시에 할 수 있는 구조**로 정의하고 있습니다. Softmax함수를 사용하는 Multi-class single-label 구조와 비교하였습니다. Multi-class single-label 구조는 즉 하나의 Task만 가지는 것과 같습니다. 예를 들어 하나의 가방 사진을 주고 책, 가방, 옷, 거울 중에 어떤 분류에 속하는지 맞추는 것입니다. 결국 가방 하나의 Class로 분류하게 됩니다. Multitask Learning의 예를 들면 자율 주행 자동차를 위해 주행시 사물 인식 시스템을 개발한다고 가정하겠습니다. 그때 사람, 정지판, 신호등 등 여러 고려할 사항들이 있으며 그런 사물들을 모두 인식해야만 합니다. 이런 경우를 Multi Task를 가졌다고 여깁니다. 독립적으로 신경망을 구성하는 것보다 더 나은 성능을 보여주게 됩니다.

### 학습 방법
$$Y = \begin{bmatrix}
 1&  0&  0& ?\\ 
 0&  1&  1& 1\\ 
 ?&  ?&  1& 0\\ 
 ?&  ?&  0& ?
\end{bmatrix}$$ <br>

이런 식으로 모든 데이터에 레이블이 마킹이 되어 있지 않아도 사용할 수 있습니다. 물음표(?)을 제외한 계산 가능한 부분만 덧셈을 진행합니다. 다음과 같은 식을 사용하여 해결합니다.<br>

$$\frac{1}{m}\sum_{i=1}^{n}\sum_{j=1}^{4}L(\hat{y}_j^{(i)}, y_j^{(i)})$$

### 사용 시기
Multitask Learning는 다음과 같은 상황에 사용됩니다.
- Low Level Feature을 공유하는 경우<br>(Ex. 도로 주행시 인식해야할 사물들)
- 데이터가 유사한 경우
- 매우 큰 데이터들을 하나의 신경망으로 학습시키려고 할 경우

### 주의
- 효과적이기 위해서는 다른 Task들이 한 Task보다 많은 데이터를 가져야 하는데 이것은 많은 Task가 있어야하고 각 작업이 비슷한 양의 데이터를 가져야 합니다.
- 모든 작업에 대해 큰 신경망을 훈련할 수 있어야 합니다.

### Reference
[How to choose Last-layer activation and loss function](https://www.dlology.com/blog/how-to-choose-last-layer-activation-and-loss-function/)
Coursera: Structuring Machine Learning Projects (Andrew Ng)