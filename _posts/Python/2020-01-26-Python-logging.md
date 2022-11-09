---
layout: post
title: '[Python] 로그 남기기 (logging)'
category: Python
tags: [python, logging]
comments: true
---

# Stream에 로그 남기기
- 스트림(콘솔)에 로그를 찍기 위해 logging을 사용함
- warning level만 출력 됨
	- logging의 default log level이 warning으로 되어 있기 때문

~~~python
import logging

logging.info('my INFO log')
logging.warning('my WARNING log')
~~~

- logging의 basicConfig level을 DEBUG로 변경하면 DEBUG 로깅도 출력됨

~~~python
import logging

logging.basicConfig(level=logging.DEBUG)

logging.debug('my DEBUG log')
logging.info('my INFO log')
logging.warning('my WARNING log')
logging.error('my ERROR log')
logging.critical('my CRITICAL log')
~~~

# File에 로그 남기기
- basicConfig의 filename을 설정하면 해당 위치에 로그 파일이 생성됨

~~~python
import logging

def get_logging():

    logging.basicConfig(filename='./server.log', level=logging.DEBUG)

    logging.debug('my DEBUG log')
    logging.info('my INFO log')
    logging.warning('my WARNING log')
    logging.error('my ERROR log')
    logging.critical('my CRITICAL log')

if __name__ == "__main__":
    get_logging()
~~~

<pre>
DEBUG:root:my DEBUG log
INFO:root:my INFO log
WARNING:root:my WARNING log
ERROR:root:my ERROR log
CRITICAL:root:my CRITICAL log
</pre>

# Stream과 File에 동시에 로그 남기기
- logging.getLogger(__name__)으로 logger instance를 생성
- stream과 file에 로그를 남기는 handler를 생성
- logger instance에 stream과 file handler를 설정
- logger instance로 log를 찍음

~~~python
import logging

def get_logging():

    # logger instance 생성
    logger = logging.getLogger(__name__)

    # handler 생성 (stream, file)
    streamHandler = logging.StreamHandler()
    fileHandler = logging.FileHandler('./server.log')

    # logger instance에 handler 설정
    logger.addHandler(streamHandler)
    logger.addHandler(fileHandler)

    # logger instance로 log 찍기
    logger.setLevel(level=logging.DEBUG)
    logger.debug('my DEBUG log')
    logger.info('my INFO log')
    logger.warning('my WARNING log')
    logger.error('my ERROR log')
    logger.critical('my CRITICAL log')


if __name__ == "__main__":
    get_logging()
~~~

# 로그 형식(Formatting)

~~~python
#-*- coding: utf-8 -*-

import logging
import logging.handlers

def get_logging():

    # logger instance 생성
    logger = logging.getLogger(__name__)

    # formatter 생성
    formatter = logging.Formatter('[%(asctime)s][%(levelname)s|%(filename)s:%(lineno)s] >> %(message)s')

    # handler 생성 (stream, file)
    streamHandler = logging.StreamHandler()
    fileHandler = logging.FileHandler('./server.log')

    # logger instance에 fomatter 설정
    streamHandler.setFormatter(formatter)
    fileHandler.setFormatter(formatter)

    # logger instance에 handler 설정
    logger.addHandler(streamHandler)
    logger.addHandler(fileHandler)

    # logger instance로 log 찍기
    logger.setLevel(level=logging.DEBUG)

    logger.debug('my DEBUG log')
    logger.info('my INFO log')
    logger.warning('my WARNING log')
    logger.error('my ERROR log')
    logger.critical('my CRITICAL log')


if __name__ == "__main__":
    get_logging()
~~~


<pre>
[2020-01-26 01:05:14,928][DEBUG|log3.py:29] >> my DEBUG log
[2020-01-26 01:05:14,928][INFO|log3.py:30] >> my INFO log
[2020-01-26 01:05:14,928][WARNING|log3.py:31] >> my WARNING log
[2020-01-26 01:05:14,928][ERROR|log3.py:32] >> my ERROR log
[2020-01-26 01:05:14,928][CRITICAL|log3.py:33] >> my CRITICAL log
</pre>

# 로그 파일이 커질 때 파일 분할
- 파일을 분할하기 위해서는 fileHandler 설정을 변경하면 됨
- maxBytes: 파일 하나의 최대 바이트 수
- backupCount: 백업 파일 개수

~~~python
import logging
import logging.handlers

# 100MB 파일을 10개까지 남기겠다는 뜻
fileMaxByte = 1024 * 1024 * 100
filename="./server.log"
fileHandler = logging.handlers.RotatingFileHandler(filename, maxBytes=fileMaxByte, backupCount=10)
~~~

# 매일 자정에 새로운 로그 파일 만들기
- 저장할 파일명은 car.log
- when='midnight'의 경우 매일밤 자정에 새로운 파일이 만들어진다.
- 이때 만들어지는 형식은 suffix에 따라 설정된다.
- 예를 들면 여기서는 carLogHandler.suffix = "%Y%m%d" 이므로 car.log.20200101


~~~python
from logging import handlers
import logging

#log settings
carLogFormatter = logging.Formatter('%(asctime)s,%(message)s')

#handler settings
carLogHandler = handlers.TimedRotatingFileHandler(filename='car.log', when='midnight', interval=1, encoding='utf-8')
carLogHandler.setFormatter(carLogFormatter)
carLogHandler.suffix = "%Y%m%d"

#logger set
carLogger = logging.getLogger()
carLogger.setLevel(logging.INFO)
carLogger.addHandler(carLogHandler)

#use logger
carLogger.info("car is coming")
~~~