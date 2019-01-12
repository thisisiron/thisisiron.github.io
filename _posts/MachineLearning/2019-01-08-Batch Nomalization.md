---
title: "Batch Nomalization란"
categories: MachineLearning
---

## Idea
입력층에만 정규화하는 것이 아니라 Hidden Layer의 값도 정규화 하는 방식입니다.<br>
여러 논쟁이 있지만 $$a^{[L]}$$이 아니라 보통 $$z^{[L]}$$을 Nomlaize합니다.<br>
다음과 같이 작동합니다.
- $$\mu = \frac{1}{m}\sum_{i}z^{i}$$

- $$\sigma ^2 = \frac{1}{m}\sum_{i}(z^{(i)} - \mu )^2$$

- $$z^{(i)}_{norm} = \frac{z^{(i)}-\mu}{\sqrt{\sigma ^2 + \varepsilon }}"$$

- $$\tilde{z}^{(i)} = \gamma z^{(i)}_{norm} + \beta$$

활성화 함수에 전달할 때 $$ z^{(i)} $$ 대신에 $$ \tilde{z}^{(i)} $$을 사용하세요.

