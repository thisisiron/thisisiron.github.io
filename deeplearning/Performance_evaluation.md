## Confusion Matrix
![confusion matrix](https://github.com/thisisiron/blogger/blob/master/images/confusion_matrix.PNG)
- True Positive: 실제 True인 것을 True라고 예측
- False Positive: 실제 False인 것을 True라고 예측
- False Negative: 실제 True인 것을 False라고 예측
- True Negative: 실제 False인 것을 False라고 예측


![TPR](https://github.com/thisisiron/blogger/blob/master/images/TPR.PNG)
![FPR](https://github.com/thisisiron/blogger/blob/master/images/FPR.PNG)


## ROC(Receiver Operating Characteristic)

![](https://github.com/thisisiron/blogger/blob/master/images/ROC.png)  
x축(1-특이도), y축(민감도)
- 민감도와 특이도가 어떤 관계를 갖고 있는지를 표현한 그래프  
- Classification의 성능을 평가에 사용되는 그래프
- True Positive Rate(Sensitivity)와 False Positive Rate(1-Specifity)로 구성


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
