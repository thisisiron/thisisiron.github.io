---
categories: MachineLearning
---

## Logistic Regression이란
Categorical(범주형) 데이터를 이용하여 데이터를 분류하는 알고리즘입니다. 로지스틱 회귀 분석이 사용되는 경우는 **binary classification problem**입니다. 물론 3개 이상 여러 범주도 곧 설명할 기법을 이용해 분류가 가능합니다. 

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

"Non-convex"한 함수, 즉 비볼록함수가 생성됩니다. 이런 함수가 어떤 문제가 발생하냐면 여러 많은 Local Minimum을 갖고 있는 문제가 있습니다. 2차 함수의 가장 마지막 끝 쪽이 최적의 부분이 되어야 하는데 위와 같은 상황에서는 거의 그렇지 못할 상황이 발생하게 됩니다. 그렇기 때문에 Logistic Regression에서는 다른 Cost Function을 사용하게 됩니다.

## Logistic Regression Cost Function
다음은 Logistic Regression Cost Function 식 입니다.
$$
\begin{align*}& J(\theta) = \dfrac{1}{m} \sum_{i=1}^m \mathrm{Cost}(h_\theta(x^{(i)}),y^{(i)}) \newline & \mathrm{Cost}(h_\theta(x),y) = -\log(h_\theta(x)) \; & \text{if y = 1} \newline & \mathrm{Cost}(h_\theta(x),y) = -\log(1-h_\theta(x)) \; & \text{if y = 0}\end{align*}
$$

아래의 그래프를 보면서 설명드리겠습니다. 해당 그래프는 $y=1$ 일 때 그래프입니다. 파란색 선은 $h_{\theta}(x)$입니다. 만약 Classifier(분류기)가 정확하게 예측을 했더라면 $h_{\theta}(x) = 1$ 이고 그래프를 보게 되면 0인 경우입니다. 하지만 반대로 아예 예측을 못한다고 한다면 $h_{\theta}(x)=0$이므로 Cost 값은 $\infty$가 됩니다. 생각해보면 잘 분류했으니 Cost 값이 줄어들게 되는거죠(Error라고 생각하셔도 무방합니다)

<img src="/assets/images/logistic_1.PNG">

다음 아래 그래프는 $y=0$일 때 그래프입니다. 마찬가지로 파란색 선은 $h_{\theta}(x)$입니다. 방금 전의 그래프와는 정반대의 상황입니다. 여기서는 $h_{\theta}(x)=0$ 일 때 Cost 값은 0이게 됩니다. $h_{\theta}(x)=1$ 경우에는 Cost 값은 $\infty$가 됩니다. 따로 따로 그래프를 보여줘서 그렇지만 생긴 것은 2차 함수와 비슷하게 생겼습니다. 즉, Logistic Regression Cost Funtion은 Convex(볼록)함수입니다. 항상 Global minimum 값만 갖게 됩니다. 이 점은 Linear Regression과 같습니다. Local minimum에 빠질 일이 없게 된 겁니다.

<img src="/assets/images/logistic_2.PNG">

처음 보여줬던 식을 통합해보면 다음과 같은 식이 만들어지게 됩니다.

$$J(\theta) = -\frac{1}{m}\sum_{i=1}^{m}[y^{(i)}log(h_{\theta}(x^{(i)}))+(1 - y^{(i)})log(1 - h_{\theta }(x^{(i)}))]$$


## Multiclass Classification
클래스가 3개 있을 경우 분류하는 방법, 혹은 그 이상의 클래스가 있을 경우 분류하는 방법은 one-vs-all 방법을 사용합니다. 즉 Class 1을 분류할 때 Class 2와 Class 3을 **Class 1이 아닌 집단**이라고 새로운 가짜 집합을 생성하는 것입니다. 그렇게 되면 이전에 우리가 배웠던 Binary Classification(이진 분류)이 되었습니다. 세 개의 Logistic Classifier을 학습시켜 각각의 클래스에 대해 $y = i(i=1,2,3)$일 확률을 예측하도록합니다. 다음 그림을 보면 하나의 Class을 학습할 때 다른 집합들은 새로운 하나의 집합으로 O 동그라미로 치환하는 것을 확인할 수 있습니다.
<img src="/assets/images/one_vs_all.PNG">

이제 새로운 Input x가 들어오게 되면 다음과 같은 식을 사용하여 가장 큰 값이 나오는 Class i를 선택하게 됩니다. 

$$\underset{i}{max}h_{\theta}^{(i)}(x)$$

### Reference
Coursera: Machine Learning (Andrew Ng)