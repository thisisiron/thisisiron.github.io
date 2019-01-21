---
title: "Debugging과 Tips"
categories: MachineLearning
---
## Debugging a Learning Algorithm
### Large error가 발생했을 때 무엇을 해야할까?
선택 사항 | 선택 기준 | 
------------ | -------- | 
Training Data을 더 모은다 | Fix high variance
Feature의 크기를 줄여본다 | Fix high variance
추가적으로 Feature 넣어본다 | Fix high bias
Polynomial Feature을 추가한다 ($x_{1}^{2},x_{2}^{2},x_{1}x_{2},etc$) | Fix high bias
Decreasing $\lambda$ | Fix high bias
Increasing $\lambda$ | Fix high variance

위 선택사항을 무작위로 선택하는 것과 같은 시간을 낭비해서는 안 됩니다. 
오른쪽 기준에 맞춰서 올바르게 선택을 해야 시간을 절약할 수 있습니다.

## Model Selection
### 여러 모델 중 어떤 모델을 선택해야할까?
$$
\begin{align*}&
d=1, \enspace h_{\theta}(x) = \theta_{0} + \theta_{1}x  \newline &
d=2, \enspace h_{\theta}(x) = \theta_{0} + \theta_{1}x + \theta_{2}x^2 \newline &
d=10, \enspace h_{\theta}(x) = \theta_{0} + \theta_{1}x + ... +  \theta_{10}x^10
\end{align*}
$$

위와 같이 각각 Polynomial 식이 존재합니다. 즉 여러 모델이 존재한다고 생각해주시면 됩니다.
알고자 하는 것은 이 중에서 좋은 모델을 선택하기 위해서 Test Set을 사용해서는 안 됩니다.
그 선택된 Model은 그 Test에 맞춰져 있기 때문입니다. Test Set에서 d=5일 때 가장 좋은 결과를 냈다고 해서 그 모델을
선택하게 되면 그 모델(d=5)은 단지 Test Set에 좋은 결과를 낸 것입니다.

<!-- <img src="/assets/images/multiple_filters.PNG"> -->

### Reference
Coursera: Machine Learning (Andrew Ng)