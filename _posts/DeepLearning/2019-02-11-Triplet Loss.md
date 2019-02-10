---
title: "Triplet Loss"
categories: DeepLearning
---

## One Shot Learning
one shot learning이란 하나의 예시를 통해서 Task에 맞는 작업을 수행하는 것입니다. 예를 들어 사람을 인식하는 Task라면 하나의 사진을 갖고 사람을 판별하는 것입니다. 구체적인 상황을 생각해보면 직원을 인식하여 출입문을 개방하는 시스템을 만든다고 가정할 때, 직원 사진을 **CNN을 통해 학습 후 Softmax로 분류**하는 방법은 사용할 수 없습니다. 다음과 같은 문제가 존재합니다.

1. 새로운 직원이 올 떄마다 학습
2. 직원 데이터베이스에 직원당 직원 사진이 1개만 존재

이런 문제를 해결하기 위해 Similarity function을 사용합니다. 다음과 같이 정의합니다.

$$
d$(img1, img2)$ \enspace $= degree of difference between images$ \\
d$(img1, img2)$ \enspace \leq \tau \enspace $"Same"$ \\
d$(img1, img2)$ \enspace > \tau \enspace $"Different"$
$$

## Siamese Network
<img src="/assets/images/siamese_network.jpg"><br>

위의 이미지처럼 Siamese Network에서 두 $x^1$과 $x^2$ 사진을 ConvNet에 넣고 feature embedding 뽑아내게 됩니다.
$f(\cdot)$은 Network을 의미하며 output은 feature embedding이 됩니다.<br>

Similarity Function은 거리를 이용하여 다음과 같이 표현할 수 있습니다.

$$
d(x^1, x^2) = \left \| f(x^1)) - f(x^2) \right \|_2^2
$$

유사한 사진인 경우 위의 함수 값이 작을 것이고, 유사하지 않은 사진인 경우에는 값이 커지게 됩니다.

## Triplet Loss
Anchor Image(A), Positive Image(P), Negative Image(N)을 구성하여 다음과 같은 수식을 작성할 수 있습니다.

$$
\left \| f(A)) - f(P) \right \|^2 \leq \left \| f(A)) - f(N) \right \|^2
$$

이 수식에서 우변을 오른쪽으로 넘기게 되면,

$$
\left \| f(A)) - f(P) \right \|^2 - \left \| f(A)) - f(N) \right \|^2 \leq 0
$$

위와 같은 수식이 작성됩니다. 이 수식을 사용할 때 문제가 발생하게 됩니다.

1. 단순히 $f$(img)$=\vec{0}$라면 위의 수식에서 0을 만족하게 됩니다.
2. 모든 이미지의 인코딩이 다른 이미지의 인코딩 값이 같은 경우 0을 만족하게 됩니다.

의미있는 인코딩이 아니라 단순히 0을 뽑아서 Loss을 줄이는 방법을 선택할 수가 있습니다. 이에 대한 해결방법으로 margin을 다음과 같이 추가합니다.

$$
\left \| f(A)) - f(P) \right \|^2 - \left \| f(A)) - f(N) \right \|^2 +margin \leq 0
$$

margin을 추가함으로써 $A-P$와 $A-N$이 서로 멀어지도록 하는 효과를 얻게 됩니다. 즉, 최소한 이 정도 차이가 나야 Negative로 인정하겠다는 의미입니다.<br>

이제 Triplet Loss에 대해 알아보겠습니다.

$$
L(A,P,N)= max(\left \| f(A)) - f(P) \right \|^2 - \left \| f(A)) - f(N) \right \|^2 +margin,0) 
$$

$\left \| f(A)) - f(P) \right \|^2 - \left \| f(A)) - f(N) \right \|^2 +margin$ 부분을 앞으로 $D$라고 부르겠습니다. D가 0보다 작거나 같았다면 loss는 0을 만들게 됩니다. Loss을 최소화하기 위해서 $D$을 0보다 작거나 같게 만들어야 합니다. 신경망은 $D$가 얼마나 negative한지는 중요하지 않습니다. <br>
위의 $L$함수는 single일 때 함수이며 데이터 전체에 대한 Loss 함수는 다음과 같습니다.

$$
J = \sum_{i=1}^{m}L(A^{(i)},P^{(i)},N^{(i)})
$$

한 가지 더 생각해볼 것이 있는데 A,P,N이 무작위로 선택되었다면 확률적으로 $(A,P)$보다 $(A,N)$이 주로 값이 크기 때문에 학습이 제대로 하지 못하는 경우가 발생할 수 있습니다. 즉, $d(A,P) \geq d(A,P)+margin$인 경우를 의미합니다.

따라서 학습하기 어려운 Triplet A,P,N을 구성해야합니다. $d(A,P)+margin \leq d(A,N)$ 수식을 만족하면 됩니다. 이 수식이 만족하기 어렵다면 $d(A,P)\approx  d(A,N)$을 만족시키면 됩니다. 이러한 제약식을 만족하는 데이터 셋을 만들게 되면 경사하강법이 효율적으로 작동합니다.


## Siamese Network and Binary Classification
<img src="/assets/images/siamese_network_binary.jpg"><br>

### Reference
Coursera:  Convolutional Neural Networks (Andrew Ng)<br>