---
layout: post
title: '[Python pandas] isin'
category: Python
tags: [python, pandas]
comments: true
---

# pandas isin
- isin 구문은 열이 list의 값을 포함하고 있는 모든 행들을 골라낼 때 주로 쓰인다.

~~~python
df = DataFrame({'A': [1, 2, 3], 'B': ['a', 'b', 'f']})
df.isin([1, 3, 12, 'a'])
~~~

<pre>
	A	B
0	True	True
1	False	False
2	True	False
</pre>

- A 칼럼이 1, 3, 12, 'a'를 포함하는 것만 골라 냄

~~~python
df[df['A'].isin([1, 3, 12, 'a'])]
~~~


<pre>
	A	B
0	1	a
2	3	f
</pre>