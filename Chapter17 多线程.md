P580---P599

[toc]

# 线程介绍

> 程序
>
> 是为完成特定任务、用某种语言编写的一组指令的集合。
> 简单的说：就是我们写的代码

> 进程
>
> 1. 进程是指运行中的程序，比如我们使用QQ，就启动了一个进程，操作系统就会为该进程分配内存空间。当我们使用迅雷，又启动了一个进程，操作系统将为迅雷分配新的内存空间。
> 2. 进程是程序的一次执行过程，或是正在运行的一个程序。是动态过程:有它自身的产生、存在和消亡的过程。

> 什么是线程
>
> 1. 线程由进程创建的，是进程的一个实体
>
> 2. 一个进程可以拥有多个线程。

> 1. 单线程: 同一个时刻，只允许执行一个线程
> 2. 多线程: 同一个时刻，可以执行多个线程，比如: 一个qq进程，可以同时打开多个聊天窗口，一个迅雷进程，可以同时下载多个文件
> 3. 并发：同一个时刻，多个任务交替执行，造成一种“貌似同时”的错觉，简单的说，单核cpu实现的多任务就是并发。
> 4. 并行：同一个时刻，多个任务同时执行。多核cpu可以实现并行。
> 5. 并行和并发可以同时存在

```java
public class CpuNum {
	public static void main(String[] args) {
		Runtime runtime = Runtime.getRuntime();
		//获取当前电脑的cpu数量
		int cpuNums = runtime.availableProcessors();
		System.out.println("当前有cpu 个数=" + cpuNums);
  }
}
```

# 线程创建

- 创建线程的两种方式
  - 在java中线程来使用有两种方法。
    1. 继承Thread 类，重写 run 方法 （* Java核心技术卷I不推荐这种写法了）
    1. 实现Runnable接口，重写 run 方法

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240215下午21953859.png" alt="image-20240215下午21953859" style="zoom: 50%;" />

可以自己去实现Runable接口,自定义自己的线程类! 或者用Lambda表达式（匿名函数）实现一个。

```java
Runnable task1 = () -> {
  try{
  }...
}
```

## 通过Thread类创建线程

> ```java
> //1.当一个类继承了 Thread 类，该类就可以当做线程使用
> //2.我们会重写 run方法，写上自己的业务代码
> //3. run Thread类 实现了 Runnable 接口的run方法
> 
> /*
> @Override //Thread类重写Runnable类中的run方法
> public void run(){
> 	if (target != null){
> 		target.run();
> 	}
> }
> */
> public class Thread01{
>   public static void main(String[] args) {
>     //创建Cat对象,可以当做线程使用
>     Cat cat = new Cat();
>     cat.start(); //启动线程,最终会调用cat的run方法
>     //cat.run();//run 方法就是一个普通的方法, 没有真正的启动一个线程，就会把 run 方法执行完毕，才向下执行
>     //如果直接调用cat 的 run 方法,run的运行其实还是在main线程中,不会开始一个新的线程!就会把run方法执行完毕才去继续执行调用者省下的代码!
>     //cat.run(); 就是一个普通的方法!
>     //说明：当main线程启动一个子线程Thread-0，主线程不会阻塞，会继续执行
>     //这是主线程和子线程会交替进行
>     System.out.println("主线程继续执行" + Thread.currentThread().getName()); //名字默认为main
>     for (int i = 0; i < 60; i++) {
>       System.out.println("主线程 i=" + i);
>       //让主线程休眠
>       Thread.sleep(1000);
>     }
>   }
> }
> 
> class Cat extends Thread {
>   int times = 0;
>   @Override
>   public void run() {  //重写run方法,写上自己的业务逻辑
>     //该线程每隔一秒,在控制台输出"喵喵,我是小猫咪"
>     while(true) {
>       System.out.println("我是猫"+ sum++ + Thread.currentThread().getName());
>       try {
>         Thread.sleep(1000);
>       } catch (InterruptedException e) {
>         e.printStackTrace();
>       }
>       if(times == 80){
>         break;;//当 times 到 80, 退出 while, 这时线程也就退出..
>       }
>     }
>   }
> }
> ```
>
> <img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240215下午50832871.png" alt="image-20240215下午50832871" style="zoom: 50%;" />

