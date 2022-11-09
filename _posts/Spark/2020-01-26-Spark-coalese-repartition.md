---
layout: post
title: '[Spark] coalesce와 repartition 차이'
category: Spark
tags: [spark, coalesce, repartition]
comments: true
---

# coalesce와 repartition 차이
- 일반적으로 coalesce는 데이터 수가 줄어들어 파티션의 수를 줄일 때 사용하고, 파티션 수를 늘려야 할 때는 repartition을 사용함
- repartition이 파티션 수를 증가시키거나 줄이는 것이 모두 가능한 반면에, coalesce는 줄이는 것만 가능
- repartition이 shuffle을 하는 반면에 coalesce는 **shuffle = true** 옵션을 주지 않는 이상 셔플을 하지 않음
- coalesce에 **shuffle = true** 줄 경우에 사실상 repartition과 같으며, 파티션을 늘리는 것이 가능해짐

## repartition 예시

```python
df.rdd.getNumPartitions() # 1
df.repartition(5)

# 특정 칼럼을 기준으로 자주 필터링한다면 자주 필터링 되는 컬럼을 기준으로 파티션을 재분배
df.repartition(col("DEST_COUNTRY_NAME"))

df.repartition(5, col("DEST_COUNTRY_NAME"))
```

## coalesce 예시
- 전체 데이터를 셔플하지 않고 파티션을 병합하려는 경우에 주로 사용

```python
# DEST_COUNTRY_NAME를 기준으로 셔플을 수행해 5개의 파티션으로 나누고, 전체 데이터를 셔플없이 병합
df.repartition(5, col("DEST_COUNTRY_NAME")).coalesce(2)
```