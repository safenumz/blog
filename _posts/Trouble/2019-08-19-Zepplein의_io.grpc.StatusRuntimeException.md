---
layout: post
title: '[Trouble zeppelin] io.grpc.StatusRuntimeException 에러'
category: Trouble
tags: [zeppelin, io.grpc.StatusRuntimeException]
comments: true
---

# Environment
- CentOS 7
- Zeppelin 0.8.1

# Trouble
- Zepplein에서 io.grpc.StatusRuntimeException 에러가 발생하는 경우

# Shooting
- zeppelin에서 제한한 메모리를 초과해서 나오는 에러이다. 
- zeppelin-site.xml 파일에서 zeppelin.websocket.max.text.message.size property를 늘려준다.

~~~sh
$ vi $ZEPPELIN_HOME/conf/interpreter.json
~~~

~~~sh
# inter
"zeppelin.ipython.grpc.message_size": {
        "propertyName": "zeppelin.ipython.grpc.message_size",
        "defaultValue": "533554432",
        "description": "grpc message size, default is 32M",
        "type": "number"
}
~~~
