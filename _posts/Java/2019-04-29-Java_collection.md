---
layout: post
title: '[Java] Collection'
category: Java
tags: [java, collection]
comments: true
---

# 정리
## 1. 상속
### 설계자
- 부모클래스 설계 : 공통부분
- 자식클래스 설계 : 고유부분

### 개발자
- 부모클래스의 멤버 그냥 사용
- 부모클래스의 없는 요소는 추가해서 사용
- 부모클래스의 멤버있지만 수정필요 경우 - overriding

## 2. 다형성
### [조건] 
- 상속관계 + 오버라이딩
- 부모변수에 자식 객체가 생성

## 3. abstract(추상)
- 미완성
- 자식클래스가 오버라이딩으로 미완성을 완성시켜줘야 함

## 4. interface : 표준화

## 5. 변경불가 - final

## 6. 공유 - static
- 클래스명 접근

# 유용한 클래스
## 기본 함수 형변환

~~~java
package z_useful;

import java.util.Objects;

public class EqualsEx {

    public static void main(String[] args) {
        Student a = new Student("012345", "홍길동");
        Student b = new Student("012345", "홍길동");
        if (a.equals(b)) System.out.println("동일인");
        else System.out.println(b);
        System.out.println(a.toString());
        System.out.println(b);
    }
}

class Student extends Object {
    String hakbun, name;

    Student() {}
    Student(String hakbun, String name) {
        this.hakbun = hakbun;
        this.name = name;
    }
    public boolean equals(Object obj) {
        // 형변환
        Student other = (Student) obj;
        if (hakbun.equals(other.hakbun)) return true;
        else return false;
    }

    public String toString() {
        return "[" + hakbun + "]" + name;
    }
}

~~~

## 얉은 복사 vs 깊은 복사

~~~java
package z_useful;

import java.util.Arrays;

public class CloneEx {

    public static void main(String[] args) {
        String[] array = {"안녕", "헬로우", "올라", "곤니찌와"};
//         String[] copy = array; // 얉은 복사
        String[] copy = array.clone(); // 깊은 복사, 실질적인 메모리까지 복사
        System.out.println(Arrays.toString(array));
        System.out.println(Arrays.toString(copy));

        copy[1] = "Hello";
        copy[2] = "Hola";

        System.out.println(Arrays.toString(array));
        System.out.println(Arrays.toString(copy));
    }
}

~~~

## enum

~~~java
package z_useful;

// enum : 상수들의 모음, 클래스와 유사 사용
enum Size {
    SMALL,
    MEDIUM,
    LARGE
}

public class EnumTest {
    public static void main(String[] args) {
        Size size = Size.SMALL; // 추후에 화면에서 넘어오는 값
        switch (size) {
            case SMALL : System.out.println("작은거"); break;
            case MEDIUM : System.out.println("중간거"); break;
            case LARGE : System.out.println("큰거"); break;
        }
    }
}
~~~

~~~java
package z_useful;

public enum Names {
    GILDONG("개발자") {
        String salary() { return "추가급여"; }
    },
    GILJA("디자이너"),
    GILJUN("팀장");

    String job;
    Names(String job) { this.job = job; }

    String salary() { return "고정급여"; }
}
~~~

## SimpleDataFormat

~~~java
package k_format;

import java.text.SimpleDateFormat;
import java.util.Date;

public class DateFormatEx {

    public static void main(String[] args) {
        Date today = new Date();
        System.out.println(today);

        SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일" +
         " E요일 hh시 mm분 ss초");
        System.out.println(sdf.format(today));
    }
    
}
~~~

## DecimalFormat

~~~java
package k_format;

import java.text.DecimalFormat;

public class DecimalFormatEx {

    public static void main(String[] args) {
        double[] data = {1234567, 555555555.123, 99.9999999, 1234.50};

        // 자리수 벗어나면 반올림되기 때문에 넉넉하게 표시해야 함
        DecimalFormat df = new DecimalFormat("###,###,###.###########");

        for (int i = 0; i < data.length; i++) {
            System.out.println(df.format(data[i]));
        }
    }

}
~~~

## MessageFormat

~~~java
package k_format;

import java.text.MessageFormat;

public class MessageFormatEx {

