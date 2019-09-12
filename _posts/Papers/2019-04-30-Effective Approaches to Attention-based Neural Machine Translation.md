---
title: "Effective Approaches to Attention-based Neural Machine Translation 논문 정리"
categories: NLP
---

-----------

해당 [논문](https://aclweb.org/anthology/D15-1166)을 공부하게 되면서 정리한 포스트입니다. 정리한 순서는 논문과 동일한 구조와 같습니다. 잘 못된 내용이나 수정 사항 혹은 의견이 있으시다면 댓글로 남겨주세요.

## Abstract
- Attention mecahanism은 번역 중에 원본 문장(source sentence)에 선택적으로 집중하여 최근에 NMT 결과를 향상
- 두 가지 Attention mechanism이 존재: global, local
- 해당 논문에서는 English-German 번역 데이터를 사용
- non-attentional system에 비해 local attention은 5.0 BLEU 스코어 향상을 이룸

## Introduction
- NMT는 end-to-end 열풍에서 학습되고 아주 긴 문장을 일반화시킬 수 있는 능력을 가진 대개 큰 Neural Network입니다.
- 최근에, NMT decoder들은 표준적인 MT에 매우 복잡한 decoder들과 다르게 간단합니다.
- global approach: 한번에 모든 원본 문장을 처리
- local approach: 한번에 원본 문장의 subset만큼 처리 
- global approach는 Bahdanau et al. 2015 에서 제시한 모델과 유사
- local approach는 Xu et al. 2015 에서 제시한 hard와 soft의 attention 모델을 조합한 것과 유사
- local approach는 global 모델 또는 soft attention에 비해 계산적으로 덜 expensive함
  
## Neural Machine Translation
Neural Machine Translation는 원본 문장에 대한 조건부 확률($p(y|x)$)을 직접적으로 설계하는 neural network입니다.
위 조건부 확률에서 x는 원본 문장(source sentence) $x_1, ..., x_n$, y는 번역된 문장(target sentence) $y_1, ..., y_m$로 표현하고 있습니다.


기본적인 NMT의 형태는 2가지 형태로 구성되어 있는데 encoder와 decoder로 구성되어있습니다.
encdoer는 각 source sentence에 대한 representation s을 계산하고 deocder는 하나의 target word을 생성합니다.
따라서 다음과 같이 조건부 확률을 분해합니다.

$$
log \ p(y|x) = \sum_{j=1}^{m} log \ p(y_j|y_{<j},s)
$$

이후 내용은 NMT에 관한 다른 논문에서 사용한 모델의 구조에 대해 설명하고 있습니다.

한 가지 더 다뤄볼 것은 마지막에 이 논문에 대한 내용입니다. 

이 논문에서 사용한 모델은 **stacking LSTM** 구조이며 형태는 다음 그림과 같습니다.

<img src="/assets/images/paper3_figure1.PNG"><br>

Zaremba et al. 2015에서 정의한 LSTM unit을 사용하며, Training objective는 다음과 같습니다.

$$
J_t = \sum_{(x,y\in D)} -log \ p(y|x)
$$

여기서 D는 training corps입니다.

## Attention-based Models
앞에서도 언급했듯이 global과 local 2가지 종류가 있습니다. 
모든 source sentence에 attention하냐? 또는 source sentence의 일부(subset)에 attention하냐?에 따라 방식이 다르게 됩니다.

수식에서 나올 공통적인 용어에 대해 정의하고 넘어가겠습니다.
- $t$는 decoding의 각 단계를 의미합니다.
- $h_t$는 decoder의 input에 대한 가장 상위층 Layer의 hidden state을 의미합니다.
- $c_t$는 context vector을 의미하며, $y_t$을 예측할 수 있도록 도와주는 그에 연관된 원본측(source-side) 정보를 담고있는 벡터입니다.

$h_t$와 $c_t$가 주어지면 간단한 concatenation layer을 통해 attentional hidden state을 다음과 같이 정의할 수 있습니다.

$$
\widetilde{h} = tanh(W_c [c_t;h_t])
$$

attentional vector $h_t$는 softmax을 통과하여 다음과 같은 식을 정의할 수 있습니다.

$$
p(y_t|y{<t},x) = \text{softmax}(W_s\widetilde{h_t})
$$

### Global Attention
Global Attention은 모든 source sentnece을 고려하는 모델입니다.

이 모델의 경우에, variable-length alignment vector $a_t$의 길이는 source side의 time steps의 수와 같습니다.
즉, 우리가 코드를 작성할 때 설정하는 source side의 MAX length와 같습니다.

이 벡터는 다음과 같은 수식에서 도출됩니다.

$$
a_t(s) = \text{align}(h_t,\overline{h_s}) \\ \quad 
= \frac{\text{exp}(score(h_t,\overline{h_s}))}{\sum_{s'}\text{exp}(score(h_t,\overline{h_s}))}
$$

다음은 *content-based* 함수로 세 가지 타입을 고려할 수 있는 *score*가 존재합니다.

$$
\text{score}(h_t,\bar{h_s}) = 
\left\{\begin{matrix} h_t^\top \bar{h_s} && dot
\\ h_t^\top W_a \bar{h_s} && general
\\ v_a^\top \text{tanh}(W_a[h_t;\bar{h_s}]) && concat

\end{matrix}\right.
$$

<img src="/assets/images/paper3_figure2.PNG"><br>

게다가, 초기에 attention model을 사용할 때는 *location-based*을 사용하였습니다.
이것은 단순히 alignment scores을 target hidden state $h_t$만 사용한 수식입니다.

$$
a_t = \text{softmax}(W_ah_t)
$$

alignment vector가 주어지면 context vector는 모든 source hidden states에 걸친 weighted average을 계산하면 됩니다.

Bahdanau et al. 2015 논문에서 제안한 모델과 global model은 유사합니다.
몇몇 다른 핵심 부분이 존재합니다. 

첫째로, 이 논문에서는 간단히 encoder와 decoder의 LSTM 가장 상위층의 hidden states을 사용했습니다.
한편 Bahdanau et al. 2015에서 제안한 모델은 bi-directional encdoer와 uni-directional decoder의 concatenation을 사용했습니다.

둘째로, 계산 과정이 단순합니다. 현 논문이 제안한 계산 과정은 $h_t \ \rightarrow  \ a_t \ \rightarrow \ c_t \ \rightarrow  \ \widetilde{h_t}$이고,
비교 모델의 계산 과정은 $h_{t-1} \ \rightarrow  \ a_t \ \rightarrow \ c_t \ \rightarrow  \ \widetilde{h_t}$입니다.

비교 모델에서는 concat product만 사용하여 alignment을 구하였지만 이후 이 논문에서는 다른 함수가 더 좋은 결과를 도출하는 것을 보여줄 것입니다.

### 3.2 Local Attention



