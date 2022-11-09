---
layout: post
title: '[JavaScript] 06) 객체 (Object)'
category: JavaScript
tags: [javascript]
comments: true
---

# 객체 (Object)
- 배열의 인덱스는 숫자인데 객체의 인덱스는 문자라는 차이가 있음

```javascript
var grades = {'egoing': 10, 'k8805': 6, 'sorialigi': 80};
console.log(grades)
// Object {'egoing': 10, 'k8805': 6, 'sorialigi': 80}
```

```javascript
var grades = {};
grades['egoing'] = 10;
grades['k8805'] = 6;
grades['sorialgi'] = 80;

console.log(grades['sorialgi']);
// 80

console.log(grade['k88' + '05'])
// 6

console.log(grade.K8805)
// 6
```


```javascript
// for문 돌때 key 값이 나옴
var grades = {'egoing': 10, 'k8805': 6, 'sorialgi': 80};
for(key in grades) {
	console.log("key : " + key + "value : " + grades[key] + <br />);
}
```

- JavaScript로 HTML 리스트를 만들 수 있음

```html
<!DOCTYPE html>
<html><body>
<ul>
<script type="text/javascript">
var grades = {'egoing': 10, 'k8805': 6, 'sorialgi': 80};
for(var name in grades) {
	documnet.write("<li>key : " + name +  "values : " + grades[name] + "</li>");
}
</script>
</ul>
</body></html>
```

- 위 코드실행에 대한 결과는 다음과 같음

```html
<!DOCTYPE html>
<html>
<head>...</head>
<body>
	<ul>
		<script type="text/javascript">...</script>
		<li>key : egoing value : 10</li>
		<li>key : k8805 value : 6</li>
		<li>key : sorialgi value : 80</li>
	</ul>
</body>
</html>
```


## 객체 지향 프로그래밍

```javascript
// 객체 내부에 함수를 넣을 수도 있음
// this는 함수를 소유하고 있는 객체를 가리키는 변수임

var grades = {
	'list' : {'egoing' : 10, 'k8805' : 8, 'sorialgi' : 80},
	'show' : function() {
		for(var name in this.list) {
			console.log(name, this.list[name]);
		}
	}
}

alert(grades['lsit']['egoing'])
// 10

alert(grades['show']())
// egoing : 10
// k8805 : 8
// sorialgi : 80

grades.show();
// egoing : 10
// k8805 : 8
// sorialgi : 80
```
