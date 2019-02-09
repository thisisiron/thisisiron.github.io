---
title: "Attention-based Ensemble for Deep Metric Learning 논문 정리"
categories: Image
---

----------------------------------

Image Retrieval 관련 [논문](https://arxiv.org/abs/1804.00382)을 공부하게 되면서 정리한 포스트입니다. 정리한 순서는 논문과 동일한 구조와 같습니다. 중요부분만 정리하려고 했는데 거의 생략한 부분이 없네요. 잘 못된 내용이나 수정 사항 혹은 의견이 있으시다면 댓글로 남겨주세요.

## Abstract
- Deep Metric Learning은 Deep Neural Network로 구성된 Embedding Function을 학습하는 것에 초점을 맞추고 있습니다.
- Embedding Function은 Embedding 공간 안에 유사한(similar) 이미지일수록 가깝고 유사하지 않은 이미지일수록 먼 거리를 갖고 있다고 이해할 수 있습니다.
- Ensemble은 최근 Deep Metric Learning의 SOTA에 적용되어 왔습니다.
- 해당 논문은 Multiple Attention Masks을 이용한 Atteontion-based Ensemble을 소개하고 있습니다.

## 1 Introduction
**Deep Metric Learning**에 먼저 알아보겠습니다.
1. 유사한 이미지들의 Feature Embedding은 서로 가까이에 있어야하고, 유사하지 않은 이미지들의 Feature Embedding은 서로 멀리 떨어져 있어야합니다.
2. 위의 조건을 만족하기 위해 Embedding Distance 관련 여러 Loss Function이 제안되고 있습니다.

**Ensemble**에 대해 알아보겠습니다.
Ensemble(앙상블)은 이미 넓게 사용되는 기술로써, 여러 모델들을 합친(Combined) 것을 얻기 위해 다수 Learners(하나의 Function으로 생각하시면 될 것 같습니다)을 학습합니다. 그럼으로써 개개의 모델들보다 더 좋은 성능을 보여주게 됩니다. 해당 논문에서는 여러 Learners에서 나온 Feature Embedding을 통합합니다.

해당 논문에서 제안하는 새로운 Framework는 Feature Embedding의 다양성(diversity)을 조장하는 것입니다.
1. 다수 Learners에게서 Multiple Attention Module을 가진 Architecture을 설계했습니다.
2. 서로 다른 Learners로 다른 Loaction을 처리함으로써 다양한 Feature Embedding Function이 학습됩니다.
3. Feature Embedding Function은 여러 Learner로부터 Feature Embedding을 구별하는 것을 목적으로 Divergence Loss로 정규화되었습니다.
4. M-way Attention-based Ensemble(ABE-M)이란 M개의 Diverse Attnention Masks을 가진 Feature Embedding을 학습하는 것을 의미합니다.
5. 밑에 보이는 이미지에서 (a)는 M-heads 모델을 의미하고, (b)는 해당 논문에서 소개하고 있는 ABE-M 모델을 의미합니다.
6. ABE-M은 적은 파라미터 수를 이용해서 더 좋은 결과를 보여주고 있습니다.

<img src="/assets/images/paper1_fig1.PNG"><br>

## 2 Related works
Deep Metric Learning의 목적은 Embedding Function $f: X\rightarrow Y$을 찾는 것입니다. data $X$ 공간 안에 있는 $x$를 feature embedding space $y$에 맵핑시켜주는 함수입니다. 만약 $f(x_i)$와 $f(x_j)$가 Metric에서 가깝다면 $x_i$와 $x_j$는 유사한 이미지입니다.<br>
Deep Metric Learning에서는 Contrastive Loss, Tripet Loss 등 여러 Loss Functino이 제안되었습니다. *최근 더 향상된 Loss function은 논문을 참고하시길 바랍니다.*<br>
Ensemble을 사용한 networks은 최근 리서치에서 single networks보다 높은 성능을 보여주고 있습니다.

1. 초기에는 초기화를 다르게 하거나 Training Sample의 다른 subset을 이용한 같은 Network의 평균을 기반으로한 방법
2. *pseudo-ensembles*라고 불리우는 Parameter Sharing 기법
3. Dropout도 높은 상관관계를 가진 지수적 네트워크를 갖는 ensemble 방법으로 해석될 수 있음(즉, 어떤 뉴런은 사용되고 어떤 뉴런은 사용되지 않으므로 엄청나게 많은 조합이 발생)
4. residual network도 상대적으로 shallow network의 ensemble처럼 수행
5. 개별 learners의 diversity(다양성)을 추구하는 새로운 **DivLoss**에 관한 효과적인 averaging strategy을 제안


**Attention Mechanism**
Attention Mechanism은 많은 컴퓨터 비전 문제에서 사용되었습니다. 초기에는 RNN 방법이 제안되었고 이후 fully convolutional attention networks 등 여러 방법들이 제시되었습니다.<br>
**ABE-M** 모델에서는 region generator에 의존하지 않는 diverse attention masks을 학습할 것입니다.

## Attention-based ensemble
### 3.1 Deep metric learning
$f: X\rightarrow Y$은 isometric embedding function가 되는 것입니다. $X$는 미지의(unknown) metric function $d_X$을 가진 $N_X$ 차원의 공간에 있습니다. $Y$는 알려진 metric function $d_Y$을 가진 $N_X$ 차원의 공간에 있습니다. 예를 들어 $Y$는 Euclidean distance 혹은 angular distance을 가진 Euclidean 공간에 있다고 할 수 있습니다.<br>
우리의 목적은 $D = \{(x(1), x(2), d_X (x(1), x(2)))|x(1), x(2) \in  X \}$에서 $f$을 추정하는 것입니다. 예를 들어 $D_C = \{(x, c)|x \in  X, c \in  C\}$($C$는 label을 의미)라는 데이터 셋에서 **contrastive metric constraint**는 다음과 같이 정의할 수 있습니다. $m_c$는 arbitrary margin을 의미합니다. 

$$
\begin{equation}
d_X(x_i,x_j)=0, \quad $if \; c_i=c_j \\
d_X(x_i,x_j)>m_c, \quad $if \; c_i \neq c_j \\
\end{equation}
$$

다음으로 $(x_i, c_i),\> (x_j, c_j), \> (x_k, c_k) \in D_C$에서 **triplet metric constraint**는 다음과 같이 정의할 수 있습니다. $m_t$는 margin을 의미합니다.

$$
\begin{equation}
d_X(x_i,x_j)+m_t<d_X(x_i,x_k), \quad c_i=c_j \enspace $and  c_i \neq c_j
\end{equation}
$$

이러한 metric constraint는 $f$을 모델링하는 방법이 아닌 $d_X$을 모델링하는 방법 중 일부입니다. Embedding function $f$는 $x_i,x_j \in X$에 대해 $d_X(x_i,x_j)=d_Y(f(x_i),f(x_j))$을 만족하면 isometric이거나 혹은 거리 보존 임베딩입니다.


### 3.2 Ensemble for deep metric learning
Deep metric learning의 고전적인 ensemble은 다수 embedding funtions의 metric 평균을 내는 방법입니다. ensemble metric funtion을 다음과 같이 정의할 수 있습니다. $f_m$은 독립적으로 학습되는 embedding function이며 learner라고도 불리웁니다.

$$
d_{ensemble, (f_1,...,f_M)}(x_i,x_j) = \frac{1}{M}\sum_{m=1}^{M}d_Y(f_m(x_i),f_m(x_j))
$$

고전적인 ensemble이외에도 두가지 embedding function을 고려해볼 수 있습니다. $s:X\rightarrow Z$ 와 $g:Z\rightarrow Y$ embedding function이 존재합니다. $X$는 미지의(unknown) metric function $d_X$을 가진 $N_X$ 차원의 공간에 있습니다. $Z$는 미지의(unknown) metric function $d_Z$을 가진 $N_Z$ 차원의 공간에 있습니다. $Y$는 알려진 metric function $d_Y$을 가진 $N_X$ 차원의 공간에 있습니다.<br>
이 두 함수를 $b(x) = g(s(x)), \enspace x\inX$라는 하나의 함수로 합칠 수 있습니다. combined function은 $X$와 $Y$ metric space 사이에 있는 $b:X\rightarrow Y$입니다. 2가지 Case에 대해 알아보겠습니다.
1. parameter sharing ensemble과 같이, 독립적으로 학습되는 다수의 $g$함수와 하나의 $s$함수를 가지고 다수의 embedding function $b_m:X\rightarrow Y$을 얻을 수 있습니다.

$$
b_m(x) = g_m(s(x))
$$

2. 다른 case에 대해 알아보면 다수의 $s$함수와 하나의 $g$함수로 이루어진 $b_m:X\rightarrow Y$을 얻을 수 있습니다.

$$
b_m(x) = g(s_m(x))
$$

$s_m$은 label을 보존하는 것뿐만 아니라 metric 정보도 보존하고 있습니다. 다른 말로하자면, label에 관한 point을 다수의 $s_m$을 이용해 $Z$ 공간 안에 다수의 지역으로 맵핑할 수 있습니다. 그리고 이것을 $Y$ 공간 안에 다수의 지역으로 맵핑할 수 있습니다.<br>
예를 들어 classification의 ensemble이라고 한다면 $g$함수는 label의 분포를 추정하는 함수가 될 것입니다. 모든 $s_m$은 label 보존 함수일 것입니다. $s_m$의 output은 $g$함수의 input이기 때문입니다. 즉 $s_m$ 함수가 label 정보를 보존하고 있기 때문에 $g$함수에서 그것을 토대로 추정할 수 있게 되는 것입니다.

$g:Z\rightarrow Y$에서 $Y$ 공간 안에 있는 $x$로부터 맵핑된 $y_m$들을 서로 멀어지게 한다면, $Z$ 공간 안에 있는 $x$로부터 맵핑된 $z_m$도 서로 멀어지게 됩니다.

### 3.3 Attention-based ensemble model
주로 2 가지 부분으로 나누어 보면 Feature Extraction Module $F(x)$와 Attention Module $A(x)$로 볼 수 있습니다. Feature Extraction에서 일반적으로 Multi-layer perceptron model을 다음과 같이 가정합니다.

$$
F(x) = h_l(h_{l-1}(...(h_2(h_1(x)))))
$$

여기서 브래이킹 포인트 i를 가지고 2 부분으로 나눌 수 있습니다. $S(\cdot)$은 $h_l, h_{l-1},...,h_{i+1}$이고 $G(\cdot)$은 $h_{i},h_{i-1},...,h_1로 나눌 수 있습니다. 이것을 $S(\cdot)$는 **spatial feature Extractor**라고 부르고, $G(\cdot)$는 **global feature embedding function**라고 부릅니다.

Attention Module에서는 채널, 너비, 높이 3차원을 가진 일반적인 Multi-layer perceptron model을 가정해봅시다. 위의 두 Module을 통합한 embedding function $B_m(x)$을 다음과 같이 정의할 수 있습니다. 여기서 $m$은 learner을 의미합니다.

$$
B_m(x) = G(S(x) \circ A_m(S(x)))
$$

$\circ$ 기호는 [element-wise product](https://en.wikipedia.org/wiki/Hadamard_product_(matrices))을 의미합니다. 개별 learner은 그들 자신의 Attention Moduel을 갖고 있는 동안 same feature extraction은 다른 learner에게 공유됩니다. Element-wise product을 위해 $A_m(S(x))$은 $S(x)$ output 크기와 동일한 attention mask 결과가 나오게 됩니다.


### 3.4 Loss
Attention Model의 Loss 함수는 다음과 같습니다.

$$
L_{div}(\{x_i,c_i\}) = \sum_{m}L_{metric,(m)}(\{x_i,c_i\})+\lambda_{div}L_{div}(\{x_i\})
$$

각 기호에 대해 설명드리겠습니다. 
- $\{x_i,c_i\}$는 training sample과 label
- $L_{metric,(m)}(\cdot )$은 $m$-th learner에 대한 isometric embedding의 loss함수
- $L_{div}(\cdot )$은 각 learner $B_m(x)$의 feature embedding을 diversifying 하기 위한 정규화 식
- $\lambda_{div}$은 정규화 강도를 조절하는 weighting parameter

Divergence Loss $L_{div}$은 다음과 같이 정의됩니다.

$$
L_{div}(\{x_i\})\sum_{i}\sum_{p,q}max(0,m_{div}-d_y(B_p(x_i),B_q(x_i))^2)
$$

각 기호에 대해 설명드리겠습니다. 
- $x_i$는 training sample을 의미
- $d_Y$는 Y 공간 안의 metric
- $m_{div}$는 margin을 의미
- $B_p(x_i),B_q(x_i)$는 다른 두 learner에게 embedded된 하나의 이미지의 feature embedding을 의미
- 위 pair가 same label이면 positive pair, different label이면 negative pair 명명

Divergence Loss는 각각 learner에게 input image의 임베딩된 point 사이의 distance을 증가시킴으로써 이미지의 다른 부분을 처리하도록 합니다. 아래의 그림을 보면 Divergence Loss는 동일한 input을 사용하는 different learner의 feature embedding을 끌어 흩어지게 합니다.

<img src="/assets/images/divergence_loss.PNG"><br>

Attention Mask는 learner사이에서 중복될 수도 있습니다. 또 Attention Mask 일부는 작은 지역에, 다른 일부는 작은 지역을 포함한 좀 더 큰 지역에 집중하는 것이 가능합니다.

## 4 Implementation
해당 논문은 GoogLeNet 기반으로 구현되었습니다. 이미지를 통해 기본적인 구조를 확인하고 가겠습니다.

<img src="/assets/images/paper1_fig3.PNG"><br>

Attention Module에 있는 conv을 제외한 conv1, pool1, incenption(3a-3b), ..., pool5까지 GoogLeNet에 존재합니다. Attention Module에 있는 conv는 element-wise product을 수행하기 위해서 $S(\cdot)$와 크기를 맞춰야 하므로, $1\times 1$을 가진 480 Knerel을 이용해서 크기를 맞추게 됩니다.

Distance Metric Loss function은 Contrastive Loss을 사용합니다. 정의는 다음과 같습니다.

$$
\begin{equation}
L_{metric,(m)}(\{(x_i,c_i)\}) = \frac{1}{N}\sum_{i,j}[m_c-D^2_{m,i,j}]_+ +y_{i,j}D^2_{m,i,j}, \\
D_{m,i,j} = d_Y(B_m(x_i),B_m(x_j)) \\
\end{equation}
$$

각 기호에 대해 설명드리겠습니다. 
- $\{(x_i,c_i)\}$는 training sample과 label을 의미
- $N$은 training set의 크기
- $y_{i,j}$는 $c_i$와 $c_j$가 동일한지 아닌지 알려주는 binary indicator을 의미<br>
  즉 같으면 1, 다르면 0으로 표기할 수 있습니다. 
- $[\cdot ]_+$는 hinge function을 의미하며 $max(0,\cdot)$ 수식으로 표현
- $m_c$는 constrastive loss의 margin을 의미
- $m_c$와 $m_{div}$는 1로 세팅

해당 모델 구현 관련 내용 정리입니다.
- Caffe Framework을 이용
- ImageNet ILSVRC dataset의 pre-trained network을 초기 값으로 사용
- 마지막 layer와 attention module의 conv layer는 Glorot가 제안한 초기화 방법을 사용<br>
  우리가 흔히 아는 Xavier 초기화 방법이라고 생각하면 됩니다. 
- momentum을 가진 SGD Optimizer을 사용했으며 momentum 값은 0.9로 세팅
- Batch Size는 64 사용
- 64개 중 일단 Random으로 32 image을 추출하여 16개는 positive pair, 다음 16개는 negative pair로 구성(해당 작업을 반복)
- Embedding Size는 512을 사용<br>
  M개 Learner을 사용하게 되면 각 Learner의 Embedding Size는 $512/M$


전처리 관련 내용 정리입니다.





## 5 Evaluation
이 논문에서의 실험은 Image retrieval task 데이터셋을 이용했습니다. 자세한 데이터넷은 논문을 참고해주시기 바랍니다. 

## 6 Experiments
### 6.1 Comparison of ABE-M with M-heads
Embedding size을 다르게 하면서 ABE-M 모델과 M-heads 모델을 비교했습니다. 논문에서 제시한 방법이 상당한 차이로 M-heads 모델을 능가했습니다.

<img src="/assets/images/paper1_fig4.PNG"><br>

그래프를 보고 도출되는 점
- M-heads와 비교 했을 때 $G(\cdot)$을 공유하기 때문에 ABE-M모델의 파라미터의 수는 많이 적음
- Attention Module의 extra compuation 때문에 higher flops을 요구하게 됨
- 위의 차이는 M이 증가하면서 점점 더 중요하지 않은 부분이 됨

<img src="/assets/images/paper1_table1.PNG"><br>

표를 보고 도출되는 점과 논문에서 제시하는 것
- ABE-1은 One attention Module이며 Ensemble이 아니고 Divergence Loss을 사용하지 않음
- ABE-1은 1-head 모델과 유사하게 작동
- $ $ABE-$M^{512} $의 성능은 M이 증가할수록 증가함
- 개개의 Learner의 Embedding Size는 줄어들더라도 M이 증가하게 되면 개개의 Learner의 성능도 증가
- $ $ABE-$1^{64}, \>$ABE-$2^{128},\> $ABE-$4^{256},\> $ABE-$8^{512} $은 모두 같은 Embedding Size 64을 가짐

### 6.2 Effects of divergence loss

## 7 Conclusion
이 논문에서 Deep metric learning에서 Ensemble의 새로운 Framework을 보여주었습니다. 
- 이미지의 부분들을 처리하는 Attention-based Architecture을 사용
- Divergence Loss는 각각의 learner가 Feature Embedding을 다양화하도록 만듬
- Divergence Loss을 적용함으로써 Ensemble의 성능을 올림뿐만 아니라 개개의 learner의 성능도 향상


## Reference
[Attention-based Ensemble for Deep Metric Learning](https://arxiv.org/abs/1804.00382)<br>