> 程序运行时在终端使用JConsole监控线程的执行情况
>
> 发现主线程main尽管挂掉了结束了,但是如果还有子线程没有跑完,仍然会继续运行! 进程仍然不会结束!

> start()方法 源码解读
>
> ```java
> /*
> 
> （1）
> public synchronized void start() {
> 	start0() ;
> ｝
> （2）
>     //start0() 是本地native方法，是JVM调用，底层是c/c++实现
>     //真正实现多线程的效果，是start0()，而不是 run
>     private native void start0();
> */
> ```
>
> ![image-20240215下午53115323](/Users/yannlau/Library/Application Support/typora-user-images/image-20240215下午53115323.png)



## 实现Runnable接口

1. java是单继承的，在某些情况了一个类可能已经继承了某个父类,这时在用继承Thread类方法来创建线程显然不可能了。
2. java设计者们提供了另外一个方式创建线程，就是通过实现Runnable接口来创建线程
3. 请编写程序，该程序可以每隔1秒。在控制台输出“hi！”，当输出10次后，自动退出。请
   使用实现Runnable接口的方式实现。Thread02java，这里底层使用了设计模式［代
   理模式］=>代码模拟 实现Runnable接口 开发线程的机制

```java
public class Thread02 {
  public static void main(String[] args) {
    Dog dog = new Dog();
    //dog.start();这里不能调用,因为了Runnable里面没有start方法
    //创建了Thread对象,把dog对象(实现了Runnable),放入了Thread
    Thread thread = new Thread(dog); // Thread(Runnable)
    thread.start();
  }
}

// Tiger tiger = new Tiger();//实现了 Runnable
// ThreadProxy threadProxy = new ThreadProxy(tiger); //代理
// threadProxy.start();

class Animal {
}

public class Tiger extends Animal implements Runnable {
	public static void main(String[] args) {
    Tiger tiger = new Tiger();
    Thread thread = new Thread(tiger);
    thread.start();
  } 
  
  @Override
  public void run(){
    System.out.println("老虎嗷嗷叫");
  }
}

class Dog  implements Runnable { //通过Runnable接口,开发线程
  int count = 0;
  @Override
  public void run(){ //普通方法
    while(true) {
      System.out.println("小狗汪汪叫..hi" + (++count) + Thread.currentThread().getName());
      try{
        Thread.sleep(1000);
      }catch(InterruptedException e){
        e.printStackTrace();
      }
      if(count == 10){
        break;
      }
    }
  }
}
```

这里是用了一种设计模式 : [代理模式]

```java
//线程代理类 , 模拟了一个极简的 Threa
class ThreadProxy implements Runnable { //可以把Proxy类当做 ThreadProxy
  private Runnable target = null; //属性,类型是Runnable
  
  @Override
  public void run(){
     if(target != null) {
       targeet.run(); //动态绑定,实际是Tiger类型的（重写的）方法
     }
  }
  
  public ThreadProxy(Runnable target) {
    this.target = target;
  }
  
  public void start() {
    start0(); //这个方法是真正实现多线程的方法
  }
  
  public void start0(){ //这个方法实际上是 native方法,由虚拟机调用
    this.run();  //其中会调用run()方法
  }
  
}
```

代理模式,弥补了java单继承的缺点

如果只用一次,可以用匿名内部类实现Runnable传入`new Thread();`

## 线程使用应用案例-多线程执行

请编写一个程序，创建两个线程，一个线程每隔1秒输出“hello,world”.输出10次，退出，一个线程每隔1秒输出 “hi”.输出5次退出.Thread03.java

```java
package com.hspedu.threaduse;
/**
* @author 韩顺平
* @version 1.0
* main 线程启动两个子线程
*/
public class Thread03 {
  public static void main(String[] args) {
    T1 t1 = new T1();
    T2 t2 = new T2();
    Thread thread1 = new Thread(t1);
    Thread thread2 = new Thread(t2);
    thread1.start();//启动第 1 个线程
    thread2.start();//启动第 2 个线程
    //... }
  }
}
class T1 implements Runnable {
  int count = 0;
  @Override
  public void run() {
    while (true) {
      //每隔 1 秒输出 “hello,world”,输出 10 次
      System.out.println("hello,world " + (++count));
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      if(count == 60) {
        break;
      }
    }
  }
}
class T2 implements Runnable {
  int count = 0;
  @Override
  public void run() {
    //每隔 1 秒输出 “hi”,输出 5 次
    while (true) {
      System.out.println("hi " + (++count));
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      if(count == 50) {
        break;
      }
    }
  }
}

```

