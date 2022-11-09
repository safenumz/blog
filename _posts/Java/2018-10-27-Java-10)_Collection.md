---
layout: post
title: '[Java] 10) Collections'
category: Java
tags: [java]
comments: true
---

# Collections
- 배열과 같이 자료(데이터)를 효율적으로 관리하기 위한 방법

## List
- List는 인터페이스로 이를 구현한 클래스는 인덱스를 이용해서 데이터를 관리
- 인덱스를 이용하며 데이터 중복이 가능
- Vector, ArrayList, LinkedList

~~~java
import java.util.ArrayList;

public class MainClass {
	public static void main(String[] args) {

		// ArrayList 객체 생성
		ArrayList<String> list = new ArrayList<String>();

		System.out.println("list.size : " + list.size());

		// 데이터 추가
		list.add("Hello");
		list.add("Java");
		list.add("World");
		System.out.println("list.size : " + list.size());
		System.out.println("list : " + list);

		// 데이터 추가
		list.add(2, "Programming");
		System.out.println("list : " + list);

		// 데이터 변경
		list.set(1, "C");
		System.out.println("list : " + list);

		// 데이터 추출
		String str = list.get(2);
		System.out.println("list.get(2) : " + str);
		System.out.println("list : " + list);

		// 데이터 제거
		str = list.remove(2);
		System.out.println("list.remove(2) : " + str);
		System.out.println("list : " + list);

		// 데이터 전체 제거
		list.clear();
		System.out.println("list : " + list);

		// 데이터 유무
		boolean b = list.isEmpty();
		System.out.println("list.isEmpty() : " + b);

		System.out.println(" ============================ ");

	}
}
~~~

## Map
- Map은 인터페이스로 이를 구현한 클래스는 Key를 이용해 데이터를 관리
- 중복되지 않은 Key를 이용하며, 데이터 중복이 가능



~~~java
import java.util.ArrayList;

public class MainClass {
	public static void main(String[] args) {
		HashMap<Integer, String> map = new HashMap<Integer, String>();
		System.out.println("map.size() : " + map.size());

		map.put(5, "Hello");
		map.put(6, "Java");
		map.put(7, "World");

		System.out.println("map : " + map);
		System.out.pritnln("map.size() : " + map.size());

		map.put(8, "!!");
		System.out.println("map : " + map);

		// 데이터 교체
		map.put(6, "C");
		System.out.println("map: " + map);

		// 데이터 추출
		str = map.get(5);
		System.out.println("map.get(5) : " + str);

		// 데이터 제거
		map.remove(8);
		System.out.println("map: " + str)

		// 특정 데이터 포함 유무
		b = map.containsKey(7);
		System.out.println("map.containsKey(7) : " + b);

		b = map.containsValue("World");
		System.out.println("map.containsValue(\"World\") : " + b);

		map.clear();
		System.out.println("map: " + map);

		// 데이터 유무
		b = map.isEmpty();
		System.out.println("map.isEmpty() : " + b);
	}
}
~~~