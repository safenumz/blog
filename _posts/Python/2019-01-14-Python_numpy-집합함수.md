---
layout: post
title: '[Python numpy] set functions'
category: Python
tags: [python, numpy]
comments: true
---

#  numpy set functions
- unique(x): 배열 내 중복된 원소 제거 후 유일한 원소를 정렬하여 반환
- intersect1d(x, y): 두 개의 배열 x, y의 교집합을 정렬하여 반환
- union1d(x, y): 두 개의 배열 x, y의 합집합을 정렬하여 반환
- in1d(x, y): 첫번째 배열 x가 두번째 배열 y의 원소를 포함하고 있는지 여부의 불리언 배열을 반환
- setdiff1d(x, y): 첫번째 배열 x로부터 두번째 배열 y를 뺀 차집합을 반환
- setxor1d(x, y): 두 배열 x, y의 합집합으로부터 교집합을 뺀 대칭차집합을 반환


## np.unique(x)
- 배열 내 중복된 원소 제거 후 유일한 원소를 정렬하여 반환

~~~python
import numpy as np
x = np.array([1, 2, 3, 1, 2, 4])

# sorted(set(x))과 동일한 연산
np.unique(x)
~~~

<pre>
array([1, 2, 3, 4])
</pre>

## np.interset1d(x, y)
- 두 개의 배열 x, y의 교집합을 정렬하여 반환

~~~python
x = np.array([1, 2, 3, 4])
y = np.array([3, 4, 6, 5])

np.interset1d(x, y)
~~~

<pre>
array([3, 4])
</pre>

## np.union1d(x, y)
- 두 개의 배열 x, y의 합집합을 정렬하여 반환

~~~python
x = array([1, 2, 3, 4])
y = array([3, 4, 6, 5])

np.union1d(x, y)
~~~

<pre>
array([1, 2, 3, 4, 5, 6])
</pre>

## np.in1d(x, y)
- 첫 번째 배열이 두번째 배열의 원소를 포함하고 있는지 여부의 불리언 배열을 반환

~~~python
x = np.array([1, 2, 3, 4, 5, 6])
y = np.array([2, 4])

np.in1d(x, y)
~~~

<pre>
array([False, True, False, True, False, False])
</pre>

## np.setdiff1d(x, y)
- 첫번째 배열 x로부터 두번째 배열 y를 뺀 차집합을 반환

~~~python
x = np.array([1, 2, 3, 4])
y = np.array([3, 4, 5, 6])

np.setdiff1d(x, y)
~~~

<pre>
array([1, 2])
</pre>

## np.setxor1d(x, y)
- 두 배열 x, y의 합집합에서 교집합을 뺀 대칭차집합을 반환

~~~python
x = np.array([1, 2, 3, 4])
y = np.array([3, 4, 5, 6])

np.setxor1d(x, y)
~~~

<pre>
array([1, 2, 5, 6])
</pre>