    public static void main(String[] args) {
        String message = "친애하는 <{0}> {1}님, 본문생략.....감사합니다.";
        String[][] data = {
                {"홍길동", "부장"},
                {"홍길자", "과장"},
                {"홍길숙", "대리"},
                {"홍길순", "사원"},
        };

        for (int i = 0; i < data.length; i++) {
            System.out.println(MessageFormat.format(message, data[i]));
        }
    }

}
~~~

## ChoiceFormat

~~~java
package k_format;

import java.text.ChoiceFormat;

public class ChoiceFormatEx {

    public static void main(String[] args) {
        int[] scores = {88, 79, 55, 62, 78, 100};

        double[] limits = {60, 70, 80, 90};
        String[] grades = {"D", "C", "B", "A"};

        ChoiceFormat cf = new ChoiceFormat(limits, grades);

        for (int k : scores) {
            System.out.println(k + ":" + cf.format(k));
        }
    }

}

/*
88:B
79:C
55:D
62:D
78:C
100:A
*/
~~~

## MessageFormat

~~~java
package k_format;

import java.text.MessageFormat;

public class MessageFormatEx {

    public static void main(String[] args) {
        String message = "친애하는 <{0}> {1}님, 본문생략.....감사합니다.";
        String[][] data = {
                {"홍길동", "부장"},
                {"홍길자", "과장"},
                {"홍길숙", "대리"},
                {"홍길순", "사원"},
        };

        for (int i = 0; i < data.length; i++) {
            System.out.println(MessageFormat.format(message, data[i]));
        }
    }

}
~~~

# Collection
## 배경
### JDK 1.2	 이전
- Vector, HashTable, Properties 등
- 각각의 클래스로 각각의 메소드를 이용

### JDK 1.2 이후
- 모든 컬랙션 클래스를 표준화하여 체계화
- 인터페이스와 다형성을 이용ㅇ한 객체지향적 설계
- 공통으로 사용하는 메소드

## 기본 인터페이스
- List와 Set은 공통 메소드가 많아 Collection 인터페이스에 공통부분을 넣고 그것을 확장(extends)
- Map은 키와 값을 가지는 구조로 다른 데이터 형식임

### List
- 순서가 있는 데이터의 집합
- 데이터 중복을 허용

### Set
- 순서를 유지하지 않는 데이터의 집합
- 데이터의 중복을 허용 안함

### Map
- 키(key)와 값(value)으로 이루어진 데이터의 집합
- 순서를 유지하지 않음
- 키는 중복을 허용하지 않음
- 값은 중복을 허용

### (1) Collection 인터페이스

|메소드                              |설명                        |
|:--------------------------------:|:-------------------------:|
| boolean add(Object o)            | Collection에 추가           |
| boolean addAll(Collection c)     |                           |
| void clear()                     | 모든 객체를 삭제              |
| boolean contain(Object o)        | 지정된 객체가 포함되어 있는지 확인 |
| boolean containAll(Collection c) |                           |
| boolean equals(Object o)         | 동일한지 확인                 |
| boolean isEmpty()                | 비어 있는지 확인              |
| Iterator iterator()              | Iterator를 얻어 반환         |
| boolean remove(Obejct o)         | 지정된 객체를 삭제             |
| boolean removeAll(Collection c)  |                           |
| int size()                       | 객체의 개수를 반환             |
| Object[] toArray()               | 객체를 객체배열로 반환          | 
| Object[] toArray(Obejct[] a)     |                           |



### (2) List 인터페이스
- 저장순서가 유지되는 컬렉션의 인터페이스
- ArrayList : 1.2 버전 이전의 Vector
- LinkedList

### (3) Set 인터페이스
- 중복을 허용하지 않고 저장순서가 유지되지 않는 컬렉션의 인터페이스

### (4) Map 인터페이스
- 키(key)와 값(value)을 묶어서 저장하는 컬렉션 인터페이스
- 키는 유일해야하기에 중복 허용 안됨

## 2. List 구현
### Stack : LIFO (Last In First Out)
- 마지막에 넣는 것을 먼저 꺼내는 구조
- 0, 1, 2 데이터를 순서대로 넣으면, 꺼낼 때는 2부터 꺼내야 함.
- 예) 웹브라우저의 뒤로 눌렸을 때 워드프로그램에서 undo/redo 실행시
- Stack은 Vector를 확장한 클래스