## 线程如何理解?

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240418上午105415754.png" alt="image-20240418上午105415754" style="zoom:50%;" />

> 继承Thread vs 实现Rinnable的区别
>
> 1. 从Java的设计来看，通过继承Thread或者实现Runnable接口来创建线程本质上没有区别 , 从JDK帮助文档我们可以看到Thread类本身就实现了Runnable接口 `start()->start0()`
>
> 2. 实现Runnable接口方式更加适合多个线程共享一个资源的情况，并且避免了单继承的限制,<u>***建议使用Runnable!***</u>
>
> 3. ```java
>    T3 t3 = new T3("hello ");
>    Thread thread01 = new Thread (t3);
>    Thread thread02 = new Thread (t3);
>    thread01.start();
>    threadoz.start();
>    System.out.println("主线程完毕");
>    ```
>

［售票系统］，编程模拟三个售票窗口售票100 分别使用 继承 Thread 和实现 Runnable方式
并分析有什么问题？SellTicket.java

```java
package com.hspedu.ticket;
/**
* @author 韩顺平
* @version 1.0
* 使用多线程，模拟三个窗口同时售票 100 张
*/
public class SellTicket {
  public static void main(String[] args) {
    //测试
    // SellTicket01 sellTicket01 = new SellTicket01();
    // SellTicket01 sellTicket02 = new SellTicket01();
    // SellTicket01 sellTicket03 = new SellTicket01();
    //
    // //这里我们会出现超卖.. // sellTicket01.start();//启动售票线程
    // sellTicket02.start();//启动售票线程
    // sellTicket03.start();//启动售票线程
    System.out.println("===使用实现接口方式来售票=====");
    SellTicket02 sellTicket02 = new SellTicket02();
    new Thread(sellTicket02).start();//第 1 个线程-窗口
    new Thread(sellTicket02).start();//第 2 个线程-窗口
    new Thread(sellTicket02).start();//第 3 个线程-窗口
  }
}
//使用 Thread 方式
class SellTicket01 extends Thread {
  private static int ticketNum = 100;//让多个线程共享 ticketNum
  @Override
  public void run() {
    while (true) {
      if (ticketNum <= 0) {
        System.out.println("售票结束...");
        break;
      }
      //休眠 50 毫秒, 模拟
      try {
        Thread.sleep(50);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      System.out.println("窗口 " + Thread.currentThread().getName() + " 售出一张票" + " 剩余票数=" + (--ticketNum));
    }
  }
}
//实现接口方式
class SellTicket02 implements Runnable {
  private int ticketNum = 100;//让多个线程共享 ticketNum
  @Override
  public void run() {
    while (true) {
      if (ticketNum <= 0) {
        System.out.println("售票结束...");
        break;
      }
      //休眠 50 毫秒, 模拟
      try {
        Thread.sleep(50);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      System.out.println("窗口 " + Thread.currentThread().getName() + " 售出一张票" + " 剩余票数=" + (--ticketNum));//1 - 0 - -1 - -2
    }
  }
}
```

# 线程终止

1． 当线程完成任务后，会自动退出。
2. 还可以通过使用变量来控制run方法退出的方式停止线程，即通知方式
3. 需求：启动一个线程t，要求在main线程中去停止线程t，请编程实现.
   ThreadExit_.java com.hspedu.exi

> ```java
>public class ThreadExit_  throws InterruptedException {
>   public static Void main(String[] args) {
>     T t1 = new T();
>     t1. start();
>     //如果希望main线程去控制t1 线程的终止，必须可以修改 Loop
>     //让t1 退出run方法 , 从而终止 t1线程 -> 通知方式
>     Thread.sleep(10*1000);
>        t1.setLoop(false);
>      }
>    }
> 
> class T extends Thread {
>   private int count = 0;
>   //设置一个控制变量
>     private boolean loop = true;
>     @Override
>     public void run() {
>       while(loop) {
>         try {
>            Thread.sleep(50); // 让当前线程休眠50ms
>          } catch (InterruptedException e){
>            e.printStackTrace();
>          }
>        }
>      }
>      public void setLoop(boolean loop) {
>       this.loop = loop;
>     }
>    }
>   ```
>   

