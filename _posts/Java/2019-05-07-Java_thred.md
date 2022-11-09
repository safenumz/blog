---
layout: post
title: '[Java] thread'
category: Java
tags: [java, thread]
comments: true
---

# Java의 입출력(IO)
- Java <---> 다른장치(device)
*stream : 데이터를 보내고 받는 가상 장치
1) byte 형
- OuputStream
- InpuStream
* filter stream
- DataOutputStream / DataInputStream
- ObjectOuputStream / ObjectInputStream
-> 직렬화 (Serializable)
2) char 형


~~~java
// String을 타는 클래스는 직렬화 시켜야 한다.
class Student implements Serializable {
	String name;
	int age;
	double height;
	Encore encore;

	Student(String name, int age, double height) {
		// 멤버 변수에 각각 지정
	}

	void setName() {}
	void getName() {}
	void calScore() {}
}

class Encore implements Serializable {

}
~~~

- program : 일종 software
- process : 현재 cpu가 실행중인 프로그램
- thread : 한 프로세스 안에서 동시에(?) 여러 작업

cf) processor : cpu

## 자바의 쓰레드 처리 절차
1. Thread 상속 (Runnable 구현)
- run() 오버라이딩

2. start() 호출 -> run() 호출됨


~~~java
package thread.basic;

import java.io.IOException;

public class EX_Ex01_Process {

    public static void main(String[] args) {
        // 다른 응용 프로그램을 프로세스 실행
        Runtime rt = Runtime.getRuntime();
        try {
            rt.exec("C://Program Files/internel explorer/iexplore.exe");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
~~~


~~~java
package thread.basic;

public class Ex02_ThreadTest {

    public static void main(String[] args) {
        MakeCar1 mc1 = new MakeCar1("차틀만들기");
        // mc1.run();
        mc1.start();

        MakeCar1 mc2 = new MakeCar1("도색작업");
        // mc2.run();
        mc2.start();
    }

}

class MakeCar1 extends Thread {

    String work;
    // 옛날 방식, this를 써서 명확하게 하는 것이 좋다.
    MakeCar1(String _work) {
        work = _work;
    }

    public void run() {
      for (int i = 0; i < 5; i++) {
          System.out.println(work + "작업중");

          // 0.5초
          try {
              Thread.sleep(500);
          } catch (InterruptedException e) {

          }
      }
    }

}
~~~

~~~java
public class Ex {
	public static void main(String[] args) {
		MakeCar1 mc1 = new MakeCar1("차틀 만들기");
		mc1.start();

		MakeCar mc2 = new MakeCar1
	}
}
~~~

## Runnable

~~~java
package thread.basic;

public class Ex02_RunnableTest {

    public static void main(String[] args) {
        MakeCar1 mc1 = new MakeCar1("차틀만들기");
        Thread t1 = new Thread(mc1);
        // mc1.run();
        t1.start();

        /*
        MakeCar1 mc2 = new MakeCar1("도색작업");
        Thread t2 = new Thread(mc2);
        mc2.start();
         */
        // mc2.run();

        new Thread(new MakeCar2("도색")).start();


        System.out.println("프로그램 끝");
    }

}

class MakeCar2 implements Runnable {

    String work;
    // 옛날 방식, this를 써서 명확하게 하는 것이 좋다.
    MakeCar2(String _work) {
        work = _work;
    }

    public void run() {
      for (int i = 0; i < 5; i++) {
          System.out.println(work + "작업중");

          // 0.5초
          try {
              Thread.sleep(500);
          } catch (InterruptedException e) {

          }
      }
    }

}
~~~

~~~java
package thread.basic;

import java.awt.*;


public class Ex3_DalTest extends Frame{
	
	Dal a, b, c;
	
	public Ex3_DalTest() {
		setSize( 500, 400 );
		setVisible( true );

		a = new Dal(this, 0, 50);
		b = new Dal(this, 0, 100);
		c = new Dal(this, 0, 150);
		
		// # 
		// 각 객체의 쓰레드 메소드(run) 호출한다

		/*
		a.start();
		b.start();
		c.start();

		 */

		new Thread(a).start();
		new Thread(b).start();
		new Thread(c).start();

		
	}	
	


	public void paint( Graphics g )
	{
		g.setColor(Color.red);
		g.drawString("__@", a.x, a.y );

		g.setColor(Color.blue);
		g.drawString("__@", b.x, b.y );

		g.setColor(Color.green);
		g.drawString("__@", c.x, c.y );
			
	}

	public static void main(String[] args)
	{
		Ex3_DalTest dal = new Ex3_DalTest();
	}

}

/*
# Thread 처리
(1) Thread 클래스나 Runnable 인터페이스 상속
(2) run() 오버라이딩

	- 임의의 수를 speed 값으로 지정		
	- x 값을 위의 임의의 수를 더하기
	- 화면을 다시 그린다 (*) app.repaint() 이용
	- 임의의 수만큼 잠시 쓰레드를 블럭한다
	# 위의 작업을 반복한다
*/
class Dal implements Runnable {
// class Dal extends Thread {
	int x, y;
	int speed;
	Frame app;
	
	public Dal( Frame _app, int _x, int _y )
	{		
		app = _app;
		x = _x;																						
		y = _y;	
	}
	
	public void run()
	{
		while (true) {
			speed = (int) (Math.random() * 10); // 0 ~ 9 임의의 수
			x += speed;
			// 그림을 다시 그리는 함수
			app.repaint();
			try {
				Thread.sleep((int) (Math.random() * 10) * 100);
			} catch (InterruptedException e) {

			}

		}

	}

}
~~~

~~~java
package thread.basic;

import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Ex4_CounterTest extends JFrame{
    private JPanel p1, p2;
    private JButton btn;
    private JTextArea ta;
    private JLabel lb;
    private boolean inputChk;
    
    public Ex4_CounterTest() {
        setTitle("단일 스레드 테스트!");
        p1 = new JPanel();
        p1.add(btn = new JButton("Click"));
        p1.add(lb = new JLabel("Count!"));//추가 
        add(p1,"North");
        
        p2 = new JPanel();       
        p2.add( ta = new JTextArea(20,50));
        add(p2);
        
        setBounds(200, 200, 600, 400);
        setVisible(true);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        
        
        btn.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
            	
                // 1- 카운트 다운을 시작하는 스레드 
                new Thread(new Runnable(){
                    public void run() {
                        for (int i = 10; i > 0; i--) {
                            lb.setText(String.valueOf(i));
                            try {
                                Thread.sleep(1000);
                            } catch (Exception ex) {

                            }
                        }
                    }
                }).start();
            	
                // 2- 입력값을 받아서 JTextArea에 붙이는 작업 
                new Thread(new Runnable() {
                    public void run() {
                        for (int i = 10; i > 0; i--) {
                            if (inputChk) {
                                lb.setText("빙고");
                                inputChk = false;
                                return;
                            }
                            lb.setText(String.valueOf(i));
                            try {
                                Thread.sleep(1000);
                            } catch (Exception ex) {

                            }
                        }
                        /*
                        String input = JOptionPane.showInputDialog("값을 입력");
                        ta.append(input);

                         */
                    }
                }).start();
   
            }
        });
    }
    public static void main(String[] args) {
        new Ex4_CounterTest();
    }
}

