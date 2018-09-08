## Confusion Matrix
![confusion matrix](https://github.com/thisisiron/blogger/blob/master/images/confusion_matrix.PNG)
- True Positive: 실제 True인 것을 True라고 예측
- False Positive: 실제 False인 것을 True라고 예측
- False Negative: 실제 True인 것을 False라고 예측
- True Negative: 실제 False인 것을 False라고 예측

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