# 线程常用方法

- 常用方法第一组

1. setName(String name) //设置线程名称，使之与参数 name 相同
2. getName() //返回该线程的名称
3. start() //使该线程开始执行；Java 虚拟机底层调用该线程的 start0 方法
4. run() //调用线程对象 run 方法；
5. setPriority //更改线程的优先级
6. getPriority //获取线程的优先级
7. sleep  //在指定的毫秒数内让当前正在执行的线程休眠（暂停执行）
8. interrupt //中断线程

> 注意事项和细节
>
> 1. start底层会创建新的线程，调用run，而 run 就是一个简单的方法调用，不会启动新线程
> 2. 线程优先级的范围
> 3. interrupt，中断线程，但并没有真正的结束线程。所以一般用于中断正在休眠线程
> 4. sleep：线程的静态方法，使当前线程休眠。Thread是一个native方法。应该是 即使在休眠状态，被interrupt()设置了中断状态，也能被JVM给通知到。
> 5. 如果线程中没有任何sleep wait等进入等待或者阻塞态的语句，即使被调用了interrupt()方法，也不会停止，除非自己主动去检测 isInterrupted(),具体见Java核心技术卷。

```java
public class Thread00 {
    public static void main(String[] args) throws InterruptedException {
        //测试相关方法
        T t = new T();
        t.setName("老刘");
        t.setPriority(Thread.MIN_PRIORITY);
        t.start();//启动子线程

        System.out.println(t.getName()); //会打印出老刘

        //主线程打印五个hi 然后就中断子线程的休眠
        for (int i = 0; i < 5; i++) {
            Thread.sleep(1000);
            System.out.println("hi" + i);
        }
      	//测试优先级
      	System.out.println(t.getName() + " 线程的优先级 = " + t.getPriority());//1
				//测试interrupt
        t.interrupt();//当执行到这里，就会中断 t线程的休眠。
    }
}

class T extends Thread {
    @Override
    public void run() {
        while (true) {
            for (int i = 0; i < 100; i++) {
                //Thread.currentThread().getName():获取当前进程的名字
                System.out.println(Thread.currentThread().getName() + "吃包子~~~~" + i);
            }
            try {
                System.out.println(Thread.currentThread().getName() + " 休眠中~~~");
                Thread.sleep(20000); //20秒
            } catch (InterruptedException e) {
                //当该线程执行到一个interrupt 方法时，就会catch 一个 异常，可以加入自己的业务代码
                // InterruptedException是捕获到一个中断异常。
                System.out.println(Thread.currentThread().getName() + "被 Interrupt了");
            }
        }
    }
}
```

- 常用方法第二组

> 1. yield：线程的礼让。让出cpu，让其他线程执行，但礼让的时间不确定，所以也不一定礼让成功.
>
>    ```java
>    public static native void yield();
>    ```
>
> 2. join：线程的插队。插队的线程一旦插队成功，则肯定先执行完插入的线程所有的任务.
>
>    ```java
>    public final void join() throws InterruptedException
>    ```
>
> 3. 案例：创建一个子线程，每隔1s 输出 hello，输出120次，主线程每隔1秒，输出 hi，输出 20次.   要求：两个线程同时执行，当主线程输出 5次后，就让子线程运行完毕，主线程再继续

```java
public class Thread01 {
    public static void main(String[] args) throws InterruptedException {
        int i = 1;
        T1 t1 = new T1();
        t1.start();
        while (true) {
            Thread.sleep(1000);
            System.out.println("Hi!" + i++);
            if (6 == i) {
                t1.join(); //线程插队,相当于让t1线程先执行完毕,然后接着下一句执行
              	sout("我是下一句!");
              //Thread.yield(); 礼让，不一定成功。可能是cpu核太多了没必要礼让
            }
            if (21 == i) break;
        }
    }
}

class T1 extends Thread {
    int i = 1;

    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            System.out.println("Hello!" + i++);
            if (21 == i)
                break;
        }
    }
}
```

