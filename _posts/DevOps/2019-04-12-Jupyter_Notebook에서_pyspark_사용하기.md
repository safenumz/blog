---
layout: post
title: '[Jupyter Notebook] pyspark 사용하기'
category: DevOps
tags: [jupyter notebook, pyspark]
comments: true
---

# Environment
- MacOS
- Spark 2.4.0

# Mac에서 Jupyter Notebook에서 pyspark 사용하기

## py4j 설치
- Python-Java Integration

```shell
$ conda install py4j
```

## .bash_profile 수정

~~~shell
export SPARK_HOME=/usr/local/Cellar/apache-spark/2.4.0
export PYTHONPATH=$SPARK_HOME/libexec/python:$SPARK_HOME/libexec/python/build:$PYTHONPATH
PYTHONPATH=$SPARK_HOME/python/lib/py4j-0.10-src.zip:$PYTHONPATH
export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/build:$PYTHONPATH
~~~

## kernel 설정

~/Library/Jupyter/kernel 경로에 pyspark 폴더를 만들고 그 안에 kernel.json 파일을 생성하고 아래 내용을 넣어 준다.

~~~shell
{
 "display_name": "PySpark",
 "language": "python",
 "argv": [
  "/anaconda3/bin/python",
  "-m",
  "ipykernel",
  "-f",
  "{connection_file}"
 ],
 "env": {
  "SPARK_HOME": "/usr/local/Cellar/apache-spark/2.4.0/",
  "PYTHONPATH": "/usr/local/Cellar/apache-spark/2.4.0/libexec/python/:/usr/local/Cellar/apache-spark/2.4.0/libexec/python/lib/py4j-0.10-src.zip",
  "PYTHONSTARTUP": "/usr/local/Cellar/apache-spark/2.4.0/libexec/python/pyspark/shell.py",
  "PYSPARK_SUBMIT_ARGS": "--master local[*] pyspark-shell"
 }
}
~~~
