---
title: "kaggle image retrieval"
categories: MachineLearning
---

Kaggle 대회 중 하나인 [Google Landmark Retrieval Challenge](https://www.kaggle.com/c/landmark-retrieval-challenge/overview)에서 1등한 내용을 번역, 요약한 내용입니다.

--------------------------------------------------------

<img src="/assets/images/kaggle_google_landmark.PNG"><br>
해당 대회에서 사용한 모델의 구성도는 위와 같습니다. pre-trained CNN 모델인 ResNet과 ResNeXt을 사용했습니다. 최신 aggregation method을 사용했습니다. 

## Global Descriptors
위 모델에서 사용한 각각의 방법에 대해 알아보겠습니다. 가장 오른쪽의 수치는 "raw" performance을 의미합니다.

- **Region-Entropy based Multi-layer Abstraction Pooling (REMAP) [42.8% mAP]:** 서로 다른 CNN에서 deep features을 통합하는 global descriptor입니다.

- **Maximum Activations of Convolutions (MAC) [32.9% mAP]:** MAC은 Convolutional filter에 각 가장 마지막 층의 Maximum Local Response을 인코딩합니다. MAC 아키텍처에서 ResNeXt 마지막 층 다음에는 Max-pooloing, L2-Normalization, PCA + Whitening 층이 존재합니다.

- **Sum-pooling of Convolutions (SPoC) [31.7% mAP]:** ResNeXt의 마지막 층에는 Sum pooling, L2-Normalization, PCA + Whitening 층이 층이 뒤따릅니다.

- **Regional Maximum Activations of Convolutions (RMAC) [34.7% mAP]:** ResNeXt의 마지막 층 기능은 여러 개의 multi-scale 중복 지역에 걸쳐 최대 풀링됩니다. 영역 기반 descriptors는 L2-normalized, PCA+Whitened 그리고 다시 L2-normalized입니다. 마지막으로, descriptors는 single descriptors로 합산(Sum)됩니다.

이 대회에서 제공하는 이미지는 검색을 통해 검증되지 않은 이미지들을 수집되었기 때문에 해당 팀은 Cleaning Process을 통해 650개 랜드마크 하나당 약 25,000 이미지를 갖도록 하였습니다. 이 절차는 semi-automatic이며, Hessian-affine detector로 검출하고 [RVD-W descriptor](http://epubs.surrey.ac.uk/812468/)에 통합되는 SIFT feature에 의존하고 있습니다.

## Combined Descriptor
6개의 fine-tuned global descriptors을 연결함으로써 모델을 구축하였습니다.

- ResNeXt+REMAP (42.8%)
- ResNeXt+RMAC (34.7%)
- ResNeXt+MAC (32.9%)
- ResNeXt+SPoC (31.7%)
- ResNet+REMAP (35.8%)
- ResNet+MAC (30.4%)

고정된 L2 Norm으로 Scaling함으로써 가중치를 할당합니다. 식은 다음과 같습니다.

```
XG = [2× ResNeXt+REMAP;  1.5× ResNeXt+RMAC; 1.5× ResNeXt+MAC; 1.5× ResNeXt+SPoC; ResNet+MAC; ResNet+REMAP]
```

가중치는 각각의 방법의 상대적인 성능을 반영하기 위해 선택됩니다. 이후에는 PCA을 수행하여 차원을 줄였습니다. PCA을 사용한 이유는
단순히 계산을 절약하는 것뿐만 아니라 noisy한 차원들을 줄이기 위함입니다. 그리고 모든 차원들이 동일한 variance을 갖도록 Whitening을 적용합니다.
PCA와 Whitening으 약간의 개선을 하였지만 쿼리 확장 단계의 결과를 몇 %을 향상시켰습니다.

## Nearest Neighbour Search
descriptor 생성 후 각각의 이미지는 4096 차원 descriptor로 표현됩니다. 다음으로 k-nearest neighbour search을 이용하게 됩니다. 목적은 가장 가까운 2500 neighbous와 L2 거리를 찾기위해 수행합니다.

## Database augmentation
다음 단계로 database-side augmention을 수행했습니다. 이것은 모든 이미지 descriptor을 가중치 조합으로 대체합니다.
목표는 neighbors의 features을 이용하여 이미지 표현의 질을 향상시키는 것입니다. 조금더 정확하게는 descriptor의 weighted sum-aggregation 수행하는 것입니다.

```
weights = logspace(0, -1.5, 10)
```

흥미롭게도 다른 데이터 셋에서 query을 확장시키기 위해 두 개 이상의 neighbors을 이용하는 것은 점수에 해롭습니다. 10개의 neighbors이 최적이라는 것을 발견했습니다. DBA 자체로 상당한 개선을 제공했지만 query expansion step과 결합될 때, 그 성능 향상은 단지 1~2%였습니다. DB expansion이 query expansion 방법의 첫 번째 수준과 유사하게 작용했기 때문이라고 생각됩니다.

## Query Expansion
Query Expansion은 image retrieval problem의 기본적인 기술입니다. 상당한 성능 향상으로 연결지을 수 있습니다. 이 장점이 설명되는 간단한 예시는 중복되는 세 개의 이미지가 있는 경우입니다.

<img src="/assets/images/query_expansion.jpg"><br>

이 경우에 query expansion는 우리가 A와 C 이미지를 매치시킬 수 있도록 도와줍니다. 이것은 우리에게 서서히 다른 관점을 가지거나 조명에 따라 달라지는 경우에도 도움을 주게 됩니다. 중간 이미지가 그들을 연결시킬 수 있도록 도와줍니다.


## Things that didn’t work
- **Local descriptors:**

### Reference
[1st Place Solution Summary: CVSSP & Visual Atoms ](https://www.kaggle.com/c/landmark-retrieval-challenge/discussion/57855)<br>