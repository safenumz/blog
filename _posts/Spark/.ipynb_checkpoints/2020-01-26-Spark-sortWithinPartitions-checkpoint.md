---
layout: post
title: '[Spark] 파티션 별 정렬 sortWithinPartitions'
category: Spark
tags: [spark, 파티션, 정렬, sortWithinPartitions]
comments: true
---

- 트랜스포메이션을 처리하기 전에 성능을 최적화하기 위해 파티션 별 정렬을 수행함

```python
# 예시
spark.read.format("json").load("/data/*-mm.json).sortWithinPartitions("count")
```