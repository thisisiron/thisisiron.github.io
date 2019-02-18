---
title: "np.asarray vs np.array"
categories: Numpy
---

## np.asarray와 np.array 차이점
array 함수와 asarray함수의 차이점은 복사(copy)에 있다. 두 함수 파라미터는 다음과 같다.

```
numpy.asarray(a, dtype=None, order=None)
```

```
numpy.array(object, dtype=None, copy=True, order='K', subok=False, ndmin=0)
```

위의 코드를 보게 되면 copy의 초기값이 다르다는 것이다. 즉, asarray는 전달받은 배열을 참조하게 되는 것이고 array는 복사본을 새로 생성하게 된다.

이런 상황을 다음 코드로 확인하겠다.


```
arr = np.zeros((2,3))
array([[0., 0., 0.],
       [0., 0., 0.]])

arr_as = np.asarray(arr)
arr_ar = np.array(arr)

arr[0] = np.ones((1,))

arr
array([[1., 1., 1.],
       [0., 0., 0.]])

arr_as
array([[1., 1., 1.],
       [0., 0., 0.]])

arr_ar
array([[0., 0., 0.],
       [0., 0., 0.]])

```



### Reference
[numpy.asarray](https://docs.scipy.org/doc/numpy/reference/generated/numpy.asarray.html)<br>
[numpy.array](https://docs.scipy.org/doc/numpy/reference/generated/numpy.array.html)
