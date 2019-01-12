---
title: "Batch Nomalization란"
categories: MachineLearning
---

## Description
입력층에만 정규화하는 것이 아니라 Hidden Layer의 값도 정규화 하는 방식입니다. 여러 논쟁이 있지만 $$a^{[L]}$$이 아니라 보통 $$z^{[L]}$$을 Nomlaize합니다. 다음과 같이 작동합니다.

1. $\mu = \frac{1}{m}\sum_{i}z^{i}$ (평균)
2. $\sigma ^2 = \frac{1}{m}\sum_{i}(z^{(i)} - \mu )^2$ (분산)
3. $z^{(i)}_{norm} = \frac{z^{(i)}-\mu}{\sqrt{\sigma ^2 + \varepsilon }}$
4. $\tilde{z}^{(i)} = \gamma z^{(i)}_{norm} + \beta$ 

활성화 함수에 전달할 때 $$ z^{(i)} $$ 대신에 $$ \tilde{z}^{(i)} $$을 사용하세요. $z^{(i)}_{norm}$은 모든 $z$들의 평균이 0, 표준편차가 1이 되도록 만듭니다. 4번식에서 다양한 분포를 가질 수 있도록 $\gamma$와 $\beta$를 사용합니다. 이 점이 입력층에서 정규화하는 것과 차이점이라고 볼 수 있습니다. 즉, Hidden Unit의 평균과 분산이 0(평균)과 1(분산)로 고정되는 것을 원치않습니다. $\gamma$와 $\beta$는 Learnable Parameter라고 불리우며, 이 값을 이용해서 평균과 표준편차 값을 다르게 가질 수 있습니다. $\gamma = \sqrt{\sigma^2 + \varepsilon}$와 $\beta = \mu$ 식이 성립한다면 3번의 식을 거꾸로 뒤집은 것과 같은 효과가 나옵니다. $\gamma = \sqrt{\sigma^2 + \varepsilon}$와 $\beta = \mu$이 성립하면 $\tilde{z}^{(i)}$가 $z$같게 됩니다.

그러면 왜 평균과 분산이 0과 1로 고정되면 좋지 않을까? 라고 의문을 가질 수 있습니다. 그 이유는 Sigmoid 함수를 예시로 보겠습니다.
<img src="/assets/images/sigmoid_mean.jpg">
만약에 평균과 분산이 0과 1의 값을 갖게 된다면 빨간색 선과 같이 값이 모이게 됩니다. 한 구간에 모이는 것보다 널리 퍼져 있는 것이 좋습니다.

여기에서 사용하는 $\beta$는 Momtum에서 사용하는 $\beta$와 다릅니다. 물론 RMSProp에서의 $\beta$도 마찬가지로 다릅니다. 또한 $\gamma$와 $\beta$는 Adam, RMSProp, Gradient 등을 이용해 업데이트를 다음과 같이 진행할 수 있습니다. 

$\beta^{[l]} = \beta^{[l]} - \alpha d\beta^{[l]}$

$\gamma^{[l]} = \gamma^{[l]} - \alpha d\gamma^{[l]}$

Batch Nomalization이 $z^{[l]}$의 평균을 0으로 만들기 때문에 $b^{[l]}$변수가 필요없어집니다. $\beta^{[l]}$가 그 역할을 대신하기 때문입니다. 결과적으로 $\beta^{[l]}$는 편향 변수를 결정하게 됩니다.

## Mini-Batch 사용 Case
Mini-Batch를 사용하는 경우에는 각 해당되는 Mini-Batch의 평균과 분산을 사용합니다. 다음과 같이 진행됩니다. 첫 번째 화살표는 W을 의미하고 두 번째 화살표는 Batch Nomalization을 의미합니다. {} 기호는 n번째 배치를 의미합니다.

1. $X^{\{1\}}\rightarrow z^{[1]} \rightarrow \tilde{z}^{[1]}$
2. $X^{\{2\}}\rightarrow z^{[2]} \rightarrow \tilde{z}^{[2]}$
3. $X^{\{n\}}\rightarrow z^{[n]} \rightarrow \tilde{z}^{[n]}$


## 장점
1. 이전 Layer의 가중치 영향을 덜 받게 합니다.
   - 이전 Layer의 값이 바뀌더라도 평균과 분산은 유지됩니다. 그렇기 때문에 값의 분포가 제한되는 것입니다.
   - 이전 층 파라미터와 뒤쪽 층의 파라미터 간의 관계를 약화시킵니다. 따라서, 다른 층에 영향을 받지 않고 각 층이 스스로 배울 수 있게 됩니다.
   - 입력값이 바뀌어서 발생하는 문제를 안정화 시킵니다. 입력 값의 분포가 바뀌더라도 조금 바뀌게 되기 때문입니다. 예를 들어 검은 고양이로만 학습하다 여러 종류의 고양이로 학습할 경우 데이터 분포가 변화하게 됩니다. Coveriate Shift(공변량변화)라고 부르는 현상이 발생됩니다. 이 경우에 학습 알고리즘을 다시 학습해야하는 문제가 발생합니다. 이런 문제를 해결하게 됩니다.

2. 파라미터의 Regularization. 미니 배치로 계산한 평균과 분산은 전체 데이터 중 일부이기 때문에 Noise(잡음)가 존재합니다. 전체 데이터에서 계산한 것이 비해 상대적으로 작은 데이터에서 추정했기 때문입니다.
   - Dropout의 경우 0 or 1을 곱하는 곱셈 잡음이 존재
   - Batch Nomalization은 $\sigma ^2$(나누기), $\mu$(빼기)(곱셈잡음, 덧셈잡음)이 존재
   - Hidden Layer에 잡음을 추가하는 것은 이후의 Hidden Layer가 하나의 Hidden Unit에 의존하지 않도록 함
   - 잡음이 작다보니 일반화(Regularization) 효과는 크지 않으므로 더욱 강력한 일반화를 원한다면 Batch Nomalization와 Dropout을 함께 사용

※ 주의: 일반화를 목적으로 사용하기 보다 학습 속도를 올리는데 사용하고 큰 미니배치를 사용시 잡음이 줄어들게 되니 일반화 효과도 줄어들게 됩니다.

## Test에서 사용될 경우
한 번에 하나의 Mini-Batch을 처리하지만 Test에서는 Batch가 존재하지 않고 한 번에 샘플 하나씩을 처리하기 때문에 평균과 분산을 계산할 수 없습니다. 따라서 학습 시에 사용된 Mini Batch의 지수 가중 이동 평균(Exponentially Weighted Average)을 추정치로 사용합니다.


### Reference
Coursera: Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization (Andrew Ng)