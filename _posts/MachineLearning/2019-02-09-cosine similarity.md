---
title: "About cosine similarity"
categories: MachineLearning
---


## Image Retrieval에서 Similarity
Image Retrieval에 관해 공부하면서 Simliarity에 알게 된 것을 정리하려고 합니다. Image Retrieval은 Query Image을 주게 되면 그것과 유사한 Image을 Database에서 찾아서 반환하게 됩니다. 간단하게 이때 유사한 정도를 나타내는 것을 **Similarity**입니다. <br>
Similarity로 Query Image와 유사함을 순위를 매겨 가져올 수 있습니다. 이 포스팅에서는 2가지 방법에 대해 소개하겠습니다.

## The Euclidean Distance
$\mathbf{R^n}$ 안에 존재하는 $x$와 $y$ 두 벡터가 존재한다고 가정합시다. 그리고 이 두 벡터의 Euclidean Distance의 공식은 다음과 같습니다.

$$
\begin{equation}\begin{aligned}
d_{euclid} &= {\Vert x - y \Vert}_2 \\
&= \sqrt{\sum_{i=1}^{n}(x_i - y_i)^2} \\
  &= \sqrt{ {\Vert x \Vert}^2 + {\Vert y \Vert}^2 - 2x\cdot y }\\
\end{aligned}\end{equation}
$$

만약에 Image A와 Image B의 Euclidean Distance 값이 Image A와 Image C의 거리보다 작다면 Image B는 Image C보다 Image A와 유사하다는 것을 알 수 있습니다.

## The Cosine Similarity
Cosine Similarity도 자주 사용되는 방법 중 하나입니다. $x$와 $y$ 두 벡터가 존재할 경우 Cosine Similarity는 다음과 같이 정의됩니다.

$$
\begin{equation}
\cos(\theta) = \frac{x\cdot y}{\Vert x \Vert \cdot \Vert y \Vert}
\end{equation}
$$

$\theta$는 $x$와 $y$ 벡터 사이의 각을 의미합니다. 다음과 같이 기하학적으로 생각해봅시다.

1. $x$와 $y$와 같은 방향을 취하고 있을 경우 $\theta = 0$이 되고, $\cos(0)=1$이 됩니다.
2. $x$와 $y$와 수직인 경우 $\theta = \frac{\pi}{2}$이 되고, $\cos(\frac{\pi}{2})=0}$이 됩니다.
3. $x$와 $y$와 반대 방향을 취하고 있는 경우 $\theta = \pi$이 되고, $\cos(\pi)=-1}$이 됩니다.

다음과 같은 그래프 형태를 보이게 됩니다.
<img src="/assets/images/cosine.PNG"><br>

## The Cosine Distance
Cosine Distance는 $x$와 $y$ 두 벡터의 각 차이를 측정한 것입니다. 그래프를 일단 보자면,

- Cosine Similarity 그래프를 x축으로 뒤집습니다. 왜냐하면 유사한 벡터는 가깝고 다른 벡터는 멀리있길 원하기 때문입니다.
- 거리는 항상 양수이기 때문에 한 단위(+1)를 올렸습니다.

<img src="/assets/images/cosine_distance.PNG"><br>

즉, $dist(x,y) = 1-\cos(\theta) = 1-sim(x,y)$ 식이 도출됩니다.

## Cosine Distance과 Euclidean Distance 관계
Image Retrieval에서 Feature Vectors은 종종 unit vecotr을 얻기 위해 $L_{2}$ nomalized로 변환합니다.<br>
Euclidean Distance식에서 $x$와 $y$ 두 벡터는 다음과 같이 변형할 수 있습니다.

$$
\begin{equation}\begin{aligned}
d_{euclid} &= \sqrt{\sum_{i=1}^{n}(x_i - y_i)^2} \\
  &= \sqrt{ {\Vert x \Vert}^2 + {\Vert y \Vert}^2 - 2x\cdot y }\\
  &= \sqrt{ 2 - 2x\cdot y }\\
  &= \sqrt{ 2(1 - x\cdot y) }\\
\end{aligned}\end{equation}
$$

### Reference
[Why is one minus cosine similarity equal to cosine distance?](https://www.quora.com/Why-is-one-minus-cosine-similarity-equal-to-cosine-distance)<br>
[https://jdhao.github.io/2017/10/24/image-similarity-measure-image-retrieval/](https://jdhao.github.io/2017/10/24/image-similarity-measure-image-retrieval/)<br>