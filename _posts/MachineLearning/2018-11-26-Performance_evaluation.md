---
categories: Machine Learning
---
## Confusion Matrix
![confusion matrix](/assets/images/confusion_matrix.PNG)
- True Positive: 실제 True인 것을 True라고 예측
- False Positive: 실제 False인 것을 True라고 예측
- False Negative: 실제 True인 것을 False라고 예측
- True Negative: 실제 False인 것을 False라고 예측

**Example**
![](https://t1.daumcdn.net/cfile/tistory/99EFFA3359E629C82B)
이미지 출처: http://bcho.tistory.com/1206

### Accuracy
대표적으로 사용되는 지표로, 전체 데이터 중에서 제대로 분류된 데이터 비율을 의미

P와 N은 positive으로 관측된 데이터, negative으로 관측된 데이터를 의미하며 P+N은 전체데이터를 의미

![](https://latex.codecogs.com/gif.latex?ACC%20%3D%20%5Cfrac%7B%28TP&plus;TN%29%7D%7BP%20&plus;%20N%7D)

### Error Rate
Accuracy의 반대의 의미로 전체 데이터 중에서 잘 못 판단한 비율을 의미

![](https://latex.codecogs.com/gif.latex?ERR%20%3D%20%5Cfrac%7B%28FP&plus;FN%29%7D%7BP%20&plus;%20N%7D)

### Sensitivity (Recall or True positive Rate)
민감도라고 하며, positive 데이터 중에서 우리가 positive으로 분류한 데이터 비율을 의미

![](https://latex.codecogs.com/gif.latex?Recall%20%3D%20%5Cfrac%7BTP%7D%7BTP%20&plus;%20FN%7D)

### Precision
positive라고 분류한 데이터 중에서 실제 positive 비율을 의미

![](https://latex.codecogs.com/gif.latex?Precision%20%3D%20%5Cfrac%7BTP%7D%7BTP%20&plus;%20FP%7D)

### Specificity
특이성이라고도 하며, negative라고 분류한 데이터 중에 실제 negative 값의 비율

![](https://latex.codecogs.com/gif.latex?Specificity%20%3D%20%5Cfrac%7BTN%7D%7BTN%20&plus;%20FP%7D)

### False Positive rate
실제 negative인데 positive로 분류, 즉 정답이 아닌 것을 정답으로 분류한 비율

![](https://latex.codecogs.com/gif.latex?FPR%20%3D%20%5Cfrac%7BFP%7D%7BFP%20&plus;%20TN%7D)

### False Negative Rate
실제 positive인데 negative로 분류, 즉 정답을 정답이 아닌 것으로 분류한 비율

![](https://latex.codecogs.com/gif.latex?FNR%20%3D%20%5Cfrac%7BFN%7D%7BFN%20&plus;%20TP%7D)

### Trading Off Precision and Recall
Cancer 분류를 예시로 든다면, (Logistic Regression 0 <= h(x) <= 1)
```
Predict 1 h(x) >= 0.5
Predict 0 h(x) < 0.5
```
Cancer인지 아닌지 확실히 판단하는 것이 중요할 경우 (환자에게 Cancer이라고 알려준다면 충격받는 것을 고려)
-> h(x) 범위를 수정한다. Threshold(0.5보다 큰 0.7, 0.9 값으로)을 높게 잡도록한다.
-> 이럴 경우 Higher Precision, Lower Recall이 된다.

Cancer 경우를 놓치고 싶지 않다면 (환자가 미리 Cancer에 대해 대비하도록 하는 것을 고려)
-> h(x) 범위를 수정한다. Threshold(0.5보다 작은 0.3, 0.1 값으로)을 낮게 잡도록한다.
-> 이럴 경우 Lower Precision, Higher Recall이 된다.


### F1 Score

| Type | Precision(P) | Recall(R) | Averge | F1 Score |
|---|----|----|----|----|
|algo.1 | 0.5 | 0.4 | 0.45 | 0.444 |
|algo.2 | 0.7 | 0.1 | 0.4 | 0.175 |
|algo.3 | 0.02 | 1.0 | 0.51 | 0.0392 |

Algo.3에서 P가 1이라면 모든 경우에서 y=1이라고 예측하고 있는 것이다.
하지만 Average 값을 보게 되면 값이 높게 측정되어 있다.
Average로 평가하는 것은 좋은 방법이 아니다.

- Average
![](https://latex.codecogs.com/gif.latex?%5Cfrac%7BP&plus;R%7D%7B2%7D)

- F1 Score (F score)
![](https://latex.codecogs.com/gif.latex?2%20*%20%5Cfrac%7BPR%7D%7BP&plus;R%7D)

F1가 크다면 P와 R도 클 것이다.
P = 0 or R = 0 -> Fscore = 0
P = 1 and R = 1 -> Fscore = 1


## ROC(Receiver Operating Characteristic)

![](https://github.com/thisisiron/blogger/blob/master/images/ROC.png)  
x축(1-특이도), y축(민감도)
- 민감도와 특이도가 어떤 관계를 갖고 있는지를 표현한 그래프  
- Classification의 성능을 평가에 사용되는 그래프
- True Positive Rate(Sensitivity)와 False Positive Rate(1-Specifity)로 구성
- y=x 보다 높으면 좋다고 판정, 낮으면 좋지 않은 것으로 판정


## AUC(Area Under Curve)

![](https://github.com/thisisiron/blogger/blob/master/images/AUC.png)  
ROC curve 아래 면적을 구한 값을 일컫는 용어.

## 5-fold Cross Validation

Order | 1 | 2 | 3 | 4 | 5
----- | ----- | ----- | ----- | ----- | -----
1 fold | test | train | train | train | train
2 fold | train | test | train | train | train
3 fold | train | train | test | train | train
4 fold | train | train | train | test | train
5 fold | train | train | train | train | test

위의 표를 보면 data set을 5개로 나누어서 첫 번째 경우 첫 번째 Test set으로 나머지 네 개 data set은 Train set으로 나누어서 성능을 확인하는 방법이다.
이런 식으로 5 번 반복을 하며 5개의 성능을 비교 분석한다.
