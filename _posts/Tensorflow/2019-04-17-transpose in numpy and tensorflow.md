---
title: "Numpy와 Tensorflow에서의 Transpose"
categories: Tensorflow
---

### Numpy에서 matmul 계산
```
>>> a = np.arange(3*5*2*10).reshape((3,5,2,10))
>>> b = np.arange(3*5*2*10).reshape((3,5,2,10))
>>> np.matmul(a,b.T)
File "<stdin>", line 1, in <module>
ValueError: matmul: Input operand 1 has a mismatch in its core dimension 0, with gufunc signature (n?,k),(k,m?)->(n?,m?) (size 5 is different from 10)
```

### tf.matmul(a,b,transpose_b=True) 계산
```
>>> a = np.arange(3*5*2*10).reshape((3,5,2,10))
>>> b = np.arange(3*5*2*10).reshape((3,5,2,10))
>>> a.shape
(3, 5, 2, 10)
>>> b.shape
(3, 5, 2, 10)
>>> tf.matmul(a,b, transpose_b=True)
<tf.Tensor 'MatMul_10:0' shape=(3, 5, 2, 2) dtype=int32>
>>> tf.matmul(a,tf.transpose(b,[0,1,3,2]))
<tf.Tensor 'MatMul_12:0' shape=(3, 5, 2, 2) dtype=int32>
```

위의 Numpy 예시에서 transpose(ex. b.T)는 b의 shape의 전체를 뒤집습니다.
즉, b의 shape는 (10,2,5,3)이 됩니다. 그러므로 matmul 계산이 불가능합니다.

Tensorflow의 경우에는 matmul 함수 안에 transpose_b라는 파라미터가 존재합니다.
해당 파라미터에 True 값을 전달하게 되면 b의 shape는 (3,5,10,2) 형태를 만들어 matmul 계산을 수행합니다

그러면 Tensorflow의 transpose 함수는 어떨까요?
단순히 tf.matmul(a,tf.transpose(b)) 명령어를 사용하면 b의 shape는 (2,10,5,3) 형태로 바뀌기 때문에 계산이 불가능합니다.
tf.transpose로 `transpose_b=True`와 같은 결과 값을 얻기 위해서는 tf.transpose(b,[0,1,3,2])와 같이 직접 지정해주어야 합니다.

### np.transpose 어떤 식으로 값을 바꾸는가?
```
>>> a = np.arange(3*5*2).reshape((3,5,2))
>>> a
array([[[ 0,  1],
        [ 2,  3],
        [ 4,  5],
        [ 6,  7],
        [ 8,  9]],

       [[10, 11],
        [12, 13],
        [14, 15],
        [16, 17],
        [18, 19]],

       [[20, 21],
        [22, 23],
        [24, 25],
        [26, 27],
        [28, 29]]])
>>> np.transpose(a, (2,0,1))
array([[[ 0,  2,  4,  6,  8],
        [10, 12, 14, 16, 18],
        [20, 22, 24, 26, 28]],

       [[ 1,  3,  5,  7,  9],
        [11, 13, 15, 17, 19],
        [21, 23, 25, 27, 29]]])
```

숫자 2의 원래 위치를 보면 (0,1,0)인데 transpose을 사용하여 파라미터로 전달된 내용과 같이 순서를 (0, 0, 1)로 바뀌었습니다.

다른 예시를 보면 숫자 26의 위치는 (2, 3, 0) → (0, 2, 3) 으로 바뀌었습니다.

잘 못된 결과나 설명이 있을 경우 댓글로 알려주시면 바로 수정하겠습니다.