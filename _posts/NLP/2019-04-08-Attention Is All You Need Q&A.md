---
title: "Attention Is All You Need Q&A"
categories: NLP
---

----------------------------------

해당 [논문](https://arxiv.org/pdf/1706.03762.pdf)에 관하여 세미나 후 팀원들의 질문 사항을 답변했었던 내용입니다. 
잘 못된 내용이나 수정 사항 혹은 의견이 있으시다면 댓글로 남겨주세요. 
Attention Is All You Need 논문 내용은 아직 포스팅하지 않았습니다.


## PPT 발표 자료
[PPT 발표 자료](https://docs.google.com/presentation/d/1VveT3cGvf4JcPhonSwXi6_0M44MJMJ8Ska2gD7jkoXM/edit?usp=sharing)

## Self-Attention이 이전에 존재했던 기법인가요?

= **intra-attention** 라고도 부릅니다

정의:  하나의 문장의 representation을 계산하기 위해 그 문장 다른 위치와 관련된 Attention 기법

Self-attention 기법은 현재 단어와 이전 문장의 부분의 상관 관계를 학습이 가능하게 함 

(단어 사이에 관계를 보여주도록 한다고 이해하면 될 것 같아요)

→ machine reading, 요약, 이미지 설명 생성에 좋은 결과를 보여줌

[](https://www.notion.so/8bbbabf0aa5e4f90aea97d11de65f4db#39da8bcdc69d419596bd7b40d58d6f79)

[Long Short-Term Memory-Networks for Machine Reading](https://arxiv.org/pdf/1601.06733.pdf)
[A STRUCTURED SELF-ATTENTIVE SENTENCE EMBEDDING](https://arxiv.org/pdf/1703.03130.pdf)

Self-Attention이랑 **Scaled Dot-Product**을 혼용하여 설명한 것 같아서 Scaled Dot-Product은 Attention is all you need에서 제시된 내용입니다.

[](https://www.notion.so/8bbbabf0aa5e4f90aea97d11de65f4db#4c375b1067c542699939173845f5b044)

## 왜 Q, V, K로 나누는가요?

제 생각은 단순히 Attention함수에서 Q, K, V 계산을 통해 Output을 만들어 내기위함 아닐까 합니다.

해당 단어가 어떤 단어와 연관이 있는지 알아내기 위함?입니다.

[](https://www.notion.so/8bbbabf0aa5e4f90aea97d11de65f4db#cbdc4e091dca430c9c834b5a054bdff9)

위의 예시에서 q1(Thinking)은 k1(Thinking)과 k2(Machines)와 곱해지는데 이 결과 값은 어떤 단어와 해당 단어가 연관이 있는지 알 수 있습니다.

정리를 해보자면 query는 어떤 단어와 관계가 있는지 알아내기 위해 모든 key들과 계산을 하게 됩니다.

1. query와 key을 곱한 후 그 결과를 softmax을 통해 확률 값으로 바꿉니다.
2. 확률 값을 이제 value와 곱하여 attention 값을 계산하게 됩니다.

## Positional Encoding 값은 Input Embedding에 더하는 건가요?

발표 때도 Sum이라고 했지만 너무 확실하게 말씀 못드려서 다시 적어보네요

다음과 같은 수식을 통해

[](https://www.notion.so/8bbbabf0aa5e4f90aea97d11de65f4db#cf698cc24a1f470a98c455398c12cd9b)

Input Embedding과 같은 크기의 Positional Encdoing을 구하게 됩니다.

그 값을 단순히 Sum하면 됩니다.

## 다음과 같은 수식은 뭔가요?

[](https://www.notion.so/8bbbabf0aa5e4f90aea97d11de65f4db#f4b3735541e24dee9961b1284a61b742)

학습 단계에 따라 학습률을 변하게 해주는 공식인데 warmup_stpe은 4000으로 설정하여 사용

1인 경우 min(1, 0.00000395284) * 0.04419417382

100000일 경우, min(0.00316227766, 0.395284)* 0.04419417382

초기에는 선형적으로 증가시키고 이후 역수를 통해서 감소시키는 것이 목적이라고 하네요

## Label Smoothing은 뭔가요?

직관적으로는 설명 먼저 드리자면 Ont-Hot Encdoing 방법은 [1, 0, 0, 0, 0] 으로 표현하게 되는데

Label smoothing은 [0.9 0.025, 0.025, 0.025, 0.025]로 표현하게 됩니다.

수식을 보면 다음과 같습니다.

[](https://www.notion.so/8bbbabf0aa5e4f90aea97d11de65f4db#dc1d7f991f984cbf8b1bff2c0c9ae075)

[Rethinking the Inception Architecture for Computer Vision](https://arxiv.org/pdf/1512.00567.pdf)

저도 해당 논문의 Session 7만 읽고 말씀드리는거라 궁금하신 내용이나 틀린 내용이라면 말씀해주세요..

q는 x값이 주어질 때 k일 확률을 구하는 함수입니다. 즉, x(단서)가 주어지면 k(class)일 확률은 얼마나되냐? 라고 묻는 것입니다.

델타 값은 0 혹은 1로 설정되어 있는 것인데 Onthot Encdoing을 떠올리면 될 것 같아요

즉 k와 y가 같다면 1, 아니면 0이라는 의미인데 첫 번째 Clas을 표현한다고 하면 

$$k_1 = 1 / k_2 = 0 / k_3=0 / k_4=0 / k_5=0 → [1, 0, 0, 0, 0]$$

와 같이 표현할 수 있습니다.

엡실론(e같이 생긴애) 값을 0.1(Attention is all you need논문에서는 0.1로 설정)으로 설정했을 경우 

위의 수식에 대입해보면 정답인 경우 0.9로 설정하게 되고 아닌 값들에게도 어느정도 여지의 값을 부여하게 됩니다.

그럼 이제 이걸 왜 하냐고 생각하실텐데 이것을 하게 되면 Overfitting을 방지하게 해준다고합니다.

### Reference
[Attention Is All You Need](https://arxiv.org/pdf/1706.03762.pdf)
[Images about Attention function](http://jalammar.github.io/illustrated-transformer/)