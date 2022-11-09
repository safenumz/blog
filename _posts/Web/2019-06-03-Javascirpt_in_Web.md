---
layout: post
title: '[WEB] JavaScript in Web'
category: WEB
tags: [javascript]
comments: true
---

# Java Script

## 변천

<a>             | 2010년 이전      | 2010년 이후
:-------------:|:--------------:|:-------------:
Presentation   | HTML, CSS, JS  | HTML, CSS, JS
Application    | JSP, ASP, PHP  | Node.js
Database       | Oracle, MySQL  | mongodb



~~~javascript
<script type="text/javascript">
	<!--
	자바스크립트 코드
	->
<script>
~~~

# Data Type

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>1_datatype.html</title>
    <script type="text/javascript">
        // 1. 자바스크립트는 자료형에 관대하다.
        var byensu = "문자열";
        document.writeln('변수의 값: ' + byensu + '<br/>');
        byensu = 1000;
        document.writeln('변수의 값: ' + byensu + '<br/>');

        // 2. 리터럴 - 자료형(데이터타입)에 들어가는 값
        var arr = ['안녕', 'Hello', 'Hola', '곤니찌와'];
        document.writeln('배열의 요소: ' + arr[1] + '<br/>');

        var arr = ['안녕', ['Hello', 'Hola'], '곤니찌와'];
        document.writeln('배열의 요소: ' + arr[1][0] + '<br/>');

        var obj = {x : "안녕", y : "Hello", z : "곤니찌와"};
        document.writeln("객체의 프로퍼티: " + obj.x + "<br/>" );
        document.writeln("객체의 프로퍼티: " + obj["x"] + "<br/>" );

        var x;
        document.writeln("x=" + x);
        document.writeln("obj.a =" + obj.a);
    </script>
</head>
<body>

</body>
</html>
~~~

# Operator

~~~html
!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>2_operator.html</title>
    <script type="text/javascript">
        // 1. == 와 === 차이점
        var num = 10;
        var str = '10';
        if (num != str) alert('같다1');
        else alert('다르다1');

        if (num !== str) document.write('같다2 <br/>');
        else document.write('다르다2 <br/>');
    </script>
</head>
<body>

</body>
</html>
~~~



# JavaScript 이벤트 바인드
1. HTML에서 이벤트 함수를 호출 (최근에는 사용 자제)
- 예전방식: <요소 onclick='함수명'/>
2. 이벤트핸들러(\*) : HTML과 JS 분리
- javascript 코드에서 요소를 찾고 이벤트를 건다.
- 요소.onevent = 함수명;
3. 이벤트리스너 : IE8 이전 동작하지 않음
- 요소.addEventListener('이벤트명', 함수명);



~~~html
<!DOCTYPE html>
<html>
<meta charset="UTF-8">
<title>메뉴판</title>
	<link type="text/css">

~~~


~~~html
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<title> main.html </title>
</head>
 <script type="text/javascript">
  window.onload = function(){
   var winObj = window.open('sub.html', ' ', 'width =700, height= 480');
   winObj.moveTo(screen.availWidth/4, screen.availHeight/4);
  document.frm.choose.onchange = function(){
   var x = document.frm.choose.selectedIndex;
    winObj.frm1.choose1.value = document.frm.choose.value
   }     
  }
 </script>
<body>
 <form name = "frm">
 <select name="choose">
   <option>서울시</option>
   <option>경기도</option>
   <option>전라도</option>
   <option>경상도</option>
  </select>
  <p>
 <input type = "text"  name ="location"  id="location">
 </p>
</form>
</body>
</html>

~~~

~~~html
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<title> sub.html </title>
<script type="text/javascript">
  window.onload = function(){  
   var x = document.frm1.location;
   for(var i =0; i<x.length; i++){
    x[i].onclick = function(){
        opener.frm.location.value = this.value;   
    }  
   }   
  }  
</script>
</head>
<body>
 <form name="frm1">
 <p>
 <input type = "text"  name="choose1" id="choose1">
 </p>
  <input type="radio" name="location" value='가산' > 가산<br>
  <input type="radio" name="location" value='판교' > 판교 <br>
  <input type="radio" name="location" value='부평'> 부평 <br>
 </form>
</body>
</html>
~~~