~~~


~~~java
package thread.basic;

import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Ex4_ZBombTest extends JFrame{
	private JPanel p1,p2;
	private JButton btn;
	private JLabel lb, image;
	private boolean inputChk;

	Ex4_ZBombTest(){
		setTitle("폭탄 테스트!");
		p1 = new JPanel();
		p1.add(btn = new JButton("시작")); 
		p1.add(lb = new JLabel("카운트다운")); 
		add(p1,"North");
		p2 = new JPanel();
		p2.add(image = new JLabel(new ImageIcon("src\\thread\\basic\\ex\\bomb_1.jpg")));

		add(p2, "Center");
		setBounds(200, 200, 600, 600);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setVisible(true);

		btn.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {

				// 1- 카운트 다운을 시작하는 스레드 
				new Thread(new Runnable() {
					public void run() {
						for (int i = 10; i > 0; i--) {
							if (inputChk) {
								inputChk = false;
								return;
							}
							if (i == 1) image.setIcon(new ImageIcon(
									"Basic5Class/src/thread/basic/imgs/bomb_2.jpg"));
							lb.setText(String.valueOf(i));
							try {
								Thread.sleep(1000);
							} catch (Exception e) {

							}
						}
					}
				}).start();

				// 2- 입력값을 받아서 JTextArea에 붙이는 작업 
				new Thread(new Runnable() {
					public void run() {
						String secret = "1234";
						String answer = JOptionPane.showInputDialog("암호를 대시오");
						if (secret.equals(answer)) {
							inputChk = true;
						}
					}
				}).start();

			}
		});
	}

	public static void main(String[] args) {
		new Ex4_ZBombTest();
	}
}
~~~

~~~java
package thread.basic;

public class Ex05_ThreadStop {

    public static void main(String[] args) {
        System.out.println("메인쓰레드:" + Thread.currentThread().getName());
        MakeCar3 mc = new MakeCar3();
        mc.start();
        try {
            Thread.sleep(2000);
        } catch (Exception ex) {

        }
        System.out.println("쓰레드 종료");
        /* 일반적으로 stop은 사용하지 않음, boolean 변수로 제어한다.
        mc.stop();
         */

        /* 한번 stop으로 죽인 Thread는 다시 살아나지 않는다.
        mc.start();
         */

        mc.flag = true;

    }

}

class MakeCar3 extends Thread {

    boolean flag = false;

    public void run() {
        for (int i = 0; i < 50; i++) {
            if (flag) return;
            System.out.println(getName() + "작업중");

            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {

            } catch(ThreadDeath e) {
                System.out.println("쓰레드 강제 종료됨");
            }
        }
    }

}


~~~

~~~java
package thread.basic;

public class EX06_Priority {