~~~java
Stack stack = new Stack();
stack.push("0");
stack.push("1");
stack.push("2");

while (!stack.empth()) {
    System.out.println(stack.pop());
}
~~~

~~~java
package l_collection;

import java.util.*;

public class StackQueueEx {

    public static void main(String[] args) {
        // Stack - LIFO(Last In First Out)
        Stack stack = new Stack();
        stack.push("A");
        stack.push("B");
        stack.push("C");
        while (!stack.isEmpty()) {
            System.out.println(stack.pop());
        }
    }
}
~~~

### Queue : FIFO (First In First Out)
- 처음에 넣은 것을 먼저 꺼내는 구조
- 0, 1, 2 데이터를 순서대로 넣으면, 꺼낼 때는 0부터 꺼내야 함.
- 예) 최근사용문서, 프린터 대기목록
- Queue는 인터페이스로 이것을 구현한 클래스가 LinkedList를 비롯하여 여러개 있음

~~~java
Queue queue = new LinkedList();

queue.offer("0");
queue.offer("1");
queue.offer("2");

while (!queue.isEmpty()) {
    System.out.println(queue.poll());
}
~~~

~~~java
package l_collection;

import java.util.*;

public class StackQueueEx {

    public static void main(String[] args) {
        // Queue - FIFO (First In First Out) - 인터페이스
        Queue queue = new LinkedList();
        queue.offer("가");
        queue.offer("나");
        queue.offer("다");
        while (!queue.isEmpty()) {
            System.out.println(queue.poll());
        }
    }
}
~~~

### Iterator

~~~java
List list = new ArrayList();
Iterator it = list.iterator();

while (is.hasNext()) {
    System.out.println(it.next());
}
~~~

~~~java
package l_collection;

import java.util.*;

/*
1. List 구조
    - 순서를 저장
 */

public class ArrayListEx {

    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<String>(4); // 동적인 배열
        list.add("rabbit");
        list.add("zebra");
        list.add("fox");
        list.add("cat");
        list.add("ant");
        list.add("lion");
//        list.add(100);
//        list.add(200);

        for (String str : list) {
            System.out.println(str);
        }

        System.out.println(list);

        System.out.println(list.toString());

        list.set(2, "dog");
        System.out.println(list);

        list.remove(4);
        System.out.println(list);

        Collections.sort(list);
        System.out.println(list);

        /*
        for (int i = 0; i < list.size(); i++) {
            String str = (String) list.get(i);
            System.out.println(str);
        }
        */
    }

}
~~~

~~~java
package l_collection;

import java.util.ArrayList;

public class ArrayListEx2 {

    public static void main(String[] args) {
        ArrayList list = method();
        // 여기서 출력
        System.out.println(list);
    }

    static ArrayList method() {
        ArrayList list = new ArrayList();

        Student a = new Student("홍길자", 20);
        Student b = new Student("홍길숙", 30);
        Student c = new Student("홍길동", 40);

        list.add(a);
        list.add(b);
        list.add(b);

        return list;
    }

}

class Student {
    String name;
    int age;
    Student (String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String toString() {
        return name + "학생은 " + age + "세입니다.";
    }

}

~~~

## 3. Set 구현
### Hashset
- HashSet은 저장순서를 유지하지 않고 중복 요소를 저장하지 않는 Set을 구현한 클래스

~~~java
// 로또 번호 만들기
Set set = new HashSet();
for (int i = 0; set.size() < 5; i++) {
    int num = (int)(Math.random() * 45) + 1;
    set.add(num);
}
~~~

~~~java
package l_collection;

import java.util.HashSet;

public class HashSetEx {

    public static void main(String[] args) {
        HashSet list = new HashSet();

        list.add("rabbit");
        list.add("zebra");
        list.add("fox");
        list.add("fox");
        list.add("ant");
        list.add("lion");

        System.out.println(list);
    }

}
~~~

~~~java
package l_collection;

import java.util.HashSet;
import java.util.ArrayList;
import java.util.Collections;

public class HashSetLotto {

