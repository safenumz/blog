---
layout: post
title: '[Python] numpy'
category: Python
tags: [python, numpy]
comments: true
---

# array
## 차원과 모양
- ndim : 행렬의 차원
- shape : 행렬의 모양

```python
>>> array = np.array(
    [[[1, 2, 3],
    [4, 5, 6]],
    [[7, 8, 9],
    [10, 11, 12]]],
    )
>>> print(array.ndim, array.shape)
3 (2, 2, 3)
```
## reshape

```python
>>> na = np.array(range(10))
>>> na.reshape(2, 5)
>>> np.reshape(na, (2, 5))
array([[0, 1, 2, 3, 4],
       [5, 6, 7, 8, 9]])
```

## zeros
- 행렬을 만들고 0을 채움

```python
>>> z = np.zeros(2, 3, 2), dtype = int)
>>> z, z.dim, z.shape
```

## ones
- 행렬을 만들고 1을 채움

```python
>>> one = np.ones((4, 3), dtype = int)
>>> one
array([[1, 1, 1],
       [1, 1, 1],
       [1, 1, 1],
       [1, 1, 1]])
```

## eye
- 단위행렬을 만들때 사용

```python
>>> np.eye(5)
array([[1., 0., 0., 0., 0.],
       [0., 1., 0., 0., 0.],
       [0., 0., 1., 0., 0.],
       [0., 0., 0., 1., 0.],
       [0., 0., 0., 0., 1.]])
```

## like
- 행렬안에 있는 모든 숫자를 0 또는 1로 변경

```python
>>> np.ones_like(z)
>>> np.zeros_like(one)
```
