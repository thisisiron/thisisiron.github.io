---
title: "Batch Nomalization란"
categories: MachineLearning
---

## Idea
입력층에만 정규화하는 것이 아니라 Hidden Layer의 값도 정규화 하는 방식이다.

- <img src="https://latex.codecogs.com/gif.latex?a^{[L]}" title="a^{[L]}" />이 아니라 보통 <img src="https://latex.codecogs.com/gif.latex?z^{[L]}" title="z^{[L]}" />을 Nomlaize한다.

- <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\mu&space;=&space;\frac{1}{m}\sum_{i}z^{(i)}" title="\mu = \frac{1}{m}\sum_{i}z^{i}" />

- <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\sigma&space;^2&space;=&space;\frac{1}{m}\sum_{i}(z^{(i)}&space;-&space;\mu&space;)^2" title="\sigma ^2 = \frac{1}{m}\sum_{i}(z^{(i)} - \mu )^2" />

- <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;z^{(i)}_{norm}&space;=&space;\frac{z^{(i)}-\mu}{\sqrt{\sigma&space;^2&space;&plus;&space;\varepsilon&space;}}" title="z^{(i)}_{norm} = \frac{z^{(i)}-\mu}{\sqrt{\sigma ^2 + \varepsilon }}" />

- <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\tilde{z}^{(i)}&space;=&space;\gamma&space;z^{(i)}_{norm}&space;&plus;&space;\beta" title="\tilde{z}^{(i)} = \gamma z^{(i)}_{norm} + \beta" />

$$$ z^{(i)} $$$ 대신에 $$$ \tilde{z}^{(i)} $$$을 사용하라