    public static void main(String[] args) {
        HashSet<Integer> lotto = new HashSet<Integer>();
        while (lotto.size() < 6) {
            int su = (int)(Math.random() * 45) + 1;
            lotto.add(su);
        }
        System.out.println(lotto);

        ArrayList<Integer> list = new ArrayList<Integer>(lotto);
        Collections.sort(list);
        System.out.println(list);
    }

}
~~~

### TreeSet
- TreeSet은 이진검색트리 형태로 데이터를 저장하는 컬랙션 클래스
- 이진검색트리는 정렬, 검색, 범위 검색 탁월

~~~java
TreeSet set = new TreeSet();
int[] score = {56, 35, 77, 68, 92, 58, 93, 90, 88, 73};

> set.subSet(20, 50);
35
> set.haedSet(80);
35, 56, 58, 68, 73, 77
> set.tailSet(80);
88, 90, 92, 93
~~~

- 기본은 중위순회
- 전위, 후위는 직접 함수를 만들어야 함

~~~java
package l_collection;

import java.util.TreeSet;

public class TreeSetEx {

    public static void main(String[] args) {
        TreeSet set = new TreeSet();
        set.add("lion");
        set.add("ant");
        set.add("snake");
        set.add("dog");
        set.add("cat");
        set.add("elephant");
        set.add("zebra");
        set.add("bee");
        set.add("tiger");

        System.out.println(set);
        System.out.println(set.subSet("d", "s"));
        System.out.println(set.headSet("d"));
        System.out.println(set.tailSet("d"));
    }

}

/*
[ant, bee, cat, dog, elephant, lion, snake, tiger, zebra]
[dog, elephant, lion]
[ant, bee, cat]
[dog, elephant, lion, snake, tiger, zebra]
*/
~~~

## 4. Map 구현
### hashtable -> HashMap
- Map은 키(key)와 값(value)을 묶어서 하나의 데이터(entry)로 저장하는데, 유일한 키로 어떤 값을 검색

~~~java
Entry[] talble;
class Entry {
    Object key;
    Object value;
}
~~~

~~~java
package l_collection;

import java.util.HashMap;
import java.util.Scanner;

public class HashMapEx {

    public static void main(String[] args) {
        HashMap<String, String> map = new HashMap<String, String>();
        map.put("kimjava", "1111");
        map.put("parkjava", "1234");
        map.put("leejava", "1234");
        // kimjava가 중복되었기 때문에 1111은 지워지고 9999로 대체된다.
        map.put("kimjava", "9999");

        Scanner input = new Scanner(System.in);
        boolean stop = false;
        while(!stop) {
            System.out.println("아이디와 패스워드 입력");
            System.out.println("아이디 입력->");
            String id = input.nextLine();
            System.out.println("패스워드 입력->");
            String pw = input.nextLine();
            // 아이디가 map에 key에 해당되는가?
            if (map.containsKey(id)) {
                // if (map.containValue(pw))
                if (map.get(id).equals(pw)) {
                    System.out.println("로그인성공");
                    stop = true;
                } else {
                    System.out.println("비밀번호가 일치하지 않습니다.");
                    continue;
                }
            } else {
               System.out.println("존재하지 않는 아이디입니다.");
               continue;
            }
        }
    }

}

/*
아이디와 패스워드 입력
아이디 입력->
kimjava
패스워드 입력->
9999
로그인성공
*/
~~~

## Properties
- Properties는 이전 클래스인 Hashtable을 상속받아 구현한 클래스
- Hashtable의 키와 값이 Obejct 형태인데 반해, Properties의 키와 값은 String이다.
- 환경설정 등을 저장하는데 사용


# Java GUI

~~~java
package a_sample;

/*
Java GUI
    - AWT - 1.2 이전
    - SWING - 1.2 이후
    Java - Write Once Run Anywhere
 */

import java.awt.*;
import javax.swing.*;

public class AwtTest extends JFrame {
    // 1. 멤버변수 선언
    JButton b1;
    JButton b2;
    // 한 칸 입력받는 텍스트에어리어
    JTextField tf;
    // 여러줄 입력받는 텍스트에어리어
    JTextArea ta;
    // Checkbox JCheckBox 대소문자 주의
    JRadioButton cb1, cb2;

