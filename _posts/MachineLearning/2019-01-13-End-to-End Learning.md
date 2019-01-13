---
title: "End-to-End Learning이란"
categories: MachineLearning
---

## End-to-End Learning는 무엇일까?
자동처리 시스템 / 학습 시스템에서 여러 단계가 필요한 처리 과정을 한 번에 처리하는 것을 말합니다. 데이터를 입력하고 목적을 학습시키는 것이라고 이해하시면 됩니다. 예를 들어 설명하겠습니다.
```
Ex. Speech Recongnition 시스템 개발
1. 기존의 모델
   - Audio -> Feature Extraction -> Phonemes -> Words -> Transcript
2. End-to-End 모델
   - Audio -----------------------------------------> Transcript
```

기존의 모델의 경우에는 음성에서 Feature을 추출하여 머신러닝 알고리즘을 이용해 음소를 알아내고 이 음소들을 이용해 단어를 만들고 Transcript을 만들게 됩니다. 하지만 End-to-End에서는 이런 과정을 생략하고 처리하는 것입니다. Audio만 있으면 한 번에 Transcript을 얻을 수 있습니다. 이렇게 순수 End-to-End말고 Audio에서 Phonemes 단계로 바로 넘어가는 경우처럼 기존의 몇 단계는 넘어가고 어떤 단계는 사용하는 것이 가능합니다. 그러나 이러한 End-to-End Learning을 하기 위해서는 많은 데이터가 필요하게 됩니다. 

## End-to-End Learning 무조건 좋은 것일까?
꼭 그렇지만 않습니다. 단계를 나눠서 학습하는 것이 효율적일 때가 있습니다. 효율적인 경우는 다음과 같습니다.
1. 복잡한 문제를 분리하여 간단한 문제로 변환
2. 데이터의 정보가 각각의 작업에 더 적합하게 사용

실제로, **순수 End-to-End보다 문제를 쪼개서 학습하는 것이 효과적이며 좋습니다.** 상황에 따라서 어떤 곳에는 End-to-End를 사용하고 다른 어떤 곳에는 단계별로 나아가야 합니다.

## 장점
- 데이터로 하여금 말을 하도록 합니다. 즉, 사람의 선입견을 덜 받도록 합니다.
- 중간 설계 요소를 줄일 수 있습니다.

## 단점
- 방대한 데이터가 필요합니다.
- 잠재적으로 유용하고 사람의 손으로 만들어진 중간요소를 완전 배제합니다. 데이터가 적을 때 효율적인 특정 지식을 사용할 수 없습니다.

### Reference
Coursera: Structuring Machine Learning Projects (Andrew Ng)