# Java理解多线程

### **程序program**

是为了完成特定的任务、用某种语言编写的一组指令的集合

简单的说就是我们编写的代码

### **进程：**

进程管理是操作系统重点、难点问题，也是贯穿[Linux](https://so.csdn.net/so/search?q=Linux&spm=1001.2101.3001.7020)学习的知识点。那么什么是进程？

**1.为什么引入进程的概念**？

> 从理论角度看，是对正在运行的程序过程的抽象；
> 从实现角度看，是一种数据结构，目的在于清晰地刻画动态系统的内在规律，有效管理和调度进入计算机系统主存储器运行的程序。

**2.什么是进程？**

>  狭义定义：进程就是一段程序的执行过程。
>		 广义定义：进程是一个具有一定独立功能的程序关于某个数据集合的一次运行活动。它是操作系统动态执行的基本单元，在传统的操作系统中，进程既是基本的分配单元，也是基本的执行单元。

进程就是运行的程序，例如我们使用的QQ，就启动了一个进程，操作系统就会进程分配内存空间。——运行的程序

进程是程序的一次执行过程、或是正在运行的一个程序。是动态的过程：有它自身产生、存在、消亡的过程。

关闭了就会释放进程所占的空间！——操作系统的理论知识

### **线程**

线程是进程创建的，是进程的一个实体。

一个进程可以有多个线程。

单线程

多线程

并发：同一个时刻，多个任务交替执行（单核）

并行：同一个时刻，多个任务同时执行（多核）

### 线程的创建方式1：继承Thread

构造方法

```java
Thread() 分配新的Thread对象
Thread(String name) 分配新的Thread对象
Thread(Runnable target) 分配新的Thread对象
Thread(Runnable target,String name) 分配新的Thread对象
```

普通方法

```java
static Thread currentThread( )
返回对当前正在执行的线程对象的引用
long getId()
返回该线程的标识
String getName()
返回该线程的名称
void run()
如果该线程是使用独立的 Runnable 运行对象构造的，则调用该 Runnable 对象的 run 方法
static void sleep(long millions)
在指定的毫秒数内让当前正在执行的线程休眠(暂停执行)
void start()
使该线程开始执行:Java虚拟机调用该线程的run()
```

子线程

```java
//继承 Thread 类 该类就能当做线程使用
/**
 * 
 * @author Administrator
 *	写自己的业务代码
 */
class Cat extends Thread{
	int times = 0;
	@Override
	public void run() { //重写run方法 自己的业务逻辑 
		while(true) {
			System.out.println("hello world! "+ (++times));
			try {
				Thread.sleep(1000);//1000ms
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			if (times == 10) {
				break;
			}
		}
	}
}
```

主线程

```java
public class thead01 {
	public static void main(String[] args) throws InterruptedException {
		//创建 Cat对象当做线程使用
		Cat cat = new Cat();
		cat.start();//启动线程
		//当main线程启动一个子线程 主线程不会阻塞 会继续执行
		System.out.println("主线程继续执行："+Thread.currentThread().getName());
		for(int i= 0;i<50;i++) {
			System.out.println("主线程："+i);
			Thread.sleep(1000);
		}
	}
}
```

通过控制台输入jconsole命令连接

![image-20230103204824007](https://chenyi-tuku.oss-cn-beijing.aliyuncs.com/Project-C2/image-20230103204824007.png)

执行情况

![image-20230103204910333](https://chenyi-tuku.oss-cn-beijing.aliyuncs.com/Project-C2/image-20230103204910333.png)

主线程断了，子线程继续执行

![image-20230103205258925](https://chenyi-tuku.oss-cn-beijing.aliyuncs.com/Project-C2/image-20230103205258925.png)



### 线程的创建方式2:实现Runnable接口

前提：如果自己的类已经extends另一个类,就无法多继承,此时,可以实现一个Runnable接口

开发线程

```java
class dog implements Runnable{ //实现Runnable接口 开发线程
	int count =0;
	@Override
	public void run() {
		// TODO Auto-generated method stub
		while(true) {
			System.out.println("hello~"+(++count));
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
	
}
```

代理模式实现

```java
public class thread02 {
	public static void main(String[] args) {
		dog dog = new dog();
		// dog,start 错误 没有start方法
		//底层使用了代理模式 使用了 设计模式
		Thread thread = new Thread(dog);
		thread.start();
	}
}
```

#### 继承和实现方式的比较

1. 继承Thread类

   优点: 编写简单,如果需要访问当前线程,无需使用Thread.currentThread()方法,直接使用this即可获得当前线程
   缺点: 自定义的线程类已继承了Thread类,所以后续无法再继承其他的类

2. 实现Runnable接口

   优点: 自定义的线程类只是实现了Runnable接口或Callable接口,后续还可以继承其他类,在这种方式下,**多个线程可以共享同一个target对象**,所以非常适合多个相同线程来处理同一份资源的情况,从而可以将CPU、代码、还有数据分开(解耦),形成清晰的模型,较好地体现了面向对象的思想

   **多线程：实现Runnable**

   ```java
   package com.demo.Runnable;
   
   public class Runnable01 {
   	public static void main(String[] args) {
   		T t = new T();
   		Thread thread = new Thread(t);
   		thread.start();
   		Thread thread2 = new Thread(t);
   		thread2.start();
   	}
   }
   class T implements Runnable{
   
   	int count = 0;
   	@Override
   	public void run() {
   		// TODO Auto-generated method stub
   		while(true) {
   			System.out.println("hahahaha\n"+(++count)+Thread.currentThread().getName());
   			try {
   				Thread.sleep(1000);
   			} catch (InterruptedException e) {
   				// TODO Auto-generated catch block
   				e.printStackTrace();
   			}
   			if (count == 10) {
   				break;
   			}
   		}
   	}
   	
   }
   ```

   ![image-20230103221158066](https://chenyi-tuku.oss-cn-beijing.aliyuncs.com/Project-C2/image-20230103221158066.png)

   缺点: 编程稍微复杂,如想访问当前线程,则需使用Thread.currentThread()方法

#### 实现多个线程:继承thread

```java
package com.demo.thread;

import java.util.Iterator;

public class thread03 {
	public static void main(String[] args) throws InterruptedException {
		Pig pig = new Pig();
		pig.start();
		Long long1 = new Long();
		long1.start();
		//主线程
		System.out.println("Main:"+Thread.currentThread().getName());
		for(int i = 0; i< 10;i++) {
			System.out.println("主线程："+i);
			Thread.sleep(1000);
		}
	}
}
class Pig extends Thread{
	int number = 0;
	@Override
	public void run() {
		// TODO Auto-generated method stub
		while(true) {
			System.out.println("hello world!"+(++number)+"线程1："+Thread.currentThread().getName());
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			if (number == 10) {
				break;
			}
		}
	}
}
class Long extends Thread{
	int number = 0;
	@Override
	public void run() {
		// TODO Auto-generated method stub
		while(true) {
			System.out.println("hi!"+(++number)+ "线程2："+Thread.currentThread().getName());
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			if (number == 5) {
				break;
			}
		}
	}
}

```

运行图

![image-20230103215238940](https://chenyi-tuku.oss-cn-beijing.aliyuncs.com/Project-C2/image-20230103215238940.png)

### 多线程售票问题

```java
package com.demo.ticket;

/**
 * 多线程售票，三个窗口售票100张
 * 
 * @author Administrator
 *
 */
public class SellTicket {
	public static void main(String[] args) {
//		SellTicket01 sellTicket01 = new SellTicket01();
//		SellTicket01 sellTicket02 = new SellTicket01();
//		SellTicket01 sellTicket03 = new SellTicket01();
//
//		sellTicket01.start();
//		sellTicket02.start();
//		sellTicket03.start();
		SellTicket02 sellTicket02 = new SellTicket02();
		new Thread(sellTicket02).start();
		new Thread(sellTicket02).start();
		new Thread(sellTicket02).start();
	}
}

//继承Thread
class SellTicket01 extends Thread {
	public static int ticketNum = 100;

	@Override
	public void run() {
		// TODO Auto-generated method stub

		while (true) {
			if (ticketNum <= 0) {
				System.out.println("票已卖完！");
				break;
			}
			try {
				Thread.sleep(100);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			System.out.println("窗口：" + Thread.currentThread().getName() + "正在售票" + "还剩：" + (--ticketNum) + "张票");

		}

	}
}
//实现Runnable
class SellTicket02 implements Runnable{
	public  int ticketNum = 100;

	@Override
	public void run() {
		// TODO Auto-generated method stub

		while (true) {
			if (ticketNum <= 0) {
				System.out.println("票已卖完！");
				break;
			}
			try {
				Thread.sleep(10);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			System.out.println("窗口：" + Thread.currentThread().getName() + "正在售票" + "还剩：" + (--ticketNum) + "张票");

		}

	}
}

```

#### 多线程方法

#### 线程终止

#### 线程插队：join

##### 插队练习

#### 垃圾回收机制：守护线程

#### 线程同步：synchronized

