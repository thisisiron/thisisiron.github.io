## ROC(Receiver Operating Characteristic)

![](https://keep.google.com/u/0/media/v2/1sAEEuaY09h79A3xi4GWXuX3vvPIZP5RBGlwB2zB4JK38c8EXO_GnGvfB8Qlp7Pc/1CTCG0q_dAxAJjaqIAMRnSAcwo5gOPLRXTDcrcvRT2EAJxNZSCFc4fpn3VUUywt8?accept=image/gif,image/jpeg,image/jpg,image/png,image/webp,audio/aac&sz=395)  
- 민감도와 특이도가 어떤 관계를 갖고 있는지를 표현한 그래프  

- Classification의 성능을 평가에 사용되는 그래프x축(1-특이도), y축(민감도)



## AUC(Area Under Curve)

![](https://keep.google.com/u/0/media/v2/1HdvQbc3UlJRlqw-vdTAoJuAcVFM6a8VHJ4hrhPjEjb4ipTa3C1U3dn4232BJB4g/1xeYvjXAnc8rr7YlD9HLIdKIXeSLHKCxrKzdvZpRN5XwTQsO8nDf15zBH44wIaw?accept=image/gif,image/jpeg,image/jpg,image/png,image/webp,audio/aac&sz=405)  
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
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg4NzA2MTU1OCwtMTQwMDcwODI2MF19
-->