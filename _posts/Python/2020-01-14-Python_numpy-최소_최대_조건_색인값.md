---
layout: post
title: '[Python numpy] 최소, 최대, 조건 색인값 (np.argmin, np.argmax, np.where)'
category: Python
tags: [python, numpy]
comments: true
---

# numpy 최소, 최대, 조건 색인값

## 최소값(min), 최대값(max): np.min(), np.max()

~~~python
import numpy as np
x = np.array([5, 4, 3, 2, 1, 0])
print(x.min())
print(np.min(x))
print(x.max())
print(np.max(x))
~~~

<pre>
0
0
5
5
</pre>

## 최소값, 최대값의 색인 위치:np.argmin(), np.argmax()

~~~python
print(x.argmin())
print(np.argmin(x))
print(x.argmax())
print(np.argmax(x))
~~~

<pre>
5
5
0
0
</pre>

## 조건에 맞는 값의 색인 위치: np.where()
- 배열에서 3과 같거나 큰 값을 가지는 색인의 위치를 알고 싶을 때

~~~python
np.where(x >= 3)
~~~

<pre style="background:transparent">
(array([0, 1, 2], dtype=int64),)
</pre>

## 조건에 맞는 값을 indexing 하기: x[np.where()]
- 배열에서 3과 같거나 큰 값을 indexing 하고 싶을 때

~~~python
x[np.where(x >= 3)]
~~~

<pre style="background:transparent">
array([5, 4, 3])
</pre>

## 조건에 맞는 값을 특정 다른 값으로 변환하기: np.where(조건, 조건이 맞을 때 값, 조건과 다를 때 값)
- 배열의 값이 3과 같거나 크면 3으로 변환하고 작으면 그대로 값을 유지하고 싶을 때
- for loop를 사용하는 것보다 더 빠르다

~~~python
np.where(x >= 3, 3, x)
~~~