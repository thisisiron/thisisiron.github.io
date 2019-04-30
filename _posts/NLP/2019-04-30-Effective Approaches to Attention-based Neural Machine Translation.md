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