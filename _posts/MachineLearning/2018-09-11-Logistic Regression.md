---
categories: MachineLearning
---

로지스틱 회귀 분석이 사용되는 예
종속 변수의 값을 0 또는 1로 표현할 수 있는 경우

## 선형회귀(Linear Regression)은 왜 사용할 수 없을까?
<img src="/assets/images/unusing_linear_regression.jpg">

위와 같은 그림에서 두 번쨰의 경우 문제가 발생하게 됩니다. No인 부분에 암 판정이 Yes인 부분이 포함되기 때문입니다. 그래서 Logistic Regression이란 분류 알고리즘을 적용하게 되었습니다.

## Logistic Regression 공식
<img src="/assets/images/sigmoid.png">

Sigmoid Function을 사용하게 되면 0과 1사이의 결과가 나옵니다. 다음에는 Sigmoid 함수 식을 보겠습니다. $\Theta$는 가중치, $x$는 입력값일 경우 2 번째 $g$함수에 파라미터로 전달하게 됩니다.
$$h(x) = g(\Theta ^T x )$$

$$ g(z) = \frac{1}{1+e^{-z}} $$

## Logistic Regression에서 문제
Linear Regression에서 사용하던 Cost Function을 사용할 수 없습니다. 왜냐하면 Linear Function에서 사용하던 Cost Function을 Logistic Regression에 적용하게 되면 다음과 같은 상황이 발생하게 됩니다.
<img src="/assets/images/logistice_problem.jpg">

"Non-convex"한 함수, 즉 비블록함수가 생성됩니다. 이런 함수가 어떤 문제가 발생하냐면 여러 많은 Local Minimum을 갖고 있는 문제가 있습니다. 2차 함수의 가장 마지막 끝 쪽이 최적의 부분이 되어야 하는데 위와 같은 상황에서는 거의 그렇지 못할 상황이 발생하게 됩니다. 그렇기 때문에 Logistic Regression에서는 다른 Cost Function을 사용하게 됩니다.

## Logistic Regression Cost Function
$$
\begin{align*}& J(\theta) = \dfrac{1}{m} \sum_{i=1}^m \mathrm{Cost}(h_\theta(x^{(i)}),y^{(i)}) \newline & \mathrm{Cost}(h_\theta(x),y) = -\log(h_\theta(x)) \; & \text{if y = 1} \newline & \mathrm{Cost}(h_\theta(x),y) = -\log(1-h_\theta(x)) \; & \text{if y = 0}\end{align*}
$$

## Multiclass Classification
클래스가 3개 있을 경우 분류하는 방법, 혹은 그 이상의 클래스가 있을 경우 분류하는 방법은 one-vs-all 방법을 사용합니다. 즉 Class 1을 분류할 때 Class 2와 Class 3을 **Class 1이 아닌 집단**이라고 새로운 가짜 집합을 생성하는 것입니다. 그렇게 되면 이전에 우리가 배웠던 Binary Classification(이진 분류)이 되었습니다. 세 개의 Logistic Classifier을 학습시켜 각각의 클래스에 대해 $y = i(i=1,2,3)$일 확률을 예측하도록합니다. 다음 그림을 보면 하나의 Class을 학습할 때 다른 집합들은 새로운 하나의 집합으로 O 동그라미로 치환하는 것을 확인할 수 있습니다.
<img src="/assets/images/one_vs_all.PNG">

이제 새로운 Input x가 들어오게 되면 다음과 같은 식을 사용하여 가장 큰 값이 나오는 Class i를 선택하게 됩니다. 

$$max_{i} h_{/theta}^{(i)}(x)$$

### Reference
Coursera: Machine Learning (Andrew Ng)