# 用户线程和守护线程

> 1. 用户线程：也叫工作线程，当线程的任务执行完或通知方式结束
>2. 守护线程：一般是为工作线程服务的，当所有的用户线程结束，守护线程自动结束
> 3. 常见的守护线程：垃圾回收机制

用于想让主线程退出时,分支线程也跟着退出,不管是否执行完毕.

```java
public class Thread02 {
    public static void main(String[] args) throws InterruptedException {
        MyDaemonThread myDaemonThread = new MyDaemonThread();
        //如果希望当main线程结束后,子线程自动结束
        //只需将子线程设为守护线程即可
        //设置守护线程要在线程启动之前进行
        myDaemonThread.setDaemon(true);

        myDaemonThread.start();

        for (int i = 1; i <= 10; i++) {
            System.out.println("宝强在辛苦工作...");
            Thread.sleep(1000);
        }
    }
}
@SuppressWarnings("all")
class MyDaemonThread extends Thread {
    @Override
    public void run() {
        for (; ; ) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("马蓉和宋喆快乐聊天,哈哈哈~~~");
        }
    }
}
```

# 线程的生命周期

```java
public static enum Thread.State extends Enum<Thread.State>
线程状态。线程可以处于以下状态之一：
• NEW
尚未启动的线程处于此状态。
• RUNNABLE
在Java虚拟机中执行的线程处于此状态。
• BLOCKED
被阻塞等待监视器锁定的线程处于此状态。
• WAITING
正在等待另一个线程执行特定动作的线程处于此状态。
• TIMED WAITING
正在等待另一个线程执行动作达到指定等待时间的线程处于此状态。
• TERMINATED
已退出的线程处于此状态。
```

```java
package com.hspedu.state_;
/**
* @author 韩顺平
* @version 1.0
*/
public class ThreadState_ {
  public static void main(String[] args) throws InterruptedException {
    T t = new T();
    System.out.println(t.getName() + " 状态 " + t.getState());
    t.start();
    while (Thread.State.TERMINATED != t.getState()) {
      System.out.println(t.getName() + " 状态 " + t.getState());
      Thread.sleep(500);
    }
    System.out.println(t.getName() + " 状态 " + t.getState());
  }
}
class T extends Thread {
  @Override
  public void run() {
    while (true) {
      for (int i = 0; i < 10; i++) {
        System.out.println("hi " + i);
        try {
          Thread.sleep(1000);
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
      }
      break;
    }
  }
}
```

![image-20240216上午114144994](/Users/yannlau/Library/Application Support/typora-user-images/image-20240216上午114144994.png)



# Synchronized

- 线程同步机制
  1. 在多线程编程，一些敏感数据不允许被多个线程同时访问，此时就使用同步访问技术，保证数据在任何时刻，最多有一个线程访问，以保证数据的完整性。
  2. 也可以这样理解：线程同步，即当有一个线程在对内存进行操作时，其他线程都不可以对这个内存地址进行操作，直到该线程完成操作，其他线程才能对该内存地址进行操作.

> 同步具体方法
>
> 1. 同步代码块
>
> ```java
> synchronized(对象-this){ // 得到对象的锁，才能操作同步代码
> // 需要被同步代码；
> }
> ```
> 2. synchronized 还可以放在方法声明中，表示整个方法为同步方法
>      `public synchronized void m(String name){`
>              `// 需要被同步的代码`
>        `｝`
> 3. 如何理解：
>      就好像 某小伙伴上厕所前先把门关上（上锁），完事后再出来(解锁)，那么其它小伙伴就可在使用厕所了
> 4. 使用synchronized 解决售票问题
> 5. 锁是在对象上的,而不是在代码块上的!



# 互斥锁  

基本介绍

