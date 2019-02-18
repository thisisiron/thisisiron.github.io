---
title: "local optima 간단한 정리"
categories: DeepLearning
---

고차원 비용함수에서 경사가 0인 경우 대부분 Local Optima(지역 최적값)가 아니라 Saddle Point(안장점)입니다. 

Saddle Point란 **미분 값이 아주 오래 동안 0에 가깝게 유지되는 지역**을 말합니다. 

<img src="/assets/images/saddle_point.PNG"><br>

위 그림에서 하늘색 점이 Saddle Point입니다. 이 지점의 경사는 0이게 됩니다. 이러한 이름으로 부르게 된 이유는 말의 안장과 비슷하게 생겼기 때문입니다.

<img src="/assets/images/plateaus.PNG"><br>

위의 그림에서 하늘색 점 이동하는 경로가 점점 경사가 0이거나 0에 가까운 값들이기 때문에 오랜 시간이 걸려 안장점에 도착하게 됩니다. 그 후 왼쪽이나 오른쪽에 무작위로 변화를 주게되면 안정지대를 벗어나게 됩니다.

- 충분히 큰 신경망을 학습시킨다면 지역최적값에 갇힐 일이 잘 없다는 점
- 안정지대 문제: 경사가 거의 0이기 때문에 학습속도가 매우 느려지고 다른 쪽으로 방향 전환이 없다면 안정지대에서 벗어나기 힘듬<br>
  -> Adam, RMSProp 등 알고리즘으로 해결

### Reference
Coursera: Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization (Andrew Ng)<br>
[Eduwith 정리 자료와 이미지](https://www.edwith.org/deeplearningai2)