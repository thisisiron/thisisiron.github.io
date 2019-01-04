---
title: "Transfer Learning"
categories: MachineLearning
---
## 정의
- 습득한 지식을 저장하고 다른 관련 문제에 적용하는 방법
- 기존의 학습 모델로부터 마지막 층을 제거한 후 분류하고자 하는 적합한 층을 연결

## 언제 사용할까?
충분한 데이터가 있다면 전체를 학습시킬 수 있지만, 데이터가 충분하지 않다면 기존의 모델에서 추가한 층에서만 학습을 진행 후 사용이 가능하다.

1. 전이 가능한 데이터는 많고 전이하려는 데이터가 적을 경우 자주 사용
2. A와 B가 같은 Input을 가지는 경우 자주 사용
3. A의 Low Level 특성이 B에게 유용할 때 자주 사용 -> A는 과일 분류, B는 사과 분류

## 동결층과 학습층
<img src="/assets/images/transfer_learning.jpg">
<br><br>

1. 기존의 모델을 그대로 사용하는 경우, 기존 모델을 Freeze(동결)하고 Softmax 가중치만 학습하게 된다.
2. 기존 모델 중 일부만 Freeze(동결)하고 이외의 것은 재학습시키거나 새로운 Hidden Layer을 추가할 수 있다.

## 그러면 어떻게 사용할까?
아래 그림과 같이 4가지 상황이 있다.<br>
**Quadrant 1.** <br>
많은 데이터를 갖고 있지만 pre-trained model의 데이터와는 다른 경우<br>
**Quadrant 2.** <br>
많은 데이터를 갖고 있고 pre-trained model의 데이터와 유사한 경우<br>
**Quadrant 3.** <br>
적은 데이터를 갖고 있지만 pre-trained model의 데이터와는 다른 경우<br>
**Quadrant 4.** <br>
 적은 데이터를 갖고 있지만 pre-trained model의 데이터와 유사한 경우<br>

<img src="/assets/images/size_similarity_matrix.PNG">

위의 4가지 상황에 따른 각각의 해결 방법이 존재한다.<br>
**Quadrant 1.** <br>
전체 모델을 학습시킬 수 있고 너가 원하는 것은 무엇이든 가능하다. 데이터가 비슷하지 않더라도 실제로는 pre-trained model의 초기 설정을 이용하는 것이 유용할 수 있다. 모델 구조를 사용할 수 있고 학습된 가중치를 사용할 수 있다.<br>
**Quadrant 2.** <br>
pre-trained model을 적극 활용할 수 있다. 따라서 Output Layer 층 쪽의 Layers을 재학습을 할 수 있다.<br>
**Quadrant 3.** <br>
가장 최악의 시나리오이다. freeze 층과 학습시킬 층의 balance을 찾는 것이 어렵다. 얕은 층을 이용한다면 학습이 제대로 안될 수도 있고 깊은 층을 이용하면 Overfit문제가 발생할 수 있다. data augmentation 기술을 고려해봐야한다.<br>
**Quadrant 4.** <br>
이전 마지막 Output Layer을 제거하고 마지막에 원하는 목적인 Output Layer을 추가하여 새로운 Classifier을 만든다.<br>
<img src="/assets/images/decision_map_transfer_learning.png">

#### Referebce
[Size-Similarity matrix and decision map](https://towardsdatascience.com/transfer-learning-from-pre-trained-models-f2393f124751)

Coursera Deep Learning course - Andrew ng Professor