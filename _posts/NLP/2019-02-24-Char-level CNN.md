---
title: "Character-level Convolutional Networks for Text Classification 논문 정리"
categories: NLP
---

----------------------------------

해당 [논문](https://arxiv.org/pdf/1509.01626.pdf)을 공부하게 되면서 정리한 포스트입니다. 정리한 순서는 논문과 동일한 구조와 같습니다. 잘 못된 내용이나 수정 사항 혹은 의견이 있으시다면 댓글로 남겨주세요.

## Abstract
- Character-level ConvNets을 이용하여 Text Classification 수행
- BOW, N-grams, TFIDF 전통적인 모델과 word-based ConvNets, RNN 딥러닝 모델을 비교

## Introduction
논문 당시, Text Classification의 모든 techniques는 word 기반이었습니다. N-grams과 같이 간단한 통계적 모델은 보통 좋은 결과를 보여주었습니다.

한편, 연구자들은 ConvNet은 raw signal로부터 정보를 추출하는데 용이하다는 것을 발견했습니다. 이 논문에서는 Character level에서 text을 다룰 것이고 1차원 ConvNets을 적용하는 방법에 대해 살펴볼 것입니다.

ConvNets은 언어의 syntactic 또는 semantic 구조에 대한 지식 없이 단어의 distributed(분산) embedding 또는 discrete(이산) embedding에 적용될 수 있습니다. 이런 방법은 기존의 모델들에 비해 경쟁력이 있음이 증명되었습니다.

ConvNets 접근법은 단어를 기초로 사용합니다. 이 방식에서 words 또는 word n-gram level 에서 추출된 character-level features가 distributed representation을 형성합니다.

이 논문에서는 매우 큰 데이터 set을 학습할 때 위에서 언급한 ConvNets이 syntactic 또는 semantic 구조에 대한 지식을 요구하지 않는 결론이외에도 words의 지식을 사용하지 않아도 되는 것을 보여줄 것입니다.

Characters로만 작동하게 되면 misspelling이나 이모티콘과 같은 비정상적인 character 조합들이 자연스럽게 학습될 수 있는 이점을 갖습니다.

## 2 Character-level Convolutional Networks
### 2.1 Key Modules
주요 구성 요소는 **temporal convolutional module**(1D Convolution)입니다.
Discrete input function
$$
g(x)\in [1,l]\rightarrow \mathbb{R}
$$

Discrete kernel function
$$
f(x)\in [1,k]\rightarrow \mathbb{R}
$$

$h(y)$ 정의
$$
\text{convolution } h(y)\in [1,\left \lfloor (1-k)/d \right \rfloor + 1]\rightarrow \mathbb{R}, \text{ stride }d
$$

$h(y)$는 $f_{i,j}(x)$와 $g_{i}$를 Conv 연산 후 Sum을 합니다.
$$
h(y) = \sum_{x=1}^{k}f(x)\cdotg(y\cdot d - x + c)
$$

- $c=k-d+1$: offset constant
- $f_{i,j}(x) \enspace (i=1,2,...,m \text{ and } j=1,2,...,n)$: weights
- $g_{i}$: input features
- $h_{j}$: output features
- $m$: input feature size
- $n$: output feature size

다른 Key module은 **temporal max-pooling**입니다. 이것은 Computer Vision에서 사용되는 1D max-pooling module입니다.
다음 정의를 통해 알아보겠습니다.

$$
g(x)\in [1,l]\rightarrow \mathbb{R}
$$

$$
h(y)\in [1,\left \lfloor (1-k)/d \right \rfloor + 1]\rightarrow \mathbb{R}
$$

$$
h(y)=\max^k_{x=1}g(y\cdot d-x+c)
$$

이런 max-pooling을 통해 6 layers보다 더 깊게 ConvNets을 학습할 수 있었습니다.

이 모델에서 사용한 Activation 함수, Optimazation와 하이퍼파라미터에 대해 알아보겠습니다.
- Activation function: ReLU 함수 사용
- Optimazation: SGD 사용 (momentum: 0.9)
- initial step size(learning rate): 0.01
- minibatch 크기: 128
- 매 Epoch은 각 클래스마다 균등적으로 고정된 크기의 training samples 랜덤 샘플링


## 2.2 Character quantization
논문에서 언급하는 모델에서는 input으로 encoded characters 형태의 sequence를 입력 받습니다. 
input 언어(해당 논문은 영어)에 맞는 $m$ 크기에 alphabet으로 인코딩합니다.
각 character을 "one-hot" encoding합니다.
character의 문장은 고정된 $l_{0}$ 길이를 가진 $m$ 크기의 vector로 바뀌어졌습니다.
$l_{0}$ 길이를 넘어선 character는 무시하며, 빈 문자를 포함하여 alphabet에 없는 문자는 값이 모두 0인 벡터로 바꿉니다.

alphabet은 다음과 같이 70개의 문자로 구성되어 있습니다.

$$
text{abcdefghijklmnoparstuvwxyz0123456789} \newline
text{-,;.!?:'"/\|_@#$%^&*~`+-=<>()[]{}}
$$

대소문자를 구별해서 사용하는 모델은 이후에 성능을 비교하면서 알아보겠습니다.

## 2.3 Model Design
Large와 Small의 ConvNets 2개을 설계했습니다. 총 9 Layer로 이루어져 있으며 6개의 ConvNet과 3개의 Fully-connected layer로 구성되어 있습니다.
다음 이미지를 통해 해당 모델의 구성을 보겠습니다.

<img src="/assets/images/paper2_fig1.PNG"><br>

input에서 feature의 수는 70(alphabet의 구성 요소)이며, input feature 길이는 1014입니다.
1014개 character는 충분히 text 안의 중요한 요소를 캡처할 수 있다고 보입니다.

또한 이 모델에서는 정규화를 위해 2개 dropout을 사용했는데 3 fully-connected layer 사이에 넣어놨습니다.
dropout의 probability는 0.5입니다.

Table 1은 Convnet의 구성 요소에 대한 것이고 Table 2는 Fully-connected에 대한 구성 요소입니다.

<img src="/assets/images/paper2_table1.PNG"><br>

<img src="/assets/images/paper2_table2.PNG"><br>

weights(가중치)은 Gaussian Distribution을 이용하여 초기화했습니다. 이때 사용한 mean과 standard deviation은 large의 경우 (0, 0.02)이며, small은 (0,0.05)입니다.

문제마다 input length(길이)는 달라질 수 있으며, 이 모델에서는 $l_{0}=1014$이며 마지막 ConvNet의 output length는 $l_{6}=(l_{0}-96)/27$입니다.
이 길이와 Layer 6의 크기(여기서는 256)를 곱하게 되면 fully-connected layer의 input으로 주어지게 됩니다.


## 2.4 Data Augmentation using Thesaurus
많은 연구자들이 적절한 Data Augmentation은 generalization error을 통제하는데 유용하다고 여겨지는 기술이라고 여깁니다. 이러한 기술들은 image 또는 speech recognition에서 적절하지만
text에서는 그렇지 않을 수 있습니다.

character의 정확한 순서는 엄격한 구문과 의미를 형성하기 때문입니다.
따라서 가장 좋은 방법은 사람이 그 문장을 바꾸어 만드는 것인데, 이 방법은 비현실적이며 과도한 비용이 듭니다.
이에 대한 해결책은 단어와 구를 동의어로 대체하는 것입니다.

해당 논문에서는 Thesaurus(유의어사전)을 이용함으로써 Data augmentation을 실험했습니다.
이러한 모든 동의어는 빈번하게 보이는 의미로 semantic(의미론적) 유사성에 의해 등급을 매겼습니다.
즉, 자주 사용되는 순으로 정렬된 것입니다.

Text에서 대체할 수 있는 단어들을 모두 뽑고 대체될 수 있는 것들 중 $r$을 랜덤적으로 선택합니다.

$r$의 확률은 $P[r] ~ p^r$에서 parameter $p$을 가지고 gemetric distribution에 의해 결정됩니다.

주어진 단어의 동의어의 index $s$는 $P[s] ~ q^s$에서 또 다른 gemetric distribution에 의해 결정됩니다.

이 방식에서 선택된 동의어가 가장 빈번하게 보이는 의미에서 멀어질 때 선택된 동의어 확률이 작아집니다.
$p=0.5$와 $q=0.5$을 따르는 gemetric distribution을 사용할 것입니다.