> 1. Java语言中，引入了对象互斥锁的概念，来保证共享数据操作的完整性。
>
> 2. 每个对象都对应于一个可称为“互斥锁”的标记，这个标记用来保证在任一时刻，只能有一个线程访问该对象。
> 3. 关键字synchronized 来与对象的互斥锁联系。当某个对象用synchronized修饰时，表明该对象在任一时刻只能由一个线程访问
> 4. 同步的局限性：导致程序的执行效率要降低
> 5. 同步方法（非静态的）的锁可以是this，也可以是其他对象（要求是同一个对象）
> 6. 同步方法（静态的）的锁为当前类本身。

使用互斥锁来解决售票问题 看老师代码演示(两种方式都演示下， 代码块加锁，和方法上加锁

```java
package com.hspedu.syn;
/**
* @author 韩顺平
* @version 1.0
* 使用多线程，模拟三个窗口同时售票 100 张
*/
public class SellTicket {
  public static void main(String[] args) {
    //测试
    // SellTicket01 sellTicket01 = new SellTicket01();
    // SellTicket01 sellTicket02 = new SellTicket01();
    // SellTicket01 sellTicket03 = new SellTicket01();
    //
    // //这里我们会出现超卖.. 
    // sellTicket01.start();//启动售票线程
    // sellTicket02.start();//启动售票线程
    // sellTicket03.start();//启动售票线程
    
    
    // System.out.println("===使用实现接口方式来售票=====");
    // SellTicket02 sellTicket02 = new SellTicket02();
    //
    // new Thread(sellTicket02).start();//第 1 个线程-窗口
    // new Thread(sellTicket02).start();//第 2 个线程-窗口
    // new Thread(sellTicket02).start();//第 3 个线程-窗口
    
    //测试一把
    SellTicket03 sellTicket03 = new SellTicket03();
    new Thread(sellTicket03).start();//第 1 个线程-窗口
    new Thread(sellTicket03).start();//第 2 个线程-窗口
    new Thread(sellTicket03).start();//第 3 个线程-窗口
  }
}
//实现接口方式, 使用 synchronized 实现线程同步
class SellTicket03 implements Runnable {
  private int ticketNum = 100;//让多个线程共享 ticketNum
  private boolean loop = true;//控制 run 方法变量
  Object object = new Object();
  //同步方法（静态的）的锁为当前类本身
  //老韩解读
  //1. public synchronized static void m1() {} 锁是加在 SellTicket03.class
  //2. 如果在静态方法中，实现一个同步代码块. 
  /*
  synchronized (SellTicket03.class) {
    System.out.println("m2");
  }
  */
  public synchronized static void m1() {
  }
  public static void m2() {
    synchronized (SellTicket03.class) {
      System.out.println("m2");
    }
  }
  //老韩说明
  //1. public synchronized void sell() {} 就是一个同步方法
  //2. 这时锁在 this 对象
  //3. 也可以在代码块上写 synchronize ,同步代码块, 互斥锁还是在 this 对象
  public /*synchronized*/ void sell() { //同步方法, 在同一时刻， 只能有一个线程来执行 sell 方法
    synchronized (/*this*/ object) {
      if (ticketNum <= 0) {
        System.out.println("售票结束...");
        loop = false;
        return;
      }
      //休眠 50 毫秒, 模拟
      try {
        Thread.sleep(50);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      System.out.println("窗口 " + Thread.currentThread().getName() + " 售出一张票" + " 剩余票数=" + (--ticketNum));//1 - 0 - -1 - -2
    }
  }
  @Override
  public void run() {
    while (loop) {
      sell();//sell 方法是一共同步方法
    }
  }
}
//使用 Thread 方式
// new SellTicket01().start()
// new SellTicket01().start();
class SellTicket01 extends Thread {
  private static int ticketNum = 100;//让多个线程共享 ticketNum
  // public void m1() {
  // synchronized (this) {
  // System.out.println("hello");
  // }
  // }
  @Override
  public void run() {
    while (true) {
      if (ticketNum <= 0) {
        System.out.println("售票结束...");
        break;
      }
      //休眠 50 毫秒, 模拟
      try {
        Thread.sleep(50);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      System.out.println("窗口 " + Thread.currentThread().getName() + " 售出一张票" + " 剩余票数=" + (--ticketNum));
    }
  }
}
//实现接口方式
class SellTicket02 implements Runnable {
  private int ticketNum = 100;//让多个线程共享 ticketNum
  @Override
  public void run() {
    while (true) {
      if (ticketNum <= 0) {
        System.out.println("售票结束...");
        break;
      }
      //休眠 50 毫秒, 模拟
      try {
        Thread.sleep(50);
      } catch (InterruptedException e) {
        e.printStackTrace();
      }
      System.out.println("窗口 " + Thread.currentThread().getName() + " 售出一张票" + " 剩余票数=" + (--ticketNum));//1 - 0 - -1 - -2
    }
  }
}
```



> 注意事项和细节
>
> 1. 同步方法如果没有使用static修饰：默认锁对象为this
>
> 2. 如果方法使用static修饰，默认锁对象：当前类.class
> 3. 实现的落地步骤：
> • 需要先分析上锁的代码
> • 选择同步代码块或同步方法
> • 要求多个线程的锁对象为同一个即可！



# 死锁

## 死锁模拟

//下面业务逻辑的分析
//1.如果flag 为T，台线程A 就会先得到/持有 01 对象锁，然后尝试去获取 02 对象锁
//2.如果线程A 得不到 o2 对象锁，就会Blocked
//3.如果fLag 为F，线程B就会先得到/持有 o2 对象锁，然后尝试去获取 o1 对象锁
//4.如果线程B 得不到 o1 对象锁，就会Blocked

```java
public class DeadLock_ {
  public static void main(String[] args) {
    //模拟死锁现象
    DeadLockDemo A = new DeadLockDemo (true);
    A.setName("A线程");
    DeadLockDemo B = new DeadLockDemo(false);
    B.setName("B线程");
    A.start();
    B.start() ;
  }
}

class DeadLockDemo extends Thread {
  static Object o1 = new Object();//保证多线程，共享一个对象，这里使用static
  static Object o2 = new Object();
  boolean flag;
  //下面业务逻辑的分析
  //1.如果flag 为T，线程A 就会先得到/持有 01 对象锁，然后尝试去获取 02 对象锁
  //2.如果线程A 得不到 o2 对象锁，就会Blocked
  //3.如果fLag 为F，线程B就会先得到/持有 o2 对象锁，然后尝试去获取 o1 对象锁
  //4.如果线程B 得不到 o1 对象锁，就会Blocked
  @override
  public void run(){
    if (flag) {
      synchronized(o1){//对象互斥锁，下面就是同步代码
        System.out.println(Thread.currentThread).getName() + "进入1");
        synchronized(o2){// 这里获得Li对象的监视权
          System.out.println(Thread.currentThread().getName() + "进入2");
        }
      }
    } else {
      synchronized (02) {
        System.out.println(Thread.currentThread() •getName() + "进入3");
        synchronized(o1){ // 这里获得li对象的监视权
          System.out.println(Thread.currentThread().getName () + "进入4");
        }
      }
    }
  }
}
```

## 释放锁

> - 下面操作会释放锁
>
> 1. 当前线程的同步方法、同步代码块执行结束
>    案例：上厕所，完事出来
> 2. 当前线程在同步代码块、同步方法中遇到break、return。
>     案例：没有正常的完事，经理叫他修改bug，不得已出来
> 3. 当前线程在同步代码块、同步方法中出现了未处理的Error或Exception，导致异常结束
>     案例：没有正常的完事，发现忘带纸，不得已出来
> 4. 当前线程在同步代码块、同步方法中执行了线程对象的 wait() 方法，当前线程暂停，并释放锁
>     案例：没有正常完事，觉得需要酝酿下，所以出来等会再进去
> 
> **总结: join释放锁 只释放 调用join()方法的对象 所持有的对象锁**
>
> - 下面操作不会释放锁
>
> 1. 线程执行同步代码块或同步方法时，程序调用Thread.sleep()、Thread.yield()方法暂停当前线程的执行，不会释放锁
>案例：上厕所，太困了，在坑位上眯了一会
> 2. 线程执行同步代码块时，其他线程调用了该线程的suspend()  方法将该线程挂起，该线程不会释放锁。
> 提示：应尽量避免使用suspend()和resume()来控制线程，方法不再推荐使用

# 补充 JConsole使用

终端输入jconsole命令。

# 补充Lock-ReentrantLock重入锁

# 补充 volatile关键字