    AwtTest() {
        super("나의 두번째 창");
        // 2. 객체 생성
        b1 = new JButton("OK");
        b2 = new JButton("Cancel");
        tf = new JTextField(30);
        ta = new JTextArea(10, 50);

        /* CheckBox를 Radio 버튼으로 변경해야 함
        cb1 = new JCheckBox("Male");
        cb2 = new JCheckBox("Feamale");
        */
        cb1 = new JRadioButton("Male");
        cb2 = new JRadioButton("Female");

        ButtonGroup bg = new ButtonGroup();
        bg.add(cb1);
        bg.add(cb2);
    }

    void addLayout() {
        // 3. 붙이기

        /*
        FlowLayout f1 = new FlowLayout();
        setLayout(f1);
        // 한 개일 때는 위의 두 줄을 한 줄로 줄일 수 있다.
        */

        // 윗줄 상단 중앙에 배치 - 컴포넌트의 크기가 고정
        // setLayout(new FlowLayout());
        // 행과 열에 배치 - 컴포넌트 크기가 전체 사이즈의 크기에 맞춰 변경됨
        // setLayout(new GridLayout(3, 2));

        // NORTH, SOUTH, CENTER, WEST, EAST 영역에 배치 - 5개 밖에 못 붙임
        setLayout(new BorderLayout());
        add(b1, BorderLayout.NORTH);
        add(b2, BorderLayout.SOUTH);
        add(tf, BorderLayout.WEST);
        add(ta, BorderLayout.CENTER);

        /*
        add(cb1, BorderLayout.EAST);
        add(cb2, BorderLayout.EAST); // 중복되서 안나옴 패널을 써야 함
        */

        // Pannel에다가 J를 붙여줘야 swing에서 쓸 수 있음
        JPanel p = new JPanel();
        p.add(cb1);
        p.add(cb2);
        add(p, BorderLayout.EAST);

        // 4. 화면에 출력
        setSize(600, 480);
        setVisible(true);

        // Swing 기능, x버튼 누르면 프로그램 실행 종료
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }

    public static void main(String[] args) {
        AwtTest at = new AwtTest();
        at.addLayout();
    }
}

~~~

~~~java
package b_info;

import java.awt.*;
import javax.swing.*;

public class InfoTest {
    // 1. 멤버변수 선언
    // is a가 아닌 has a 사용(상속 안받았음), f 써야 함
    JFrame f;
    JButton bAdd, bShow, bSearch, bDelete, bCancel, bExit;
    JTextArea ta;
    JTextField tfName, tfId, tfTel, tfSex, tfAge, tfHome;

    // 2. 멤버객체 생성
    InfoTest() {
        f = new JFrame("정보");
        bAdd = new JButton("입력");
        bShow = new JButton("전체보기");
        bSearch = new JButton("검색");
        bDelete = new JButton("삭제");
        bCancel = new JButton("취소");
        bExit = new JButton("종료");

        ta = new JTextArea();
        tfName = new JTextField(15);
        tfId = new JTextField();
        tfTel = new JTextField();
        tfSex = new JTextField();
        tfAge = new JTextField();
        tfHome = new JTextField();
    }

    // 3. 화면 붙이기 및 화면 출력
    void addLayout() {
        // South 영역
        JPanel p_south = new JPanel();
        p_south.setLayout(new GridLayout(1, 6));
        p_south.add(bAdd);
        p_south.add(bShow);
        p_south.add(bSearch);
        p_south.add(bDelete);
        p_south.add(bCancel);
        p_south.add(bExit);

        // West 영역
        JPanel p_west = new JPanel();
        p_west.setLayout(new GridLayout(6, 2));
        p_west.add(new JLabel("이름"));
        p_west.add(tfName);
        p_west.add(new JLabel("아이디"));
        p_west.add(tfId);
        p_west.add(new JLabel("전화번호"));
        p_west.add(tfTel);
        p_west.add(new JLabel("성별"));
        p_west.add(tfSex);
        p_west.add(new JLabel("나이"));
        p_west.add(tfAge);
        p_west.add(new JLabel("주소"));
        p_west.add(tfHome);

        // 프레임영역에 붙이기
        f.setLayout(new BorderLayout());
        f.add(p_south, BorderLayout.SOUTH);
        f.add(p_west, BorderLayout.WEST);
        f.add(ta, BorderLayout.CENTER);

        f.setSize(480, 500);
        f.setVisible(true);
    }

    public static void main(String[] args) {
        InfoTest it = new InfoTest();
        it.addLayout();
    }

}

~~~




