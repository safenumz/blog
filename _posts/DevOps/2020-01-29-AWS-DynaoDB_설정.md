---
layout: post
title: '[AWS] DynamoDB 설정'
category: DevOps
tags: [aws]
comments: true
---

# Not Only SQL
- 관계형 데이터 베이스와의 극명한 차이

## 다이나믹 스키마
- Structure를 정의하지 않고도 Documents, Key Values 등을 생성
- 각각의 다큐먼트는 유니크한 Structure로 구성 가능
- 데이터베이스들마다 다른 Syntax
- 필드들을 지속적으로 추가 가능

## Scalability
- SQL Databases are vertically scalable - CPU, RAM or SSD
- NoSQL Databases are horizontally scalable - Sharding / Partitioning

# 파티션(Partition)에 대한 이해
- 데이터 매니지먼트, 퍼포먼스 등 다양한 이유로 데이터를 나누는 일
- Vertical vs Horisonal Partition
- 버티컬 파티션은 테이블을 더 작은 테이블로 나누는 작업으로써 노멀라이제이션 후에도 경우에 따라 파티션 칼럼을 나누는 파티션 작업을 함
- Schema / Structure 자체를 카피하여 데이터 자체를 Sharded Key로 분리


# 서비스 > 데이터베이스 > DynamoDB

## Create Table
- Table name*: top_tracks
- Primary key: 
	- Partition Key: artist_id String
	- Add sort key: id String
- 읽기/쓰기 용량 모드
	- Provisioned(free-tier eligible): auto scaling이 가능한 부분(scaling하는 데 시간 소요), 서버를 정해서 쓰기 때문에 free-tier로 쓸 수 있는 부분이 있음
	- On-demand: 얼마나 쓸지 모를 때, 사용한 만큼만 비용을 지불하고 싶을 때
- Auto Scaling
	- 읽기 용량: 70% 사용되면 자동으로 늘어나도록 설정

## Create Index
- Primary key: genre
- Index name: genre_index
- Projected attributes: All


# Python과 DynamoDB 연동

## boto3 패키지 설치

~~~shell
# MacOS 경우
$ pip3 install boto3 --user
~~~

## INSERT Data

~~~python
import sys
import os
import boto3
import requests
import base64
import json
import logging
import pymysql


client_id = <api id>
client_secret = <api password>

host = <aws endpoint>
port = 3306
username = <id>
database = <database>
password = <password>


def main():


    try:
        dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2', endpoint_url='http://dynamodb.ap-northeast-2.amazonaws.com')
    except:
        logging.error('could not connect to dynamodb')
        sys.exit(1)

    try:
        conn = pymysql.connect(host, user=username, passwd=password, db=database, port=port, use_unicode=True, charset='utf8')
        cursor = conn.cursor()
    except:
        logging.error("could not connect to rds")
        sys.exit(1)

    headers = get_headers(client_id, client_secret)

    table = dynamodb.Table('top_tracks')

    cursor.execute('SELECT id FROM artists')

    countries = ['US', 'CA']
    for country in countries:
        for (artist_id, ) in cursor.fetchall():

            URL = "https://api.spotify.com/v1/artists/{}/top-tracks".format(artist_id)
            params = {
                'country': 'US'
            }

            r = requests.get(URL, params=params, headers=headers)

            raw = json.loads(r.text)

            for track in raw['tracks']:

                data = {
                    'artist_id': artist_id,
                    'country': country
                }

                data.update(track)

                table.put_item(
                    Item=data
                )


def get_headers(client_id, client_secret):

    endpoint = "https://accounts.spotify.com/api/token"
    encoded = base64.b64encode("{}:{}".format(client_id, client_secret).encode('utf-8')).decode('ascii')

    headers = {
        "Authorization": "Basic {}".format(encoded)
    }

    payload = {
        "grant_type": "client_credentials"
    }

    r = requests.post(endpoint, data=payload, headers=headers)

    access_token = json.loads(r.text)['access_token']

    headers = {
        "Authorization": "Bearer {}".format(access_token)
    }

    return headers


if __name__=='__main__':
    main()
~~~

## SELECT Data

~~~python
import sys
import os
import boto3

from boto3.dynamodb.conditions import Key, Attr

def main():

    try:
        dynamodb = boto3.resource('dynamodb', region_name='ap-northeast-2', endpoint_url='http://dynamodb.ap-northeast-2.amazonaws.com')
    except:
        logging.error('could not connect to dynamodb')
        sys.exit(1)

    table = dynamodb.Table('top_tracks')

	# table.scan (테이블 전체 스캔 속도가 느림, 왠만하면 query로 사용)
    response = table.query(
		#KeyConditionExpression=Key('artist_id').eq('0cc6vw3VN8YlIcvr1v7tBL'),
        #KeyFilter
        FilterExpression=Attr('popularity').gt(90)
    )
    print(response['Items'])
    print(len(response['Items']))



if __name__=='__main__':
    main()
~~~