---
categories: MachineLearning
---
# Scaling vs. Nomalization 두 개의 차이점은 무엇일까?
Scaling과 Nomalization이 혼동되는 이유는 때때로 두 용어를 서로 바꾸어서 사용되기 때문입니다.

이 두 개의 용어는 서로 매우 유사합니다.

두 경우 모두 특정 유용한 특성을 갖도록 숫자 변수의 값을 변환시킵니다.

**차이점**은 Scaling은 데이터 범위를 변경하는 것이고 Nomalization은 데이터 분포 모양을 변경한다는 점입니다.

## Scaling
특정 척도에 맞도록 데이터를 변환하는 것입니다.

SVM 또는 KNN과 같은 데이터 지점 간의 차이를 측정하는 방법을 사용할 때 사용된다.

예를 들어, 일부 제품의 가격을 엔화와 달러로 볼 수 있습니다. 1달러는 약 100엔이지만 SVM이나 KNN과 같이 가격을 조정하지 않으면 1엔의 가격 차이를 1달러 차이만큼 중요하지 않게 생각할 것입니다.

![Scaling](https://www.kaggleusercontent.com/kf/3013017/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..L9_EltS3TgMI9yGBB6tJXQ.1jFqRWoFyr0UBezhSzzpiiJKX2Oj3Hj3Peo7VCiy27C1-WSh2CjIfy6hG8w12-Isv9vhxy-_Hk3rEFdE6chMDoJJ4MG-6sXCf4cyzweE2YLBjfsDbrEu9u3xzsPOAsFlE6F-aSLfjC4frWcjoqSBKVm5EXLNmMg8WhR5btEwop8fsWf3tX7NrCMrj6oKu9yQ.nn-52juOBBEygbAX5c0VhQ/__results___files/__results___5_1.png)


## Normalization
Scaling은 단지 우리의 데이터 범위가 변경됩니다.

Nomalization은 조금 더 근본적인 변화를 줍니다. Normalization의 목적은 관측치를 정규 분포로 설명할 수 있도록 변경하는 것입니다.

일반적인 경우, 기계학습 또는 통계 기법을 사용하려는 경우에만 데이터를 Normalization한다.

![Normalization](https://www.kaggleusercontent.com/kf/3013017/eyJhbGciOiJkaXIiLCJlbmMiOiJBMTI4Q0JDLUhTMjU2In0..L9_EltS3TgMI9yGBB6tJXQ.1jFqRWoFyr0UBezhSzzpiiJKX2Oj3Hj3Peo7VCiy27C1-WSh2CjIfy6hG8w12-Isv9vhxy-_Hk3rEFdE6chMDoJJ4MG-6sXCf4cyzweE2YLBjfsDbrEu9u3xzsPOAsFlE6F-aSLfjC4frWcjoqSBKVm5EXLNmMg8WhR5btEwop8fsWf3tX7NrCMrj6oKu9yQ.nn-52juOBBEygbAX5c0VhQ/__results___files/__results___7_1.png)


### Reference
https://www.kaggle.com/rtatman/data-cleaning-challenge-scale-and-normalize-data