    public static void main(String[] args) {
        // 운영체제에서 라운드로빈 방식으로 실행되기 때문에 사용자가 아무리
        // 우선순위를 높게 준다고 해도 크게 영향을 미치지 못한다.
        MakeCar mc1 = new MakeCar("차틀", Thread.MAX_PRIORITY);
        mc1.start();

        MakeCar mc2 = new MakeCar("도색", Thread.MIN_PRIORITY);
        mc2.start();
    }

}

class MakeCar extends Thread {
    String work;
    MakeCar(String _work, int prio) {
        work = _work;
        setPriority(prio);
    }
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(work + "작업중");
        }
    }
}
~~~

~~~java
package thread.basic;

class Count {

    int i = 0;

    // synchronized 앞의 작업이 끝날 때까지 기다린다.
    // 느리지만 자원을 공유하고 있다면 자원의 신뢰성을 보장하기 위해 쓴다.
    // synchronized void increment() {
    void increment() {
        // 전체가 아닌 필요한 것만 synchronized를 쓰는 것이 좋다.
        // 공유하는 객체가 없을 때는 자기 자신(this)이라도 lock을 건다.
        synchronized (this) {
            i++;
        }
    };
}

class ThreadCount extends Thread {

    Count cnt;

    public ThreadCount(Count cnt) {
        this.cnt = cnt;
    }

    public void run() {
        for (int i = 0; i < 100000000; i++) {
            cnt.increment();
        }
    }

}

public class Ex07_Synch {

    public static void main(String[] args) {

        Count cnt = new Count();

        ThreadCount tc1 = new ThreadCount(cnt);
        tc1.start();

        ThreadCount tc2 = new ThreadCount(cnt);
        tc2.start();

        try {
            tc1.join();
            tc2.join();
        } catch (Exception ex) {}

        System.out.println("i값 = " + cnt.i);
    }

}
~~~

## BreadTest

~~~java
class Bread {
	String bread;

	boolean isCheck = false;

	public void setBread(String bread) {
		this.bread = bread;
	}

	public String getBread() {
	
		return bread;
	}
}

class Baker extends Thread {
	Bread bbang;

	Baker(Bread bbang) {
		this.bbang = bbang;
	}
	
	public void run() {
		bbang.setBread("진열된 완성된 맛있는 빵");
	}
}

class Customer extends Thread {
	Bread bbang;

	Customer(Bread bbang) {
		this.bbang = bbang;
	}

	public void run() {
		System.out.println("빵을 사감: " + bbang.getBread());
	}
}

class Ex08_BreadTest {
	publci static void main(String[] args) {
		Bread bread = new Bread();

		Baker baker = new Baker(bread);
		
		Customer customer = new Customer(bread);

		customer.start();
		baker.start();

		try {
			customer.join();
			baker.join();
		} catch (Exception ex) {}
	}
}
~~~

## 복습

~~~java
1. 다음 메소드들 중에서 쓰레드의 실행은 멈추기 하는 것은 ?

(1) sleep() (2) stop() (3) wait()

(4) notify() (5) notifyAll()

/*
[정답] 1 2 3
*/


2. 쓰레드를 실행할 수 있는 클래스를 만들기 위해 사용되는 인터페이스는 ?

(1) Run (2) Runnable (3) Thread

(4) Threadable (5) Exception

/*
[정답] 2
*/


3. 쓰레드를 실행할 수 있는 클래스를 선언하였을 때 정의해야 하는메소드는 ?

(1) start() (2) wait() (3) run()

(4) init() (5) stop()

/*
[정답] 3
*/


4. 쓰레드에는 우선순위를 부여할 수 있는데, 이 경우 setPriority() 메소드를 안에 어떤 상수를 지정할수 있는가 ?

(1) MAX_PRIORITY (2) PRIORITY_MAX

(3) Thread.MAX_PRIORITY (4) Thread.PRIORITY_MAX

/*
[정답] 3
*/


10. 다음 소스 중 main() 메소드안에 “여기” 부분에서 쓰레드를 생성하고 실행하는 코드로적당한 것은 ?

 class Test implements Runnable {

  public static void main(String[] args) {

   /*  여기  */

  }
  public void run() {

  }

 }

(1) 
Test t = new Test();
t.run();

(2) 
Test t = new Test();
new Thread().run();

(3) 
Test t = new Test();
new Thread(t).run();

(4) 
Test t = new Test();
new Thread(t).start();

/*
[정답] 4
*/



11. 아래 프로그램에서 쓰레드를 생성하고 시작하기 위한 코드로 맞는것은 ?

 class MyThread implements Runnable {

  public void run() {

   try {

    for (int i = 0; i < 10; i++) {

     System.out.println(“Thread is running”);

     Thread.sleep(1000);

    }

   } catch (InterruptedException ex) {}

  }

 }

(1) new Thread(MyThread).start();

(2) new Thread(new MyThread()).run();

(3) new MyThread().start();

(4) new Thread(new MyThread()).start();

/*
[정답] 4
*/
~~~