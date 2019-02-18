---
title: "Human-level Performance에 대해"
categories: DeepLearning
---

## Human-level Performance란
<img src="/assets/images/human_level.PNG"><br>

위의 이미지를 보면 초록색 선과 파란색 선 기준이 보일 것입니다. 초록색 선부터 설명을 드리자면 **base optimal**(베이지안 최적오차)은 이론상 가능한 최저의 오차입니다.
즉 Overfitting이 발생되지 않는 한 이 값을 넘을 수는 없습니다. 예를 들어 오디오 인식 프로그램에서 잡음이 들어가게 되면 정확도 100% 결과를 내놓을 수 없습니다. 또는 이미지가 너무 흐려서 고양이인지 아닌지 절대 구분하지 못할 수 있습니다. 이 경우에도 100% 정확도를 내놓을 수 없습니다.

파란색 선은 Human-level Performance입니다. 즉 사람의 수준 오차를 의미하게 됩니다.
사람 수준의 오차는 베이지안 오차보다 더 나쁩니다. 베이지안 오차보다 작은 오차는 존재하지 않기 때문입니다.
그렇지만 베이지안 오차와 사람 오차의 차이는 크지 않을 것입니다.

## Human-level을 넘는다면?
대부분의 task에서 Human 성능은 베이지안 최적 오차와 크게 차이나지 않습니다. 즉 Human 성능을 넘게 되면 더 성능을 향상시킬만한 여유가 없을 가능성이 높습니다.
그렇기때문에 Human-level을 넘게 되면 진행속도가 느려지게 됩니다. 이러한 이유들을 정리해보면 다음과 같습니다.

1. Human 성능과 base 성능의 차이가 크지않음
2. Human 성능보다 낮은 경우 특정 방법을 이용해서 쉽게 높일 수 있지만 반대의 경우는 힘듬

## Avoidable Bias
| | Case1 | Case2 |
-------- | ---------------------- | ------------- | 
|Humans | 1% | 7.5% |
|Training Error | 8% | 8% |
|Dev Error | 10% | 10% |

위에 표에서 나오는 Humans은 사람 수준의 오차 혹은 베이지안 오차의 추정치가 됩니다. 아까 앞에서도 말했듯이 사람 수준의 오차와 베이지안 오차는 크게 차이 나지않기 때문에 근사값이라고 추정하여 사용하겠습니다.

위의 표 예시를 통해 설명드리겠습니다. 

- Case1의 경우 Human 성능과 Training Error 차이가 더 크게 발생하고 있습니다. 이런 경우에는 Bias에 초점을 맞춰야합니다.
- Case2의 경우에는 Humans와 Training Error 차이보다 Training Error와 Dev Error가 차이가 더 큰 경우 Vairance에 초점을 맞춰야합니다.

베이지안 오차와 Training Error의 차이를 **Avoidable Bias**라고 합니다. 더 이상 낮아질 수 없는 편향 또는 최소 오차가 존재한다는 뜻입니다. 즉, Avoidable Bias 값이 크다면 모델이 충분히 학습되지 않은 것입니다.

이에 대한 해결 방법은 다음과 같습니다.
1. Bigger Model로 학습
2. 더 좋은 알고리즘 사용(ex. RMSProp, Adam, ...)
3. 더 나은 신경망이나 하이퍼파라미터를 찾음(ex. 활성화 함수 바꾸기, Hidden unit 수 바꾸기, ...)

## Variance
Training Error와 Dev Error 차이는 **Variance**라고 부릅니다.

이에 대한 해결 방법은 다음과 같습니다.
1. More Data
2. Regularization(ex. L2, Dropout, ...)
3. 다양한 신경망 구조나 하이퍼파라미터 찾기

## 어떤 것을 Humans 성능 기준으로 해야할까?
다음과 같은 상황이 있을 경우 어떤 사람을 기준으로 해야할 것인지 의문이 발생할 수 있습니다.

1. 전문가1 Error: 0.3%
2. 전문가2 Error: 0.5%
3. 일반인 Error: 1%

위와 같은 예시가 있는 경우 사람의 성능은 0.3%로 기준을 맞춥니다.

## 결론
학습 알고리즘이 잘 작동하기 위해서는 아래의 두 과정을 거치게 됩니다.
1. 훈련세트에 잘 들어 맞아야합니다. 즉, 회피 가능 편향을 줄이는 것입니다.
2. 개발 및 시험 세트에서도 좋은 성능을 내도록 일반화합니다. 즉, 분산이 낮아야합니다.

### Reference
Coursera: Structuring Machine Learning Projects (Andrew Ng)<br>
[Eduwith 정리 자료와 이미지](https://www.edwith.org/deeplearningai3)