---
title: "argmax 함수와 max함수 차이"
categories: Tensorflow
---

## tf.keras.backend.max와 tf.keras.backend.argmax 차이
tf.keras.backend.max와 tf.keras.backend.argmax는 numpy에서 함수와 동일하게 작동하므로 다음과 같은 코드를 보고 설명하겠습니다.

```
x = np.random.randn(5)
print(x)
print(np.argmax(x))
print(np.max(x))

[-0.73370703  0.98163748 -0.23885241 -0.26628727  1.00499406]
4
1.0049940634
```

- max: 해당 배열 안에 존재하는 가장 큰 값을 Return
- argmax: 가장 큰 값의 index을 Return

### Reference
[tf.keras.backend.max](https://www.tensorflow.org/api_docs/python/tf/keras/backend/max)<br>
[tf.keras.backend.argmax](https://www.tensorflow.org/api_docs/python/tf/keras/backend/argmax)<br>
[What is the difference between keras.backend.max vs keras.backend.argmax?](https://stackoverflow.com/questions/52878998/what-is-the-difference-between-keras-backend-max-vs-keras-backend-argmax)