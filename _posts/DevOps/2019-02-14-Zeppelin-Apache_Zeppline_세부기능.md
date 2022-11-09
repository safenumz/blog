---
layout: post
title: '[Zeppelin] Zeppeline 세부기능'
category: DevOps
tags: [zeppeline, spark]
comments: true
---

# Apache Zeppeline 세부기능
## 특징
- 협업 가능
- 마크다운 문법 가능
- 코드 숨기기, 결과 숨기기 가능
- 대시 보드로 활용 가능
- 트위터에서는 사내에서 수백명이 Zeppeline을 사용하고 있다.

## Interpreter Settings
- 맨 위에 있는 것이 기본 : 우선순위 설정 가능
- %md : 마크다운 문법을 쓰겠다는 의미
- %sql : spark sql를 쓰겠다는 의미
- %python : 파이썬 코드

## Tip
- 코드를 작성할 때 ${} 안에 변수를 넣어주면 그래프 위에 그 변수를 변경할 수 있게 할 수 있는 박스가 나온다. 그 박스의 값을 변경하면 바로 그래프에 반영된다.
- sql은 기본결과가 테이블인데 Display System 부분에서 변경할 수 있다.
- 데이터는 1000개까지 기본적으로 보여진다.
- 피봇차트를 쓸 수 있다. x축과 y축을 바꿔가며 분석이 가능하다.

```sql
%sql
select age, job, count(1) value
from bank
group by age, job
order by age, job
```

- 그래프를 area로 선택하고 settings에서 Keys에 age, groups에 job, values에 sum을 넣으면 다음과 같은 그래프가 그려진다.

 ![zepplelin](/assets/post-img/Spark/0001.png)

 - csv 등의 파일로 다운로드 가능하다.
 - HTML 코드도 가능, 이미지 넣을 수 있다.
 - Angular : 따로 서버가 없어도 간단히 UI 만들어서 사용 가능하다.
 - NGINX 보안기능 제공
