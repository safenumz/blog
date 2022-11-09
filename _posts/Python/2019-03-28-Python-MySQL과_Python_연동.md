---
layout: post
title : '[Python] MySQL과 Python 연동하기'
category: Python
tages: [sql, mysql, python]
commets: true
---

# 데이터 베이스 연결

```python
db = MySQLdb.connect(
  "[mysql 서버 ip]", # IP (Mysql 서버)
  "root", # Mysql 사용자 계정
  "dss", # password
  "world", # 데이터 베이스 이름
  charset='utf8', # 문자 인코딩 방식
)
db
```

# SELECT

```python
# query
SQL_QUERY="""
  SELECT code, name, Population
  FROM country;
"""

# excute query
cur = db.cursor()
cur.execute(SQL_QUERY)

rows = cur.fetchall()
for row in rows[:5]:
  print(row)

# ('ABW', 'Aruba', 103000)
# ('AFG', 'Afghanistan', 22720000)
# ('AGO', 'Angola', 12878000)
# ('AIA', 'Anguilla', 8000)
# ('ALB', 'Albania', 3401200)
```


# CREATE DATABASE

```python
QUERY = """
  CREATE DATABASE city_info_2;
"""
cur = db.cursor()

cur.execute(QUERY)

# 데이터 베이스 이동
QUERY = """
  USE city_info_2;
"""

cur = db.surcur()
cur.execute(QUERY)

# 테이블 생성
QUERY = """
  CREATE TABLE popular(
    rank int(4),
    city_name varchar(20),
    population int(10)
  );
"""

cur.execute(QUERY)

# 데이터 입력
QUERY = """
  INSERT INTO popular (rank, city_name, population)
  VALUES (1, 'seoul', 9900000),
  (2, 'busan', 3500000),
  (3, 'incheon', 2900000);
"""

cur.execute(QUERY)

# 데이터베이스와 cursor의 명령과 동기화시키는 명령
db.commit()
```

# MySQL Datas -> Pandas Dataframe

```python
QUERY = """
  SELECT * FROM popular
"""

cur.execute(QUERY)
```

```python
import pandas as pd

rows = cur.fetchall()
rows
# ((1, 'seoul', 9900000), (2, 'busan', 3500000), (3, 'incheon', 2900000))

df = pd.DataFrame(list(rows), columns=['rank', 'city_name', 'population'])
df

# rank	city_name	population
# 0	1	seoul	9900000
# 1	2	busan	3500000
# 2	3	incheon	2900000

city_df = pd.read_sql(QUERY, db)
city_df

# rank	city_name	population
# 0	1	seoul	9900000
# 1	2	busan	3500000
# 2	3	incheon	2900000
```

```python
import seaborn as sns
from sqlalchemy import create_engine

tips = sns.load_dataset('tips')
tips.tail()


# total_bill	tip	sex	smoker	day	time	size
# 239	29.03	5.92	Male	No	Sat	Dinner	3
# 240	27.18	2.00	Female	Yes	Sat	Dinner	2
# 241	22.67	2.00	Male	Yes	Sat	Dinner	2
# 242	17.82	1.75	Male	No	Sat	Dinner	2
# 243	18.78	3.00	Female	No	Thur	Dinner	2

engine = create_engine("mysql+mysqldb://root:[name]@[ip주소]/city_info_2")
engine

tips.to_sql(name='tips', con=engine, if_exists='replace')

# 데이터베이스 서버와 동기화
db.commit()
```

```python
import pandas pd
QUERY = """
  SELECT * FROM tips;
"""

df = pd.read_sql(QUERY, db)
df.tail()


# index	total_bill	tip	sex	smoker	day	time	size
# 239	239	29.03	5.92	Male	No	Sat	Dinner	3
# 240	240	27.18	2.00	Female	Yes	Sat	Dinner	2
# 241	241	22.67	2.00	Male	Yes	Sat	Dinner	2
# 242	242	17.82	1.75	Male	No	Sat	Dinner	2
# 243	243	18.78	3.00	Female	No	Thur	Dinner	2
```

## DELETE DATABASE

```python
QUERY="""
  DROP DATABASE city_info_2;
"""
cur = db.cursor()
cur.execute(QUERY)
```

## CONNECTION CLOSE

```python
db.close()
db
# <_mysql.connection closed at 557a45a65fa8>
```
