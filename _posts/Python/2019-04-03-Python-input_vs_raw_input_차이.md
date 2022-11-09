---
layout: post
title: '[Python] input vs raw_input의 차이'
category: Python
tags: [python, input, raw_input]
comments: true
---

## Python input vs raw_input 차이

Python에서 User의 입력을 받는 방식은 두 가지가 있다.
 
input()은 정수형으로 받고 raw_input()은 문자열로 받는 차이가 있다.

~~~python
>> name = input("이름입력: ")
이름입력: 'Jason'
~~~

input()은 정수형으로 받기 때문에 User가 문자를 입력할 경우에는 반드시 문자열 표시 ""를 해줘야 입력이된다. ""를 입력하지 않으면 에러를 발생시킨다.

~~~python
>> name = raw_input("이름입력: ")
이름입력: Jason
~~~

raw_input()은 User가 "" 표시를 하지 않고도 문자열을 입력가능하다.




