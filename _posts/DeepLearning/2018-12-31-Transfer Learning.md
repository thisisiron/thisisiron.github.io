---
title: "Transfer Learning(전이학습)과 Fine-tuning"
categories: DeepLearning
---
## Transfer Learning(전이학습) 이란
- 습득한 지식을 저장하고 다른 관련 문제에 적용하는 방법
- 기존의 학습 모델로부터 마지막 층을 제거한 후 분류하고자 하는 적합한 층을 연결

## Fine-tuning 이란
- pre-trained 모델의 가중치를 초기 가중치로 재학습하는 방법
- 모든 네트워크에서 가능하고, 일부만 진행할 수도 있음

## 언제 사용할까?
충분한 데이터가 있다면 전체를 학습시킬 수 있지만, 데이터가 충분하지 않다면 기존의 모델에서 추가한 층에서만 학습을 진행 후 사용이 가능합니다.

1. 전이 가능한 데이터는 많고 전이하려는 데이터가 적을 경우 자주 사용
2. A와 B가 같은 Input을 가지는 경우 자주 사용
3. A의 Low Level 특성이 B에게 유용할 때 자주 사용 -> A는 과일 분류, B는 사과 분류

## 동결층(Freeze)과 재학습(Fine-tuning)
<img src="/assets/images/transfer_learning.jpg">
<br><br>

**동결층**
- 이미 많은 데이터로 학습되어 있는 Layer
- *재학습하지 않고* 그대로 사용할 Layer

**Fine-tuning**
- 기존의 학습되어 있는 층을 초기 값으로 *재학습*하거나 새로 추가하여 학습할 Layer
- 기존의 학습되어 있는 가중치를 이용하는 것을 **Fine-tuning**이라고함

예시
1. 기존의 모델을 그대로 사용하는 경우, 기존 모델을 Freeze(동결)하고 Softmax 가중치만 학습하게 됩니다.
2. 기존 모델 중 일부만 Freeze(동결)하고 이외의 것은 재학습시키거나 새로운 Hidden Layer을 추가할 수 있습니다.

## 상황에 따른 사용법
아래 그림과 같이 4가지 상황이 있습니다.<br>
**Quadrant 1.** <br>
많은 데이터를 갖고 있지만 pre-trained model의 데이터와는 다른 경우<br>
**Quadrant 2.** <br>
많은 데이터를 갖고 있고 pre-trained model의 데이터와 유사한 경우<br>
**Quadrant 3.** <br>
적은 데이터를 갖고 있지만 pre-trained model의 데이터와는 다른 경우<br>
**Quadrant 4.** <br>
적은 데이터를 갖고 있지만 pre-trained model의 데이터와 유사한 경우<br>

<img src="/assets/images/Size_Similarity_matrix.png">

위의 4가지 상황에 따른 각각의 해결 방법이 존재합니다.<br>
**Quadrant 1.** <br>
전체 모델을 학습시킬 수 있고 개발자가 원하는 것은 무엇이든 가능합니다. 데이터가 비슷하지 않더라도 실제로는 **pre-trained model의 초기 설정을 이용하는 것**(Fine-tuning)이 유용할 수 있습니다. 모델 구조를 사용할 수 있고 학습된 가중치를 사용할 수 있습니다.<br>

**Quadrant 2.** <br>
pre-trained model을 적극 활용할 수 있습니다. 다음과 같은 2가지 방법이 존재합니다.
1. Output Layer 층 쪽의 Layers을 재학습 가능
2. Full Network에서 fine-tune 시도 가능

**Quadrant 3.** <br>
가장 최악의 시나리오입니다. freeze 층과 학습시킬 층의 balance을 찾는 것이 어렵습니다. 얕은 층을 이용한다면 학습이 제대로 안될 수도 있고 깊은 층을 이용하면 Overfit문제가 발생할 수 있습니다. data augmentation 기술을 고려해봐야합니다.<br>

**Quadrant 4.** <br>
이전 마지막 Output Layer을 제거하고 마지막에 원하는 목적인 Output Layer을 추가하여 새로운 Classifier을 만듭니다. Fine-tuning방법은 좋은 방법이 아닐 수 있습니다. ConvNet이 과적합이 발생할 수 있기 때문입니다.<br>
<img src="/assets/images/decision_map_transfer_learning.png">

### Reference
[Size-Similarity matrix and decision map](https://towardsdatascience.com/transfer-learning-from-pre-trained-models-f2393f124751)<br>
Coursera: Deep Learning course (Andrew ng)
[Transfer Learning](http://cs231n.github.io/transfer-learning/)