---
title: "Attention-based Ensemble for Deep Metric Learning 정리"
categories: Image
---

Image Retrieval 관련 [논문](https://arxiv.org/abs/1804.00382)을 공부하게 되면서 정리한 포스트입니다. 정리한 순서는 논문과 동일한 구조와 같습니다. 잘 못된 내용이나 수정 사항 혹은 의견이 있으시다면 댓글로 남겨주세요.

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


## Reference
[Attention-based Ensemble for Deep Metric Learning](https://arxiv.org/abs/1804.00382)<br>