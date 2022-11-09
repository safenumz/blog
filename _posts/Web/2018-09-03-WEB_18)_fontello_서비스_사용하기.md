---
layout: post
title: '[WEB] 18) fontello 서비스 사용하기'
category: WEB
tags: [web, html, css, 생활코딩]
comments: true
---

# [Fontello](http://fontello.com)
## Fontello 서비스
- [http://fontello.com](http://fontello.com)
- 이모티콘 서비스
- icon-emo-coffee
- 0xe808을 &#xe808로 변경
- <i calss="icon-emo-coffee"></i> 방식을 사용하면 font-faily를 지정안해도 됨

~~~html
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="../../assets/fontello.css">
    <style>
      body {
        font-family: "fontello";
        color: red;
        font-size: 100px;
      }
    </style>
  </head>
  <body>
    <!-- &#xe808<br> -->
    <i class="icon-emo-coffee"></i>
  </body>
</html>
~~~


- content로 문자 삽입하면 검색엔진에서는 검색 안됨
- animation 입력 : <i class="icon-emo-coffee animate-spin"></i>

~~~html
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="../../assets/fontello.css">
    <link rel="stylesheet" href="../../assets/animation.css">
    <style>
      body {
        font-size: 100px;
      }
      #test:before{
        content: "s";
        color: red;
        font-size: 30px;
      }
      #test:after{
        content:"e"
      }
    </style>
  </head>
  <body>
    <div id="test">
      A
    </div>
    <i class="icon-emo-coffee animation-spin"></i>
  </body>
</html>
~~~

## Font 만들기
- [https://thenounproject.com](https://thenounproject.com)
- thenownproject 홈페이지에 들어가서 마음에 드는 이미지를 svg 포맷으로 다운, Fontello 사이트에 드래그하면 업로드되고 font> 다운로드 할 수 있음
- Fontello사이트에서 config.json 파일을 import 하면 이전에 선택했던 이미지가 나옴

~~~html
<link rel="stylesheet" href="../../assets/fontello/css/science.css">
<link rel="stylesheet" href="../../assets/fontello/css/animation.css">
<i class="icon-emo-coffee animation-spin"></i>
~~~
