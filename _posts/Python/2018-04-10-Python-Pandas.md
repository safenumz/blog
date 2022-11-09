---
layout: post
title: '[Python] pandas'
category: Python
tags: [python, pandas]
comments: true
---

# pandas
## crosstab
crosstab으로 교차표를 쉽게 만들 수 있다.

```python
import pandas as pd
import seaborn as sns

tips = sns.load_dataset('tips')
tips.tail()

# total_bill	tip	sex	smoker	day	time	size
# 239	29.03	5.92	Male	No	Sat	Dinner	3
# 240	27.18	2.00	Female	Yes	Sat	Dinner	2
# 241	22.67	2.00	Male	Yes	Sat	Dinner	2
# 242	17.82	1.75	Male	No	Sat	Dinner	2
# 243	18.78	3.00	Female	No	Thur	Dinner	2

pd.crosstab(tips['sex'], tips['smoker'])
# smoker	Yes	No
# sex		
# Male  	60	97
# Female	33	54
```

교차표에서 행 합, 열 합을 추가할 수도 있다.

```python
pd.crosstab(tips['sex'], tips['smoker'], margins=True)
# smoker	Yes	No	All
# sex			
# Male	  60	97	157
# Female	33	54	87
# All   	93	151	244
```

구성비율 기준으로 교차표를 볼 수 있다.

```python
pd.crosstab(tips['sex'], tips['smoker'], normalize=True)
# smoker	Yes      	No
# sex		
# Male  	0.245902	0.397541
# Female	0.135246	0.221311
```

일반적으로 데이터분석에서는 조건부 확률에 대해 관심이 많기 때문에 crosstab이 div와 함께 쓰는 경우가 많다. crosstab과 div를 함께쓰면 남자일 경우에 흡연자일 확률, 여자일 경우에 흡연자일 확률을 각각 계산할 수 있다.

```python
tab = pd.crosstab(tips['sex'], tips['smoker'])
tab.div(tab.sum(1), axis=0)
# smoker	Yes	       No
# sex		
# Male  	0.382166	0.617834
# Female	0.379310	0.620690
```

bar 차트로 시각화할 수 있다.

```python
import matplotlib.pyplot as plt
tab = pd.crosstab(tips['sex'], tips['smoker'])
tab.div(tab.sum(1), axis=0).plot(kind='bar', stacked=True)
plt.show()
```

<center>
<figure>
<figcaption>성별에 따른 흡연자 비율</figcaption>
<img src='/assets/post-img/Python/crosstab.png' alt='crosstab'>
</figure>
</center>