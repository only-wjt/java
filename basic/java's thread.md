---
title: java's thread 
tags: java,thread,basic
grammar_cjkRuby: true
---
[参考](https://www.cnblogs.com/duanxz/category/1026463.html)

## 进程和线程

### 概念

&emsp;&emsp;线程：是程序执行流的最小单元，是系统独立调度和分配CPU（独立运行）的基本单位。

&emsp;&emsp;进程：是资源分配的基本单位。一个进程包括多个线程。

### 区别：

&emsp;&emsp;线程与资源分配无关，它属于某一个进程，并与进程内的其他线程一起共享进程的资源。

&emsp;&emsp;`每个进程都有自己一套独立的资源（数据），供其内的所有线程共享。`

&emsp;&emsp;不论是大小，开销线程要更“轻量级”

&emsp;&emsp;`一个线程内的线程通信比进程之间的通信更快速，有效。（因为共享变量）`

### 多线程与多进程

&emsp;&emsp;多线程：同一时刻执行多个线程。如，用浏览器一边下载，一边听歌，一边看视频，一边看网页......

&emsp;&emsp;多进程：同时执行多个程序。如，同事运行YY，QQ，以及各种浏览器。

### 并发与并行

&emsp;&emsp;并发：当有多个线程在操作时,如果系统只有一个CPU,则它根本不可能真正同时进行一个以上的线程，它只能把CPU运行时间划分成若干个时间段,再将时间 段分配给各个线程执行，在一个时间段的线程代码运行时，其它线程处于挂起状。.这种方式我们称之为并发(Concurrent)。

&emsp;&emsp;并行：当系统有一个以上CPU时,则线程的操作有可能非并发。当一个CPU执行一个线程时，另一个CPU可以执行另一个线程，两个线程互不抢占CPU资源，可以同时进行，这种方式我们称之为并行(Parallel)。

&emsp;&emsp;强烈注意：多核，多cpu，多机是不同的概念。 

### 补充：

&emsp;&emsp;多内核是指在一枚处理器中集成两个或多个完整的计算引擎(内核)。

&emsp;&emsp;多核心cpu主要分原生多核和封装多核。

&emsp;&emsp;- 原生多核指的是真正意义上的多核，每个核心之间都是完全独立的，都拥有自己的前端总线，不会造成冲突，即使在高负载状况下，每个核心都能保证自己的性能不受太大的影响，通俗的说，原生多核的抗压能力强，但是需要先进的工艺，每扩展一个核心都需要很多的研发时间。

&emsp;&emsp;- 封装多核是只把多个核心直接封装在一起，和原生的比起来还是差了很多，而且后者成本比较高，优点在于多核心的发展要比原生快的多。

&emsp;&emsp;多个处理机及存储器模块构成的并行处理机被称为多处理机系统(multiprocessor system)，简称多处理机。多机系统是将多个VLSI（超大规模集成电路）工艺集成的微处理机芯片结合在一起，由多个处理机并行工作以达到所需的高速度的，因此多机系统实际上是并行处理技术和VLSI技术相结合的产物。

## Java 多线程编程
&emsp;&emsp;Java 给多线程编程提供了内置的支持。 `一条线程指的是进程中一个单一顺序的控制流`，一个进程中可以并发多个线程，每条线程并行执行不同的任务。
多线程是多任务的一种特别的形式，但多线程使用了更小的资源开销。
&emsp;&emsp;这里定义和线程相关的另一个术语 - 进程`：一个进程包括由操作系统分配的内存空间，`包含一个或多个线程。`一个线程不能独立的存在，它必须是进程的一部分`。一个进程一直运行，直到所有的非守护线程都结束运行后才能结束。
&emsp;&emsp;多线程能满足程序员编写高效率的程序来达到充分利用 CPU 的目的。

### 一个线程的生命周期
&emsp;&emsp;线程是一个动态执行的过程，它也有一个从产生到死亡的过程。

&emsp;&emsp;下图显示了一个线程完整的生命周期


![thread的生命周期](./images/javathread.png)


#### 新建状态:
&emsp;&emsp;使用 new 关键字和 Thread 类或其子类建立一个线程对象后，该线程对象就处于新建状态。它保持这个状态直到程序 start() 这个线程。

#### 就绪状态:
&emsp;&emsp;当线程对象调用了start()方法之后，该线程就进入就绪状态。就绪状态的线程处于就绪队列中，要等待JVM里线程调度器的调度。

#### 运行状态:
&emsp;&emsp;如果就绪状态的线程获取 CPU 资源，就可以执行 run()，此时线程便处于运行状态。处于运行状态的线程最为复杂，它可以变为阻塞状态、就绪状态和死亡状态。

#### 阻塞状态:
&emsp;&emsp;如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种：

##### 等待阻塞：
&emsp;&emsp;运行状态中的线程执行 wait() 方法，使线程进入到等待阻塞状态。

##### 同步阻塞：
&emsp;&emsp;线程在获取 synchronized 同步锁失败(因为同步锁被其他线程占用)。

##### 其他阻塞：
&emsp;&emsp;通过调用线程的 sleep() 或 join() 发出了 I/O 请求时，线程就会进入到阻塞状态。当sleep() 状态超时，join() 等待线程终止或超时，或者 I/O 处理完毕，线程重新转入就绪状态。

##### 死亡状态:
&emsp;&emsp;一个运行状态的线程完成任务或者其他终止条件发生时，该线程就切换到终止状态。

### java线程中的关键方法sleep、wait、yield、join

&emsp;&emsp;Java中的多线程是一种抢占式的机制而不是分时机制。线程主要有以下几种状态：新建，就绪，运行，阻塞，死亡。`抢占式机制指的是有多个线程处于就绪状态，但是只有一个线程在运行。`

![thread方法](./images/threadMethod.png)

#### sleep()方法

　　在指定时间内让当前正在执行的线程暂停执行，但不会释放“锁标志”。不推荐使用。

　　sleep()使当前线程进入阻塞状态，在指定时间内不会执行。

#### wait()方法

　　在其他线程调用对象的notify或notifyAll方法前，导致当前线程等待。线程会释放掉它所占有的“锁标志”，从而使别的线程有机会抢占该锁。

　　当前线程必须拥有当前对象锁。如果当前线程不是此锁的拥有者，会抛出IllegalMonitorStateException异常。

　　唤醒当前对象锁的等待线程使用notify或notifyAll方法，也必须拥有相同的对象锁，否则也会抛出IllegalMonitorStateException异常。

　　wait() 和notify()必须在synchronized函数或synchronized　block中进行调用。如果在non-synchronized函数或non-synchronized　block中进行调用，虽然能编译通过，但在运行时会发生 IllegalMonitorStateException的异常。

#### yield方法

　　暂停当前正在执行的线程对象。

　　yield()只是使当前线程重新回到可执行状态，所以执行yield()的线程有可能在进入到可执行状态后马上又被执行。

　　yield()只能使同优先级或更高优先级的线程有执行的机会。

#### join方法

　　等待该线程终止。

　　等待调用join方法的线程结束，再继续执行。如：t.join();//主要用于等待t线程运行结束，若无此句，main则会执行完毕，导致结果不可预测。

### 线程同步

&emsp;&emsp;一个线程结束的标志是：run()方法结束。
&emsp;&emsp;一个机锁被释放的标志是：synchronized块或方法结束。
&emsp;&emsp;当有多个线程访问共享数据的时候，就需要对线程进行同步。线程同步相关的方法中的几个主要方法的按照所属可以分成：
&emsp;&emsp;`Thread类的方法：sleep(),yield(),join()等`,join()底层还是用wait()实现的
&emsp;&emsp;`Object的方法：wait()和notify()、notifyAll()等`
&emsp;&emsp;Object中的对象头存放的锁信息在控制同步访问时使用。[见《java对象在内存中的结构（HotSpot虚拟机）》](http://www.cnblogs.com/duanxz/p/4967042.html)和[《Synchronized之二：synchronized的实现原理》](http://www.cnblogs.com/duanxz/p/4745871.html)

&emsp;&emsp;Wait()方法和notify()方法：当一个线程执行到wait()方法时，它就进入到一个和该对象相关的等待池中，同时失去了对象的机锁。当它被一个notify()方法唤醒时，等待池中的线程就被放到了锁池中。该线程从锁池中获得机锁，然后回到wait()前的中断现场。
 
&emsp;&emsp;Thread类中的方法：
&emsp;&emsp;由于sleep()方法是Thread类的方法，因此它不能改变对象的机锁。所以当在一个Synchronized方法中调用sleep()时，线程虽然休眠了，但是对象的机锁没有被释放，其他线程仍然无法访问这个对象。而wait()方法则会在线程休眠的同时释放掉机锁，其他线程可以访问该对象。
&emsp;&emsp;Yield()方法：`是停止当前线程，让同等优先权的线程运行。如果没有同等优先权的线程，那么Yield()方法将不会起作用。`
&emsp;&emsp;join()方法：`是由一个线程调用另一个线程，`调用线程等待被调用线程终止。
 
&emsp;&emsp;sleep()与wait()的共同点及不同点：
&emsp;&emsp;共同点： 他们都是在多线程的环境下，都可以在程序的调用处阻塞指定的毫秒数，并返回。
&emsp;&emsp;不同点： Thread.sleep(long)可以不在synchronized的块下调用，而且使用Thread.sleep()不会丢失当前线程对任何对象的同步锁(monitor);` object.wait(long)必须在synchronized的块下来使用，`调用了之后失去对object的monitor, 这样做的好处是它不影响其它的线程对object进行操作。

&emsp;&emsp;举个java.util.Timer的例子来说明。

````
private void main Loop() {
        while (true) {
        ....
        synchronized(queue) {
        .....
        if (!taskFired) // Task hasn't yet fired; wait
               queue.wait(executionTime - currentTime);
        }
}
````

&emsp;&emsp;在这里为什么要使用queue.wait()，而不是Thread.sleep(), 是因为暂时放弃queue的对象锁，可以让允许其它的线程执行一些同步操作。如：

````
private void sched(TimerTask task, long time, long period) {
          synchronized(queue) {
              ...
              queue.add(task);
          }
}
````

&emsp;&emsp;但是正如上篇文章讲到的，使用queue.wait(long)的前提条件是sched()动作执行的时间很短，否则如果很长，那么queue.wait()不能够按时醒来。



## Thread Join()的用法
 
### join用法

 
&emsp;&emsp;1.`join方法定义在Thread类中，则调用者必须是一个线程`

 
&emsp;&emsp;例如：

 ````
Thread t = new CustomThread();//这里一般是自定义的线程类
t.start();//线程起动
t.join();//此处会抛出InterruptedException异常
````
 
&emsp;&emsp;2.上面的两行代码也是在一个线程里面执行的

 
&emsp;&emsp;以上出现了两个线程，一个是我们自定义的线程类，`我们实现了run方法，做一些我们需要的工作`；`另外一个线程，生成我们自定义线程类的对象`，然后执行

 ````
customThread.start();
customThread.join();
````
 
&emsp;&emsp;在这种情况下，两个线程的关系是一个线程`由另外一个线程生成并起动，`所以我们暂且认为第一个线程叫做“子线程”，另外一个线程叫做“主线程”。

 
### 为什么要用join()方法

 
&emsp;&emsp;主线程生成并启动了子线程，而子线程里要进行大量的耗时的运算(这里可以借鉴下线程的作用)，当主线程处理完其他的事务后，需要用到子线程的处理结果，这个时候就要用到join();方法了。

 
### join方法的作用

 
&emsp;&emsp;在网上看到有人说“将两个线程合并”。这样解释我觉得理解起来还更麻烦。不如就借鉴下API里的说法：

 
&emsp;&emsp;`等待该线程终止。`

 
&emsp;&emsp;解释一下，`是主线程等待子线程的终止。也就是说主线程的代码块中，如果碰到了t.join()方法，此时主线程需要等待（阻塞），等待子线程结束了(Waits for this thread to die.),才能继续执行t.join()之后的代码块。`

 
### 示例

&emsp;&emsp;看了一下源代码，发现new Thread()，实际上底层也是实现了runable接口

````
class BThread extends Thread {
    public BThread() {
        super("[BThread] Thread");
    };
    public void run() {
        String threadName = Thread.currentThread().getName();
        System.out.println(threadName + " start.");
        try {
            for (int i = 0; i < 5; i++) {
                System.out.println(threadName + " loop at " + i);
                Thread.sleep(1000);
            }
            System.out.println(threadName + " end.");
        } catch (Exception e) {
            System.out.println("Exception from " + threadName + ".run");
        }
    }
}
class AThread extends Thread {
    BThread bt;
    public AThread(BThread bt) {
        super("[AThread] Thread");
        this.bt = bt;
    }
    public void run() {
        String threadName = Thread.currentThread().getName();
        System.out.println(threadName + " start.");
        try {
            bt.join();
            System.out.println(threadName + " end.");
        } catch (Exception e) {
            System.out.println("Exception from " + threadName + ".run");
        }
    }
}
public class TestDemo {
    public static void main(String[] args) {
        String threadName = Thread.currentThread().getName();
        System.out.println(threadName + " start.");
        BThread bt = new BThread();
        AThread at = new AThread(bt);
        try {
            bt.start();
            Thread.sleep(2000);
            at.start();
            at.join();
        } catch (Exception e) {
            System.out.println("Exception from main");
        }
        System.out.println(threadName + " end!");
    }
}
````
结果：

````
main start.    //主线程起动，因为调用了at.join()，要等到at结束了，此线程才能向下执行。 
[BThread] Thread start. 
[BThread] Thread loop at 0 
[BThread] Thread loop at 1 
[AThread] Thread start.    //线程at启动，因为调用bt.join()，等到bt结束了才向下执行。 
[BThread] Thread loop at 2 
[BThread] Thread loop at 3 
[BThread] Thread loop at 4 
[BThread] Thread end. 
[AThread] Thread end.    // 线程AThread在bt.join();阻塞处启动，向下继续执行的结果 
main end!      //线程AThread结束，此线程在at.join();阻塞处启动，向下继续执行的结果。
````

修改一下代码：

````
public class TestDemo {
    public static void main(String[] args) {
        String threadName = Thread.currentThread().getName();
        System.out.println(threadName + " start.");
        BThread bt = new BThread();
        AThread at = new AThread(bt);
        try {
            bt.start();
            Thread.sleep(2000);
            at.start();
            //at.join(); //在此处注释掉对join()的调用
        } catch (Exception e) {
            System.out.println("Exception from main");
        }
        System.out.println(threadName + " end!");
    }
}
````

结果：

````
main start.    // 主线程起动，因为Thread.sleep(2000)，主线程没有马上结束;

[BThread] Thread start.    //线程BThread起动
[BThread] Thread loop at 0
[BThread] Thread loop at 1
main end!   // 在sleep两秒后主线程结束，AThread执行的bt.join();并不会影响到主线程。
[AThread] Thread start.    //线程at起动，因为调用了bt.join()，等到bt结束了，此线程才向下执行。
[BThread] Thread loop at 2
[BThread] Thread loop at 3
[BThread] Thread loop at 4
[BThread] Thread end.    //线程BThread结束了
[AThread] Thread end.    // 线程AThread在bt.join();阻塞处起动，向下继续执行的结果
````

### 从源码看join()方法
在AThread的run方法里，执行了bt.join();，进入看一下它的JDK源码：

````
public final void join() throws InterruptedException {
    join(0);
}
````

````
  public final synchronized void join(long millis)
    throws InterruptedException {
        long base = System.currentTimeMillis();
        long now = 0;

        if (millis < 0) {
            throw new IllegalArgumentException("timeout value is negative");
        }

        if (millis == 0) {
            while (isAlive()) {
                wait(0);
            }
        } else {
            while (isAlive()) {
                long delay = millis - now;
                if (delay <= 0) {
                    break;
                }
                wait(delay);
                now = System.currentTimeMillis() - base;
            }
        }
    }
````

&emsp;&emsp;Join方法实现是通过wait（小提示：Object 提供的方法）。 当main线程调用t.join时候，main线程会获得线程对象t的锁（wait 意味着拿到该对象的锁),调用该对象的wait(等待时间)，直到该对象唤醒main线程 ，比如退出后。这就意味着main 线程调用t.join时，必须能够拿到线程t对象的锁。


## Object里的wait、notify、notifyAll的使用方法

&emsp;&emsp;wait()、notify()、notifyAll()是三个定义在Object类里的方法，可以用来控制线程的状态

````
public final native void notify();

public final native void notifyAll();

public final native void wait(long l)
        throws InterruptedException;

    public final void wait(long l, int i)
        throws InterruptedException
    {
        if(l < 0L)
            throw new IllegalArgumentException("timeout value is negative");
        if(i < 0 || i > 999999)
            throw new IllegalArgumentException("nanosecond timeout value out of range");
        if(i > 0)
            l++;
        wait(l);
    }

    public final void wait()
        throws InterruptedException
    {
        wait(0L);
    }
````

&emsp;&emsp;这三个方法最终调用的都是jvm级的final native方法。随着jvm运行平台的不同可能有些许差异。

&emsp;&emsp;如果对象调用了wait方法就会使持有该对象的线程`把该对象的控制权交出去，然后处于等待状态。`
&emsp;&emsp;如果对象调用了notify方法就会`通知某个正在等待这个对象的控制权的线程可以继续运行。`
&emsp;&emsp;如果对象调用了notifyAll方法就会通知`所有等待这个对象控制权的线程继续运行。`
&emsp;&emsp;其中wait方法有三个over load方法：

````
wait()

wait(long)

wait(long,int)

wait方法通过参数可以指定等待的时长。如果没有指定参数，默认一直等待直到被通知。
````

&emsp;&emsp;以下是一个演示代码，以最简洁的方式说明复杂的问题：

&emsp;&emsp;简要说明下：

&emsp;&emsp;NotifyThread是用来模拟3秒钟后通知其他等待状态的线程的线程类；

&emsp;&emsp;WaitThread是用来模拟等待的线程类；

&emsp;&emsp;等待的中间对象是flag，一个String对象；

&emsp;&emsp;main方法中同时启动一个Notify线程和三个wait线程；

````
package com.dxz.synchronizeddemo;

public class NotifyTest {
    private String flag = "true";

    class NotifyThread extends Thread {
        public NotifyThread(String name) {
            super(name);
        }

        public void run() {
            try {
                sleep(3000);// 推迟3秒钟通知
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            flag = "false";
            flag.notify();
        }
    };

    class WaitThread extends Thread {
        public WaitThread(String name) {
            super(name);
        }

        public void run() {
            System.out.println(getName() +  "  flag:" + flag);
            while (!flag.equals("false")) {
                System.out.println(getName() + " begin waiting!");
                long waitTime = System.currentTimeMillis();
                try {
                    flag.wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                waitTime = System.currentTimeMillis() - waitTime;
                System.out.println("wait time :" + waitTime);
            }
            System.out.println(getName() + " end waiting!");

        }
    }
````

````
    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main Thread Run!");
        NotifyTest test = new NotifyTest();
        NotifyThread notifyThread = test.new NotifyThread("notify01");
        WaitThread waitThread01 = test.new WaitThread("waiter01");
        WaitThread waitThread02 = test.new WaitThread("waiter02");
        WaitThread waitThread03 = test.new WaitThread("waiter03");
        notifyThread.start();
        waitThread01.start();
        waitThread02.start();
        waitThread03.start();
    }
}
````
&emsp;&emsp;结果：

````
Main Thread Run!
Exception in thread "waiter03" waiter03  flag:true
waiter02  flag:true
waiter03 begin waiting!
waiter01  flag:true
waiter02 begin waiting!
waiter01 begin waiting!
Exception in thread "waiter02" Exception in thread "waiter01" java.lang.IllegalMonitorStateException
    at java.lang.Object.wait(Native Method)
    at java.lang.Object.wait(Object.java:502)
    at com.dxz.synchronizeddemo.NotifyTest$WaitThread.run(NotifyTest.java:34)
java.lang.IllegalMonitorStateException
    at java.lang.Object.wait(Native Method)
    at java.lang.Object.wait(Object.java:502)
    at com.dxz.synchronizeddemo.NotifyTest$WaitThread.run(NotifyTest.java:34)
java.lang.IllegalMonitorStateException
    at java.lang.Object.wait(Native Method)
    at java.lang.Object.wait(Object.java:502)
    at com.dxz.synchronizeddemo.NotifyTest$WaitThread.run(NotifyTest.java:34)
Exception in thread "notify01" java.lang.IllegalMonitorStateException
    at java.lang.Object.notify(Native Method)
    at com.dxz.synchronizeddemo.NotifyTest$NotifyThread.run(NotifyTest.java:19)
````

&emsp;&emsp;在wait和notify的地方都报错java.lang.IllegalMonitorStateException，前面也讲过，`wait和notify方法一定要在synchronized里面，`更具体点说有：

&emsp;&emsp;任何一个时刻，对象的控制权（monitor）只能被一个线程拥有。
&emsp;&emsp;无论是执行对象的wait、notify还是notifyAll方法，必须保证当前运行的线程取得了该对象的控制权（monitor）
&emsp;&emsp;如果在没有控制权的线程里执行对象的以上三种方法，就会报java.lang.IllegalMonitorStateException异常。
&emsp;&emsp;JVM基于多线程，默认情况下不能保证运行时线程的时序性
&emsp;&emsp;基于以上几点事实，我们需要确保让线程拥有对象的控制权。

&emsp;&emsp;也就是说在waitThread中执行wait方法时，要保证waitThread对flag有控制权；

&emsp;&emsp;在notifyThread中执行notify方法时，要保证notifyThread对flag有控制权。

&emsp;&emsp;线程取得控制权的方法有三：见《Synchronized之一：基本使用》

&emsp;&emsp;同步的实例方法（锁用的是其实例对象本身。所有的非静态同步方法执行需要顺序执行，即不能并行执行。）
&emsp;&emsp;同步的静态方法（锁用的是其类对象本身。所有的静态同步方法执行需要顺序执行，即不能并行执行。）
&emsp;&emsp;实例方法中的同步块（锁是自己指定的，但不能是引用性对象及null对象）
&emsp;&emsp;静态方法中的同步块（锁是自己指定的，但不能是引用性对象及null对象）
&emsp;&emsp;我们用第三种方法来做说明：
&emsp;&emsp;将以上notify和wait方法包在同步块中

````
//nofity放入synchronized中
            synchronized (flag) {
                flag = "false";
                flag.notify();
            }
//wait放入synchronized中
            synchronized (flag) {
                while (!flag.equals("false")) {
                    System.out.println(getName() + " begin waiting!");
                    long waitTime = System.currentTimeMillis();
                    try {
                        flag.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    waitTime = System.currentTimeMillis() - waitTime;
                    System.out.println("wait time :" + waitTime);
                }
            }
````

&emsp;&emsp;再运行的结果：

````
Main Thread Run!
waiter02 flag:true
waiter01 flag:true
waiter03 flag:true
waiter02 begin waiting!
waiter03 begin waiting!
waiter01 begin waiting!
Exception in thread "notify01" java.lang.IllegalMonitorStateException
at java.lang.Object.notify(Native Method)
at com.dxz.synchronizeddemo.NotifyTest$NotifyThread.run(NotifyTest.java:20)
````

&emsp;&emsp;在flag.notify();的地方还是会报错java.lang.IllegalMonitorStateException。这时的异常是由于在针对flag对象同步块中，更改了flag对象的状态所导致的。如下：

````
flag="false";  //改变的对象的内容
flag.notify();
````

&emsp;&emsp;对在同步块中对flag进行了赋值操作，使得flag引用的对象改变，这时候再调用notify方法时，因为没有控制权所以抛出异常。

&emsp;&emsp;我们可以改进一下，将flag改成一个JavaBean，然后更改它的属性不会影响到flag的引用。

&emsp;&emsp;我们这里改成数组来试试，也可以达到同样的效果：

````
private String flag[] = {"true"}; 
　　　　　　　synchronized (flag) {  
                flag[0] = "false";
                flag.notify();
            }
			
			
             synchronized (flag) { 
                System.out.println(getName() +  "  flag:" + flag);
                while (!flag[0].equals("false")) {
                    System.out.println(getName() + " begin waiting!");
                    long waitTime = System.currentTimeMillis();
                    try {
                        flag.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    waitTime = System.currentTimeMillis() - waitTime;
                    System.out.println("wait time :" + waitTime);
                }
                System.out.println(getName() + " end waiting!");
            }
````

&emsp;&emsp;这时候再运行，不再报异常，但是线程没有结束是吧，没错，还有线程堵塞，处于wait状态。

&emsp;&emsp;原因很简单，我们有三个wait线程，只有一个notify线程，notify线程运行notify方法的时候，是随机通知一个正在等待的线程，所以，现在应该还有两个线程在waiting。

&emsp;&emsp;我们只需要将NotifyThread线程类中的flag.notify()方法改成notifyAll()就可以了。notifyAll方法会通知所有正在等待对象控制权的线程。

&emsp;&emsp;最终完成版如下：

````
package com.dxz.synchronizeddemo;

public class NotifyTest2 {
    private String flag[] = {"true"};  

    class NotifyThread extends Thread {
        public NotifyThread(String name) {
            super(name);
        }

        public void run() {
            try {
                sleep(3000);// 推迟3秒钟通知
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (flag) {  
                flag[0] = "false";
                flag.notify();
            }
        }
    };

    class WaitThread extends Thread {
        public WaitThread(String name) {
            super(name);
        }

        public void run() {
            synchronized (flag) { 
                System.out.println(getName() +  "  flag:" + flag);
                while (!flag[0].equals("false")) {
                    System.out.println(getName() + " begin waiting!");
                    long waitTime = System.currentTimeMillis();
                    try {
                        flag.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    waitTime = System.currentTimeMillis() - waitTime;
                    System.out.println("wait time :" + waitTime);
                }
                System.out.println(getName() + " end waiting!");
            }

        }
    }

    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main Thread Run!");
        NotifyTest2 test = new NotifyTest2();
        NotifyThread notifyThread = test.new NotifyThread("notify01");
        WaitThread waitThread01 = test.new WaitThread("waiter01");
        WaitThread waitThread02 = test.new WaitThread("waiter02");
        WaitThread waitThread03 = test.new WaitThread("waiter03");
        notifyThread.start();
        waitThread01.start();
        waitThread02.start();
        waitThread03.start();
    }
}
````
 
&emsp;&emsp;notify()和notifyAll()的本质区别
&emsp;&emsp;notify()和notifyAll()都是Object对象用于通知处在等待该对象的线程的方法。两者的最大区别在于：

&emsp;&emsp;`notifyAll使所有原来在该对象上等待被notify的所有线程统统退出wait的状态，变成等待该对象上的锁，一旦该对象被解锁，他们就会去竞争。`
&emsp;&emsp;notify则文明得多，它只是选择一个wait状态线程进行通知，并使它获得该对象上的锁，但不惊动其他同样在等待被该对象notify的线程们，当第一个线程运行完毕以后释放对象上的锁此时如果该对象没有再次使用notify语句，则即便该对象已经空闲，其他wait状态等待的线程由于没有得到该对象的通知，继续处在wait状态，直到这个对象发出一个notify或notifyAll，它们等待的是被notify或notifyAll，而不是锁。

&emsp;&emsp;下面是一个很好的例子：

````
package com.dxz.synchronizeddemo;

public class NotifyTest3 {
    private String flag[] = { "true" };

    class NotifyThread extends Thread {
        public NotifyThread(String name) {
            super(name);
        }

        public void run() {
            try {
                sleep(3000);// 推迟3秒钟通知
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            synchronized (flag) {
                flag[0] = "false";
                flag.notifyAll();
            }
        }
    };

    class WaitThread extends Thread {
        public WaitThread(String name) {
            super(name);
        }

        public void run() {
            System.out.println(getName() + "  flag:" + flag);
            synchronized (flag) {
                System.out.println(getName() + "  flag:" + flag);
                while (!flag[0].equals("false")) {
                    System.out.println(getName() + " begin waiting!");
                    long waitTime = System.currentTimeMillis();
                    try {
                        flag.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    waitTime = System.currentTimeMillis() - waitTime;
                    System.out.println("wait time :" + waitTime);
                }
                System.out.println(getName() + " end waiting!");
            }
            System.out.println(getName() + " end waiting!");

        }
    }

    public static void main(String[] args) throws InterruptedException {
        System.out.println("Main Thread Run!");
        NotifyTest3 test = new NotifyTest3();
        NotifyThread notifyThread = test.new NotifyThread("notify01");
        WaitThread waitThread01 = test.new WaitThread("waiter01");
        WaitThread waitThread02 = test.new WaitThread("waiter02");
        WaitThread waitThread03 = test.new WaitThread("waiter03");
        notifyThread.start();
        waitThread01.start();
        waitThread02.start();
        waitThread03.start();
    }
}
````

&emsp;&emsp;结果：

````
Main Thread Run!
waiter01  flag:[Ljava.lang.String;@584aceca
waiter01  flag:[Ljava.lang.String;@584aceca
waiter02  flag:[Ljava.lang.String;@584aceca
waiter01 begin waiting!
waiter02  flag:[Ljava.lang.String;@584aceca
waiter02 begin waiting!
waiter03  flag:[Ljava.lang.String;@584aceca
waiter03  flag:[Ljava.lang.String;@584aceca
waiter03 begin waiting!
wait time :3000
waiter03 end waiting!
waiter03 end waiting!
wait time :3001
waiter02 end waiting!
waiter02 end waiting!
wait time :3001
waiter01 end waiting!
waiter01 end waiting!
````


## 线程的优先级

&emsp;&emsp;Java线程可以有优先级的设定，高优先级的线程比低优先级的线程有更高的几率得到执行（不完全正确，请参考下面的“线程优先级的问题“）。
&emsp;&emsp;记住当线程的优先级没有指定时，所有线程都携带普通优先级。
&emsp;&emsp;优先级可以用从1到10的范围指定。10表示最高优先级，1表示最低优先级，5是普通优先级。
&emsp;&emsp;记住优先级最高的线程在执行时被给予优先。但是不能保证线程在启动时就进入运行状态。
&emsp;&emsp;与在线程池中等待运行机会的线程相比，当前正在运行的线程可能总是拥有更高的优先级。
&emsp;&emsp;由调度程序决定哪一个线程被执行。
&emsp;&emsp;t.setPriority()用来设定线程的优先级。
&emsp;&emsp;记住在线程开始方法被调用之前，线程的优先级应该被设定。
&emsp;&emsp;你可以使用常量，如MIN_PRIORITY,MAX_PRIORITY，NORM_PRIORITY来设定优先级

&emsp;&emsp;优先级的取值
&emsp;&emsp;Java线程的优先级是一个整数，其取值范围是1 （Thread.MIN_PRIORITY ） - 10 （Thread.MAX_PRIORITY ）。

&emsp;&emsp;Thread源代码里对NORM_PRIORITY （数值为5） 的注释是“线程默认的优先级”

````
    public static final int MIN_PRIORITY = 1;
    public static final int NORM_PRIORITY = 5;
    public static final int MAX_PRIORITY = 10;
````
	
&emsp;&emsp;其实不然。默认的优先级是父线程的优先级。在init方法里，

````
Thread parent = currentThread();  
this.priority = parent.getPriority();  
````

&emsp;&emsp;或许这么解释是因为Java程序的主线程(main方法)的优先级默认是为NORM_PRIORITY，这样不主动设定优先级的，后续创建的线程的优先级也都是NORM_PRIORITY了。

````
public static void main(String[] args) {  
    System.out.println(Thread.currentThread().getPriority());  
}  
````

&emsp;&emsp;其执行结果是5。

&emsp;&emsp;设置优先级
&emsp;&emsp;可以通过setPriority方法（final的，不能被子类重载）更改优先级。优先级不能超出1-10的取值范围，否则抛出IllegalArgumentException。另外如果该线程已经属于一个线程组（ThreadGroup），该线程的优先级不能超过该线程组的优先级：

````
public final void setPriority(int i)
    {
        checkAccess();
        if(i > 10 || i < 1)
            throw new IllegalArgumentException();
        ThreadGroup threadgroup;
        if((threadgroup = getThreadGroup()) != null)
        {
            if(i > threadgroup.getMaxPriority())
                i = threadgroup.getMaxPriority();
            setPriority0(priority = i);
        }
    }
````

&emsp;&emsp; 其中setPriority0是一个本地方法。

````
    private native void setPriority0(int i);
````

&emsp;&emsp;线程组的最大优先级,我们可以设定线程组的最大优先级，当创建属于该线程组的线程时该线程的优先级不能超过这个数。

 
&emsp;&emsp;线程组最大优先级的设定：

&emsp;&emsp;系统线程组的最大优先级默认为Thread.MAX_PRIORITY
&emsp;&emsp;创建线程组的时候其最大优先级默认为父线程组（如果未指定父线程组，则其父线程组默认为当前线程所属线程组）的最大优先级
&emsp;&emsp;可以通过setMaxPriority更改最大优先级，但无法超过父线程组的最大优先级
&emsp;&emsp;setMaxPriority的问题：

&emsp;&emsp;`该方法只能更改本线程组及其子线程组（递归）的最大优先级。但不能影响已经创建的直接或间接属于该线程组的线程的优先级，`也就是说，`即使目前有一个子线程的优先级比新设定的线程组优先级大，也不会更改该子线程的优先级。只有当试图改变子线程的优先级或者创建新的子线程的时候，线程组的最大优先级才起作用。`

&emsp;&emsp;线程优先级的问题
&emsp;&emsp;以下内容摘抄、翻译自JAVAMEX -> Java threading introduction -> Thread priorioties
&emsp;&emsp;对于线程优先级，我们需要注意：

> &emsp;&emsp;Thread.setPriority()可能根本不做任何事情，这跟你的操作系统和虚拟机版本有关
> &emsp;&emsp;线程优先级对于不同的线程调度器可能有不同的含义，可能并不是你直观的推测。特别地，优先级并不一定是指CPU的分享。在UNIX系统，优先级或多或少可以认为是CPU的分配，但Windows不是这样
> &emsp;&emsp;线程的优先级通常是全局的和局部的优先级设定的组合。Java的setPriority()方法只应用于局部的优先级。换句话说，你不能在整个可能的范围内设定优先级。（这通常是一种保护的方式，你大概不希望鼠标指针的线程或者处理音频数据的线程被其它随机的用户线程所抢占）
> &emsp;&emsp;不同的系统有不同的线程优先级的取值范围，但是Java定义了10个级别（1-10）。这样就有可能出现几个线程在一个操作系统里有不同的优先级，在另外一个操作系统里却有相同的优先级（并因此可能有意想不到的行为）
> &emsp;&emsp;操作系统可能（并通常这么做）根据线程的优先级给线程添加一些专有的行为（例如”only give a quantum boost if the priority is below X“）。这里再重复一次，优先级的定义有部分在不同系统间有差别。
> &emsp;&emsp;大多数操作系统的线程调度器实际上执行的是在战略的角度上对线程的优先级做临时操作（例如当一个线程接收到它所等待的一个事件或者I/O），通常操作系统知道最多，试图手工控制优先级可能只会干扰这个系统。
> &emsp;&emsp;你的应用程序通常不知道有哪些其它进程运行的线程，所以对于整个系统来说，变更一个线程的优先级所带来的影响是难于预测的。例如你可能发现，你有一个预期为偶尔在后台运行的低优先级的线程几乎没有运行，原因是一个病毒监控程序在一个稍微高一点的优先级（但仍然低于普通的优先级）上运行，并且无法预计你程序的性能，它会根据你的客户使用的防病毒程序不同而不同。

&emsp;&emsp;你可以参考Java优先级与各操作系统优先级之间的对应关系

&emsp;&emsp;实际编码注意事项:不要假定高优先级的线程一定先于低优先级的线程执行，不要有逻辑依赖于线程优先级，否则可能产生意外结果


## 线程的创建方法

&emsp;&emsp;&emsp;&emsp;Java使用Thread类代表线程，所有的线程对象都必须是Thread类或其子类的实例。Java可以用四种方式来创建线程，如下所示：

&emsp;&emsp;1）继承Thread类创建线程

&emsp;&emsp;2）实现Runnable接口创建线程

&emsp;&emsp;3）使用Callable和Future创建线程

&emsp;&emsp;4）使用线程池例如用Executor框架

&emsp;&emsp;下面让我们分别来看看这四种创建线程的方法。

 
### 继承Thread类

&emsp;&emsp;通过继承Thread类来创建并启动多线程的一般步骤如下

&emsp;&emsp;1)定义Thread类的子类，`并重写该类的run()方法，`该方法的方法体就是线程需要完成的任务，`run()方法也称为线程执行体。`

&emsp;&emsp;2)创建Thread子类的实例，也就是创建了线程对象

&emsp;&emsp;3)启动线程，即调用线程的start()方法

````
public class MyThread extends Thread{//继承Thread类

　　public void run(){

　　//重写run方法

　　}

}

public class Main {

　　public static void main(String[] args){

　　　　new MyThread().start();//创建并启动线程

　　}

}
````

### 实现Runnable接口创建线程

&emsp;&emsp;通过实现Runnable接口创建并启动线程一般步骤如下：

&emsp;&emsp;1)定义Runnable接口的实现类，`一样要重写run()方法，这个run（）方法和Thread中的run()方法一样是线程的执行体`

&emsp;&emsp;2)创建Runnable实现类的实例，并用这个实例作为Thread的target来创建Thread对象，这个Thread对象才是真正的线程对象

&emsp;&emsp;3)第三步依然是通过调用线程对象的start()方法来启动线程

````
public class MyThread2 implements Runnable {//实现Runnable接口

　　public void run(){

　　//重写run方法

　　}

}

public class Main {

　　public static void main(String[] args){

　　　　//创建并启动线程

　　　　MyThread2 myThread=new MyThread2();

　　　　Thread thread=new Thread(myThread);//这个才是真正的线程对象

　　　　thread.start();

　　　　//或者    new Thread(new MyThread2()).start();

　　}

}
````

### 使用Callable和Future创建线程

&emsp;&emsp;和Runnable接口不一样，`Callable接口提供了一个call（）方法作为线程执行体，``call()方法比run()方法功能要强大。`

&emsp;&emsp;1) call()方法可以有返回值

&emsp;&emsp;2) call()方法可以声明抛出异常

&emsp;&emsp;Java5`提供了Future接口来代表Callable接口里call()方法的返回值，`并且为Future接口`提供了一个实现类FutureTask，`这个实现类既实现了Future接口，还实现了Runnable接口，因此可以作为Thread类的target。在Future接口里定义了几个公共方法来控制它关联的Callable任务。

>boolean cancel(boolean mayInterruptIfRunning)：视图取消该Future里面关联的Callable任务

>V get()：返回Callable里call（）方法的返回值，调用这个方法会导致程序阻塞，`必须等到子线程结束后才会得到返回值`

>V get(long timeout,TimeUnit unit)：返回Callable里call（）方法的返回值，最多阻塞timeout时间，经过指定时间没有返回抛出TimeoutException

>boolean isDone()：若Callable任务完成，返回True

>boolean isCancelled()：如果在Callable任务正常完成前被取消，返回True


&emsp;&emsp;介绍了相关的概念之后，创建并启动有返回值的线程的步骤如下：

&emsp;&emsp;1) 创建Callable接口的实现类，并实现call()方法，然后创建该实现类的实例（从java8开始可以直接使用Lambda表达式创建Callable对象）。

&emsp;&emsp;2) 使用FutureTask类来包装Callable对象，该FutureTask对象封装了Callable对象的call()方法的返回值

&emsp;&emsp;3) 使用FutureTask对象作为Thread对象的target创建并启动线程（因为FutureTask实现了Runnable接口）

&emsp;&emsp;4) 调用FutureTask对象的get()方法来获得子线程执行结束后的返回值


````
public class Main {

　　public static void main(String[] args){

　　　MyThread3 th=new MyThread3();

　　　//使用Lambda表达式创建Callable对象

　　   //使用FutureTask类来包装Callable对象

　　　FutureTask<Integer> future=new FutureTask<Integer>(

　　　　(Callable<Integer>)()->{

　　　　　　return 5;

　　　　}

　　  );

　　　new Thread(task,"有返回值的线程").start();//实质上还是以Callable对象来创建并启动线程

　　  try{

　　　　System.out.println("子线程的返回值："+future.get());//get()方法会阻塞，直到子线程执行结束才返回

 　　 }catch(Exception e){

　　　　ex.printStackTrace();

　　　}

　　}

}
````


### 使用线程池例如用Executor框架

&emsp;&emsp;1.5后引入的Executor框架的最大优点是把任务的提交和执行解耦。要执行任务的人只需把Task描述清楚，然后提交即可。这个Task是怎么被执行的，被谁执行的，什么时候执行的，提交的人就不用关心了。具体点讲，提交一个Callable对象给ExecutorService（如最常用的线程池ThreadPoolExecutor），将得到一个Future对象，调用Future对象的get方法等待执行结果就好了。Executor框架的内部使用了线程池机制，它在java.util.cocurrent 包下，通过该框架来控制线程的启动、执行和关闭，可以简化并发编程的操作。因此，在Java 5之后，通过Executor来启动线程比使用Thread的start方法更好，除了更易管理，效率更好（用线程池实现，节约开销）外，还有关键的一点：`有助于避免this逃逸问题——如果我们在构造器中启动一个线程，因为另一个任务可能会在构造器结束之前开始执行，此时可能会访问到初始化了一半的对象用Executor在构造器中。`

&emsp;&emsp;Executor框架包括：线程池，Executor，Executors，ExecutorService，CompletionService，Future，Callable等。

&emsp;&emsp;Executor接口中之定义了一个方法execute（Runnable command），该方法接收一个Runable实例，它用来执行一个任务，任务即一个实现了Runnable接口的类。ExecutorService接口继承自Executor接口，它提供了更丰富的实现多线程的方法，比如，ExecutorService提供了关闭自己的方法，以及可为跟踪一个或多个异步任务执行状况而生成 Future 的方法。 可以调用ExecutorService的shutdown（）方法来平滑地关闭 ExecutorService，调用该方法后，将导致ExecutorService停止接受任何新的任务且等待已经提交的任务执行完成(已经提交的任务会分两类：一类是已经在执行的，另一类是还没有开始执行的)，当所有已经提交的任务执行完毕后将会关闭ExecutorService。因此我们一般用该接口来实现和管理多线程。

&emsp;&emsp;ExecutorService的生命周期包括三种状态：运行、关闭、终止。创建后便进入运行状态，当调用了shutdown（）方法时，便进入关闭状态，此时意味着ExecutorService不再接受新的任务，但它还在执行已经提交了的任务，当已经提交了的任务执行完后，便到达终止状态。如果不调用shutdown（）方法，ExecutorService会一直处在运行状态，不断接收新的任务，执行新的任务，服务器端一般不需要关闭它，保持一直运行即可。

&emsp;&emsp;Executors提供了一系列工厂方法用于创建线程池，返回的线程池都实现了ExecutorService接口。   

````
    public static ExecutorService newFixedThreadPool(int nThreads)

    创建固定数目线程的线程池。

    public static ExecutorService newCachedThreadPool()

    创建一个可缓存的线程池，调用execute将重用以前构造的线程（如果线程可用）。如果现有线程没有可用的，则创建一个新线程并添加到池中。终止并从缓存中移除那些已有60秒钟未被使用的线程。

    public static ExecutorService newSingleThreadExecutor()

    创建一个单线程化的Executor。

    public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize)

    创建一个支持定时及周期性的任务执行的线程池，多数情况下可用来替代Timer类。

 

    这四种方法都是用的Executors中的ThreadFactory建立的线程，下面就以上四个方法做个比较
````


![线程池创建线程](https://www.github.com/only-wjt/images/raw/master/小书匠/线程池.png)


&emsp;&emsp;一般来说，CachedTheadPool在程序执行过程中通常会创建与所需数量相同的线程，然后在它回收旧线程时停止创建新线程，因此它是合理的Executor的首选，只有当这种方式会引发问题时（比如需要大量长时间面向连接的线程时），才需要考虑用FixedThreadPool。（该段话摘自《Thinking in Java》第四版）

#### Executor执行Runnable任务
&emsp;&emsp;通过Executors的以上`四个静态工厂方法获得`ExecutorService实例，而后调用该实例的execute（Runnable command）方法即可。一旦Runnable任务传递到execute（）方法，该方法便会自动在一个线程上


```` 
import java.util.concurrent.ExecutorService;   
import java.util.concurrent.Executors;   
  
public class TestCachedThreadPool{   
    public static void main(String[] args){   
        ExecutorService executorService = Executors.newCachedThreadPool();   
//      ExecutorService executorService = Executors.newFixedThreadPool(5);  
//      ExecutorService executorService = Executors.newSingleThreadExecutor();  
        for (int i = 0; i < 5; i++){   
            executorService.execute(new TestRunnable());   
            System.out.println("************* a" + i + " *************");   
        }   
        executorService.shutdown();   
    }   
}   
  
  //相当于runable中的run()，此处在main函数中进行new对象操作，直接传递到execute方法中执行
class TestRunnable implements Runnable{   
    public void run(){   
        System.out.println(Thread.currentThread().getName() + "线程被调用了。");   
    }   
} 
````

#### Executor执行Callable任务
&emsp;&emsp;在Java 5之后，任务分两类：一类是实现了Runnable接口的类，一类是实现了Callable接口的类。两者都可以被ExecutorService执行，但是Runnable任务没有返回值，而Callable任务有返回值。并且Callable的call()方法只能通过ExecutorService的submit(Callable<T> task) 方法来执行，并且返回一个 <T>Future<T>，是表示任务等待完成的 Future。

&emsp;&emsp;Callable接口类似于Runnable，两者都是为那些其实例可能被另一个线程执行的类设计的。但是 Runnable 不会返回结果，并且无法抛出经过检查的异常而Callable又返回结果，而且当获取返回结果时可能会抛出异常。Callable中的call()方法类似Runnable的run()方法，区别同样是有返回值，后者没有。

&emsp;&emsp;当将一个Callable的对象传递给ExecutorService的submit方法，则该call方法自动在一个线程上执行，并且会返回执行结果Future对象。同样，将Runnable的对象传递给ExecutorService的submit方法，则该run方法自动在一个线程上执行，并且会返回执行结果Future对象，但是在该Future对象上调用get方法，将返回null。

&emsp;&emsp;下面给出一个Executor执行Callable任务的示例代码：

````
import java.util.ArrayList;   
import java.util.List;   
import java.util.concurrent.*;   
  
public class CallableDemo{   
    public static void main(String[] args){   
        ExecutorService executorService = Executors.newCachedThreadPool();   
        List<Future<String>> resultList = new ArrayList<Future<String>>();   
  
        //创建10个任务并执行   
        for (int i = 0; i < 10; i++){   
            //使用ExecutorService执行Callable类型的任务，并将结果保存在future变量中   
            Future<String> future = executorService.submit(new TaskWithResult(i));   
            //将任务执行结果存储到List中   
            resultList.add(future);   
        }   
  
        //遍历任务的结果   
        for (Future<String> fs : resultList){   
                try{   
                    while(!fs.isDone);//Future返回如果没有完成，则一直循环等待，直到Future返回完成  
                    System.out.println(fs.get());     //打印各个线程（任务）执行的结果   
                }catch(InterruptedException e){   
                    e.printStackTrace();   
                }catch(ExecutionException e){   
                    e.printStackTrace();   
                }finally{   
                    //启动一次顺序关闭，执行以前提交的任务，但不接受新任务  
                    executorService.shutdown();   
                }   
        }   
    }   
} 
````
  
````
class TaskWithResult implements Callable<String>{   
    private int id;   
  
    public TaskWithResult(int id){   
        this.id = id;   
    }   
  
    /**  
     * 任务的具体过程，一旦任务传给ExecutorService的submit方法， 
     * 则该方法自动在一个线程上执行 
     */   
    public String call() throws Exception {  
        System.out.println("call()方法被自动调用！！！    " + Thread.currentThread().getName());   
        //该返回结果将被Future的get方法得到  
        return "call()方法被自动调用，任务返回的结果是：" + id + "    " + Thread.currentThread().getName();   
    }   
} 
````

&emsp;&emsp;从结果中可以同样可以看出，submit也是首先选择空闲线程来执行任务，如果没有，才会创建新的线程来执行任务。另外，需要注意：如果Future的返回尚未完成，则get（）方法会阻塞等待，直到Future完成返回，可以通过调用isDone（）方法判断Future是否完成了返回。


#### 自定义线程池
&emsp;&emsp;自定义线程池，可以用ThreadPoolExecutor类创建，它有多个构造方法来创建线程池，用该类很容易实现自定义的线程池，这里先贴上示例程序：

````
import java.util.concurrent.ArrayBlockingQueue;   
import java.util.concurrent.BlockingQueue;   
import java.util.concurrent.ThreadPoolExecutor;   
import java.util.concurrent.TimeUnit;   
  
public class ThreadPoolTest{   
    public static void main(String[] args){   
        //创建等待队列   
        BlockingQueue<Runnable> bqueue = new ArrayBlockingQueue<Runnable>(20);   
        //创建线程池，池中保存的线程数为3，允许的最大线程数为5  
        ThreadPoolExecutor pool = new ThreadPoolExecutor(3,5,50,TimeUnit.MILLISECONDS,bqueue);   
        //创建七个任务   
        Runnable t1 = new MyThread();   
        Runnable t2 = new MyThread();   
        Runnable t3 = new MyThread();   
        Runnable t4 = new MyThread();   
        Runnable t5 = new MyThread();   
        Runnable t6 = new MyThread();   
        Runnable t7 = new MyThread();   
        //每个任务会在一个线程上执行  
        pool.execute(t1);   
        pool.execute(t2);   
        pool.execute(t3);   
        pool.execute(t4);   
        pool.execute(t5);   
        pool.execute(t6);   
        pool.execute(t7);   
        //关闭线程池   
        pool.shutdown();   
    }   
}   
  
class MyThread implements Runnable{   
    @Override   
    public void run(){   
        System.out.println(Thread.currentThread().getName() + "正在执行。。。");   
        try{   
            Thread.sleep(100);   
        }catch(InterruptedException e){   
            e.printStackTrace();   
        }   
    }   
}
````

&emsp;&emsp;从结果中可以看出，七个任务是在线程池的三个线程上执行的。这里简要说明下用到的ThreadPoolExecuror类的构造方法中各个参数的含义。   

 
````
public ThreadPoolExecutor (int corePoolSize, int maximumPoolSize, long         keepAliveTime, TimeUnit unit,BlockingQueue<Runnable> workQueue)
 

corePoolSize：线程池中所保存的线程数，包括空闲线程。

maximumPoolSize：池中允许的最大线程数。

keepAliveTime：当线程数大于核心数时，该参数为所有的任务终止前，多余的空闲线程等待新任务的最长时间。

unit：等待时间的单位。

workQueue：任务执行前保存任务的队列，仅保存由execute方法提交的Runnable任务。
````


### 四种创建线程方法对比

&emsp;&emsp;实现Runnable和实现Callable接口的方式基本相同，不过是后者执行call()方法有返回值，后者线程执行体run()方法无返回值，因此可以把这两种方式归为一种这种方式与继承Thread类的方法之间的差别如下：

&emsp;&emsp;1、线程只是实现Runnable或实现Callable接口，还可以继承其他类。

&emsp;&emsp;2、这种方式下，多个线程可以共享一个target对象，非常适合多线程处理同一份资源的情形。

&emsp;&emsp;3、但是编程稍微复杂，如果需要访问当前线程，必须调用Thread.currentThread()方法。

&emsp;&emsp;4、继承Thread类的线程类不能再继承其他父类（Java单继承决定）。

&emsp;&emsp;5、前三种的线程如果创建关闭频繁会消耗系统资源影响性能，而使用线程池可以不用线程的时候放回线程池，用的时候再从线程池取，项目开发中主要使用线程池

&emsp;&emsp;注：在前三种中一般推荐采用实现接口的方式来创建多线程


## interrupt中断

&emsp;&emsp;Java中断机制是一种协作机制，也就是说通过中断并不能直接终止另一个线程，它只是要求被中断线程在合适的时机中断自己，这需要被中断的线程自己处理中断。这好比是家里的父母叮嘱在外的子女要注意身体，但子女是否注意身体，怎么注意身体则完全取决于自己。

### 中断状态的管理
&emsp;&emsp;一般说来，当可能阻塞的方法声明中有抛出InterruptedException则暗示该方法是可中断的，如BlockingQueue#put、BlockingQueue#take、Object#wait、Thread#sleep、condition.await以及可中断的通道上的 I/O 操作方法等，如果程序捕获到这些可中断的阻塞方法抛出的InterruptedException或检测到中断后，这些中断信息该如何处理？一般有以下两个通用原则：

&emsp;&emsp;如果遇到的是可中断的阻塞方法抛出InterruptedException，可以继续向方法调用栈的上层抛出该异常，如果是检测到中断，则可清除中断状态并抛出InterruptedException，使当前方法也成为一个可中断的方法。 
&emsp;&emsp;若有时候不太方便在方法上抛出InterruptedException，比如要实现的某个接口中的方法签名上没有throws InterruptedException，这时就可以捕获可中断方法的InterruptedException并通过Thread.currentThread.interrupt()来重新设置中断状态。如果是检测并清除了中断状态，亦是如此。 

&emsp;&emsp;一般的代码中，尤其是作为一个基础类库时，绝不应当吞掉中断，即捕获到InterruptedException后在catch里什么也不做，清除中断状态后又不重设中断状态也不抛出InterruptedException等。因为吞掉中断状态会导致方法调用栈的上层得不到这些信息。

&emsp;&emsp;当然，凡事总有例外的时候，当你完全清楚自己的方法会被谁调用，而调用者也不会因为中断被吞掉了而遇到麻烦，就可以这么做。

&emsp;&emsp;总得来说，就是要让方法调用栈的上层获知中断的发生。假设你写了一个类库，类库里有个方法amethod，在amethod中检测并清除了中断状态，而没有抛出InterruptedException，作为amethod的用户来说，他并不知道里面的细节，如果用户在调用amethod后也要使用中断来做些事情，那么在调用amethod之后他将永远也检测不到中断了，因为中断信息已经被amethod清除掉了。如果作为用户，遇到这样有问题的类库，又不能修改代码，那该怎么处理？只好在自己的类里设置一个自己的中断状态，在调用interrupt方法的时候，同时设置该状态，这实在是无路可走时才使用的方法。

&emsp;&emsp;注意：`synchronized在获锁的过程中是不能被中断的，意思是说如果产生了死锁，则不可能被中断（请参考后面的测试例子）。`与synchronized功能相似的reentrantLock.lock()方法也是一样，它也不可中断的，即如果发生死锁，那么reentrantLock.lock()方法无法终止，如果调用时被阻塞，则它一直阻塞到它获取到锁为止。但是如果调用带超时的tryLock方法reentrantLock.tryLock(long timeout, TimeUnit unit)，那么如果线程在等待时被中断，将抛出一个InterruptedException异常，这是一个非常有用的特性，因为它允许程序打破死锁。你也可以调用reentrantLock.lockInterruptibly()方法，它就相当于一个超时设为无限的tryLock方法。


### 中断线程（方法）

#### java.lang.Thread类提供了几个方法来操作这个中断状态：


![线程中断方法](https://www.github.com/only-wjt/images/raw/master/小书匠/线程中断方法.png)

##### interrupt() 
&emsp;&emsp;interrupt方法用于中断线程。调用该方法的线程的状态为将被置为"中断"状态。
&emsp;&emsp;注意：线程中断仅仅是置线程的中断状态位，不会停止线程。需要用户自己去监视线程的状态为并做处理。支持线程中断的方法（也就是线程中断后会抛出interruptedException的方法）就是在监视线程的中断状态，一旦线程的中断状态被置为“中断状态”，就会抛出中断异常。
##### interrupted()

````
    public static boolean interrupted()
    {
        return currentThread().isInterrupted(true);
    }
````

&emsp;&emsp;该方法就是直接调用当前线程的isInterrupted(true)的方法。静态方法interrupted会将当前线程的中断状态清除，但这个方法的命名极不直观，很容易造成误解，需要特别注意。

##### isInterrupted()

````
    public boolean isInterrupted()
    {
        return isInterrupted(false);
    }
    private native boolean isInterrupted(boolean flag);
````

&emsp;&emsp;该方法却直接调用当前线程的native isInterrupted(false)的方法，不清除中断状态。

&emsp;&emsp;因此interrupted()和isInterrupted()这两个方法主要区别：这两个方法最终都会调用同一个方法-----isInterrupted( Boolean 参数)（重载方法,是私有的native方法），只不过参数固定为一个是true，一个是false；

&emsp;&emsp;由于第二个区别主要体现在调用的方法的参数上，让我们来看一看这个参数是什么含义：
&emsp;&emsp;先来看一看被调用的方法 isInterrupted(boolean arg)（Thread类中重载的方法）的定义：

````
private native boolean isInterrupted( boolean flag);

````

&emsp;&emsp;这个本地方法，看不到源码。作用是要清除状态位。如果这个参数为true，说明返回线程的状态位后，要清掉原来的状态位（恢复成原来情况）。这个参数为false，就是直接返回线程的状态位。

&emsp;&emsp;这两个方法很好区分，只有当前线程才能清除自己的中断位（对应interrupted()方法）


#### 中断方法

##### interrupt()
&emsp;&emsp;interrupt()只是改变中断状态而已，interrupt()不会中断（不是停止线程）一个正在运行的线程。这一方法实际上完成的是，给线程抛出一个中断信号， 这样受阻线程就得以退出阻塞的状态（不会停止线程。需要用户自己去监视线程的状态为并做处理）。更确切的说，如果线程被Object.wait, Thread.join和Thread.sleep三种方法之一阻塞，那么，它将接收到一个中断异常（InterruptedException），从而提早地终结被阻塞状态。

&emsp;&emsp;如果线程是阻塞（Object.wait, Thread.join和Thread.sleep）的，则线程会自动检测中断，抛出中断异常（InterruptedException），给应用提供中断相宜的处理。
&emsp;&emsp;如果线程没有被阻塞，则线程不会帮助我们检测中断，需要我们手动进行中断检测，在检测到中断后，应用可以做相应的处理（同上）。如2.2.1.4中示例
&emsp;&emsp;或者可以简单总结一下中断两种情况：

&emsp;&emsp;一种是当线程处于阻塞状态或者试图执行一个阻塞操作时，我们可以使用实例方法interrupt()进行线程中断，执行中断操作后将会抛出interruptException异常(该异常必须捕捉无法向外抛出)并将中断状态复位；
&emsp;&emsp;另外一种是当线程处于运行状态时，我们也可调用实例方法interrupt()进行线程中断，但同时必须手动判断中断状态，并编写中断线程的代码(其实就是结束run方法体的代码)

&emsp;&emsp;例如：
&emsp;&emsp;线程A在执行sleep,wait,join时,线程B调用线程A的interrupt方法,的确这一个时候A会有InterruptedException 异常抛出来。 但这其实是在sleep,wait,join这些方法内部会不断检查中断状态的值,而自己抛出的InterruptedException。
&emsp;&emsp;如果线程A正在执行一些指定的操作时如赋值,for,while,if,调用方法等,都不会去检查中断状态,所以线程A不会抛出 InterruptedException,而会一直执行着自己的操作。当线程A终于执行到wait(),sleep(),join()时,才马上会抛出 InterruptedException.
&emsp;&emsp;若没有调用sleep(),wait(),join()这些方法,即没有在线程里自己检查中断状态自己抛出InterruptedException的话,那InterruptedException是不会被抛出来的。

&emsp;&emsp;注意1:当线程A执行到wait(),sleep(),join()时,抛出InterruptedException后，中断状态已经被系统复位了，线程A调用Thread.interrupted()返回的是false。

##### sleep() & interrupt()示例

&emsp;&emsp;线程A正在使用sleep()暂停着: Thread.sleep(100000);
&emsp;&emsp;如果要取消他的等待状态,可以在正在执行的线程里(比如这里是B)调用a.interrupt()令线程A放弃睡眠操作,这里a是线程A对应到的Thread实例
&emsp;&emsp;当在sleep中时 线程被调用interrupt()时,就马上会放弃暂停的状态.并抛出InterruptedException.丢出异常的,是A线程。

````
package com.dxz.interrupt;


public class ThreadDemo1A extends Thread {

    @Override
    public void run() {
        try {
            System.out.println("线程a开始工作");
            sleep(30000);
        } catch (InterruptedException e) {
            System.out.println(this.isInterrupted());
        }
    }
}


package com.dxz.interrupt;

import java.util.concurrent.TimeUnit;

public class ThreadDemo1B {

    public static void main(String[] args) throws InterruptedException {
        ThreadDemo1A threada = new ThreadDemo1A();
        threada.start();
        TimeUnit.SECONDS.sleep(3);
        System.out.println("开始中断线程a");
        threada.interrupt();
    }
}
````

结果：

````
线程a开始工作
开始中断线程a
false
````

##### wait() & interrupt()示例

&emsp;&emsp;线程A调用了wait()进入了等待状态,也可以用interrupt()取消。
&emsp;&emsp;不过这时候要小心锁定的问题.线程在进入等待区,会把锁定解除,当对等待中的线程调用interrupt()时,会先重新获取锁定,再抛出异常。在获取锁定之前,是无法抛出异常的。

````
package com.dxz.interrupt;

public class ThreadDemo2A extends Thread {

    @Override
    public void run() {
        try {
            System.out.println("线程a开始工作");
            synchronized (this) {
                wait(30000);
            }
        } catch (InterruptedException e) {
            System.out.println(this.isInterrupted());
        }
    }
}

package com.dxz.interrupt;

import java.util.concurrent.TimeUnit;

public class ThreadDemo2B {

    public static void main(String[] args) throws InterruptedException {
        ThreadDemo2A threada = new ThreadDemo2A();
        threada.start();
        TimeUnit.SECONDS.sleep(3);
        System.out.println("开始中断线程a");
        threada.interrupt();
    }
}
````

&emsp;&emsp;结果：

````
线程a开始工作
开始中断线程a
false
````

##### join() & interrupt()示例

&emsp;&emsp;当线程以join()等待其他线程结束时,当它被调用interrupt()，它与sleep()时一样, 会马上跳到catch块里。注意，是对谁调用interrupt()方法,一定是调用被阻塞线程的interrupt方法.如在线程a中调用来线程t.join()，
则a会等t执行完后在执行t.join后的代码,当在线程b中调用a.interrupt()方法,则会抛出InterruptedException，当然线程t的join()也就被取消了。线程t还会继续运行下去。

````
package com.dxz.interrupt;

public class T extends Thread {

    public T(String s) {
        super(s);
    }
    
    @Override
    public void run() {
        System.out.println("线程t开始进入死循环");
        for(;;);
    }
}

package com.dxz.interrupt;


public class ThreadDemo3A extends Thread {

    public ThreadDemo3A(String s) {
        super(s);
    }
    @Override
    public void run() {
        try {
            System.out.println("线程a等待线程t执行");
            T t = new T("demo3t");
            t.start();
            t.join();
            System.out.println("线程a中的线程t已经结束");
        } catch (InterruptedException e) {
            System.out.println(this.isInterrupted());
        }
    }

}
package com.dxz.interrupt;

import java.util.concurrent.TimeUnit;

public class ThreadDemo3B  {

    public static void main(String[] args) {
        ThreadDemo3A threada = new ThreadDemo3A("demo3a");
        threada.start();
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (InterruptedException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        System.out.println("开始中断线程a");
        threada.interrupt();
        System.out.println("--------");
    }
    
}
````

&emsp;&emsp;结果：

````
线程a等待线程t执行
线程t开始进入死循环
开始中断线程a

false
````

![图1](https://www.github.com/only-wjt/images/raw/master/小书匠/图1.png)

&emsp;&emsp;可以看到T线程在主线程结束了，还在运行。

![图2](https://www.github.com/only-wjt/images/raw/master/小书匠/图2.png)


##### 处于非阻塞状态的线程需要我们手动进行中断检测并结束程序--示例

````
package com.dxz.synchronize;

import java.util.concurrent.TimeUnit;

public class InterruputThread {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread() {
            @Override
            public void run() {
                while (true) {
                    // 判断当前线程是否被中断
                    if (this.isInterrupted()) {
                        System.out.println("线程中断");
                        break;
                    }
                }

                System.out.println("已跳出循环,线程中断!");
            }
        };
        t1.start();
        TimeUnit.SECONDS.sleep(2);
        t1.interrupt();
    }
}
````

&emsp;&emsp;结果：

````
线程中断
已跳出循环,线程中断!
````

##### 类库中的有些类的方法也可能会调用中断
&emsp;&emsp;此外，类库中的有些类的方法也可能会调用中断，如FutureTask中的cancel方法，如果传入的参数为true，它将会在正在运行异步任务的线程上调用interrupt方法，如果正在执行的异步任务中的代码没有对中断做出响应，那么cancel方法中的参数将不会起到什么效果；又如ThreadPoolExecutor中的shutdownNow方法会遍历线程池中的工作线程并调用线程的interrupt方法来中断线程，所以如果工作线程中正在执行的任务没有对中断做出响应，任务将一直执行直到正常结束。

### 线程被中断的检测（判断）
&emsp;&emsp;判断某个线程是否已被发送过中断请求，请使用Thread.currentThread().isInterrupted()方法（因为它将线程中断标示位设置为true后，不会立刻清除中断标示位，即不会将中断标设置为false），而不要使用thread.interrupted()（该方法调用后会将中断标示位清除，即重新设置为false）方法来判断，下面是线程在循环中时的中断方式：

````
while(!Thread.currentThread().isInterrupted() && more work to do){
do more work
}
````
### 处理时机
&emsp;&emsp;显然，作为一种协作机制，不会强求被中断线程一定要在某个点进行处理。实际上，被中断线程只需在合适的时候处理即可，如果没有合适的时间点，甚至可以不处理，这时候在任务处理层面，就跟没有调用中断方法一样。“合适的时候”与线程正在处理的业务逻辑紧密相关，例如，每次迭代的时候，进入一个可能阻塞且无法中断的方法之前等，但多半不会出现在某个临界区更新另一个对象状态的时候，因为这可能会导致对象处于不一致状态。
&emsp;&emsp;处理时机决定着程序的效率与中断响应的灵敏性。频繁的检查中断状态可能会使程序执行效率下降，相反，检查的较少可能使中断请求得不到及时响应。如果发出中断请求之后，被中断的线程继续执行一段时间不会给系统带来灾难，那么就可以将中断处理放到方便检查中断，同时又能从一定程度上保证响应灵敏度的地方。当程序的性能指标比较关键时，可能需要建立一个测试模型来分析最佳的中断检测点，以平衡性能和响应灵敏性。

###  中断的响应
&emsp;&emsp;程序里发现中断后该怎么响应？这就得视实际情况而定了。有些程序可能一检测到中断就立马将线程终止，有些可能是退出当前执行的任务，继续执行下一个任务……作为一种协作机制，这要与中断方协商好，当调用interrupt会发生些什么都是事先知道的，如做一些事务回滚操作，一些清理工作，一些补偿操作等。若不确定调用某个线程的interrupt后该线程会做出什么样的响应，那就不应当中断该线程。

### Thread.interrupt VS Thread.stop
&emsp;&emsp;Thread.stop方法已经不推荐使用了。而在某些方面Thread.stop与中断机制有着相似之处。如当线程在等待内置锁或IO时，stop跟interrupt一样，不会中止这些操作；当catch住stop导致的异常时，程序也可以继续执行，虽然stop本意是要停止线程，这么做会让程序行为变得更加混乱。
&emsp;&emsp;那么它们的区别在哪里？最重要的就是中断需要程序自己去检测然后做相应的处理，而Thread.stop会直接在代码执行过程中抛出ThreadDeath错误，这是一个java.lang.Error的子类。


## ~~线程之stop()~~


&emsp;&emsp;存在很大问题，很多人已经不再使用，下面可以看到stop()方法存在什么样的问题


&emsp;&emsp;从SUN的官方文档可以得知，调用Thread.stop()方法是不安全的，这是因为当调用Thread.stop()方法时，会发生下面两件事：

&emsp;&emsp;1. 即刻抛出ThreadDeath异常，在线程的run()方法内，任何一点都有可能抛出ThreadDeath Error，包括在catch或finally语句中。

&emsp;&emsp;2. 会释放该线程所持有的所有的锁，而这种释放是不可控制的，非预期的。

 

&emsp;&emsp;当线程抛出ThreadDeath异常时，会导致该线程的run()方法突然返回来达到停止该线程的目的。ThreadDetath异常可以在该线程run()方法的任意一个执行点抛出。但是，线程的stop()方法一经调用线程的run()方法就会即刻返回吗？

````
package com.dxz.threadstop;

public class ThreadStopTest {
    public static void main(String[] args) {
        try {
            Thread t = new Thread() {
                // 对于方法进行了同步操作，锁对象就是线程本身
                public synchronized void run() {
                    try {
                        long start = System.currentTimeMillis();
                        // 开始计数
                        for (int i = 0; i < 100000; i++)
                            System.out.println("runing.." + i);
                        System.out.println((System.currentTimeMillis() - start) + "ms");
                    } catch (Throwable ex) {
                        System.out.println("Caught in run: " + ex);
                        ex.printStackTrace();
                    }
                }
            };
            // 开始计数
            t.start();
            // 主线程休眠100ms
            Thread.sleep(100);
            // 停止线程的运行
            t.stop();
        } catch (Throwable t) {
            System.out.println("Caught in main: " + t);
            t.printStackTrace();
        }

    }
}
````

![结果1](https://www.github.com/only-wjt/images/raw/master/小书匠/结果1.png)

&emsp;&emsp;由于打印的数据太多了，就没有全部截图了，但是我们可以看到，调用了stop方法之后，线程并没有停止，而是将run方法执行完。那这个就诡异了，多次运行之后发现每次运行的结果都表明，工作线程并没有停止，而是每次都成功的数完数(执行完run方法)，然后正常中止，而不是由stop()方法进行终止的。这个是为什么呢？根据SUN的文档，原则上只要一调用thread.stop()方法，那么线程就会立即停止，并抛出ThreadDeath error，`查看了Thread的源代码后才发现，原先Thread.stop(Throwable obj)方法是同步的，而我们工作线程的run()方法也是同步，那么这样会导致主线程和工作线程共同争用同一个锁（工作线程对象本身），由于工作线程在启动后就先获得了锁，所以无论如何，当主线程在调用t.stop()时，它必须要等到工作线程的run()方法执行结束后才能进行，结果导致了上述奇怪的现象。`

![结果2](https://www.github.com/only-wjt/images/raw/master/小书匠/结果2.png)

&emsp;&emsp;下面看一下stop的源码1.8版本：

````
    /**
     * @deprecated Method stop is deprecated
     */

    public final void stop()
    {
        SecurityManager securitymanager = System.getSecurityManager();
        if(securitymanager != null)
        {
            checkAccess();
            if(this != currentThread())
                securitymanager.checkPermission(SecurityConstants.STOP_THREAD_PERMISSION);
        }
        if(threadStatus != 0)
            resume();
        stop0(new ThreadDeath());
    }
````

&emsp;&emsp;把上述工作线程的run()方法的同步去掉，再进行执行，结果就如上述第一点描述的那样了，运行结果如下：

![结果3](https://www.github.com/only-wjt/images/raw/master/小书匠/结果3.png)

&emsp;&emsp;从结果中我们可以看到，调用stop方法会抛出一个ThreadDeath异常，这时候run方法也就执行结束了，线程就终止了，这种是用抛异常来结束线程的，但是这种抛出线程是不安全的，因为他不可控制，不知道到在run方法中的何处就可能抛出异常，所以是危险的。下面在看一下stop的这个隐患可能造成的影响：

&emsp;&emsp;接下来是看看当调用thread.stop()时，被停止的线程会释放其所持有的锁，看如下代码：

````
public static void main(String[] args) {
        // 定义锁对象
        final Object lock = new Object();
        // 定义第一个线程，首先该线程拿到锁，而后等待3s,之后释放锁
        try {
            Thread t0 = new Thread() {
                public void run() {
                    try {
                        synchronized (lock) {
                            System.out.println("thread->" + getName() + " acquire lock.");
                            sleep(3 * 1000);
                            System.out.println("thread->" + getName() + " 等待3s");
                            System.out.println("thread->" + getName() + " release lock.");
                        }
                    } catch (Throwable ex) {
                        System.out.println("Caught in run: " + ex);
                        ex.printStackTrace();
                    }
                }
            };

            // 定义第二个线程，等待拿到锁对象
            Thread t1 = new Thread() {
                public void run() {
                    synchronized (lock) {
                        System.out.println("thread->" + getName() + " acquire lock.");
                    }
                }
            };

            // 线程一先运行，先拿到lock
            t0.start();
            // 而后主线程等待100ms,为了做延迟
            Thread.sleep(100);
            // 停止线程一
            // t0.stop();
            // 这时候在开启线程二
            t1.start();
        } catch (Throwable t) {
            System.out.println("Caught in main: " + t);
            t.printStackTrace();
        }

    }
````
	
&emsp;&emsp;运行结果如下
	
![结果4](https://www.github.com/only-wjt/images/raw/master/小书匠/结果4.png)

&emsp;&emsp;从运行结果中我们可以看到，当没有进行t0.stop()方法的调用时， 可以发现，两个线程争用锁的顺序是固定的。这个现象是正常的。

&emsp;&emsp;下面我们把t0.stop注释的哪行，删除注释，调用t0.stop()方法，运行结果如下：

![结果5](https://www.github.com/only-wjt/images/raw/master/小书匠/结果5.png)

&emsp;&emsp;从运行结果中我们可以看到，调用了t0.stop()方法后，可以发现，t0线程抛出了ThreadDeath error并且t0线程释放了它所占有的锁。

&emsp;&emsp;从上面的程序验证结果来看，thread.stop()确实是不安全的。它的不安全主要是：释放该线程所持有的所有的锁。一般任何进行加锁的代码块，都是为了保护数据的一致性，如果在调用thread.stop()后导致了该线程所持有的所有锁的突然释放(不可控制)，那么被保护数据就有可能呈现不一致性，其他线程在使用这些被破坏的数据时，有可能导致一些很奇怪的应用程序错误。

&emsp;&emsp;下面顺便说一下：

&emsp;&emsp;Java中多线程锁释放的条件：

&emsp;&emsp;1）执行完同步代码块，就会释放锁。（synchronized）

&emsp;&emsp;2）在执行同步代码块的过程中，遇到异常而导致线程终止，锁也会被释放。（exception）

&emsp;&emsp;3）在执行同步代码块的过程中，执行了锁所属对象的wait()方法，这个线程会释放锁，进入对象的等待池。(wait)


&emsp;&emsp;从上面的三点我就可以看到stop方法释放锁是在第二点的，通过抛出异常来释放锁，通过证明，这种方式是不安全的，不可靠的。


## 停止线程的方法总结

&emsp;&emsp;在JAVA中，曾经使用stop方法来停止线程，然而，该方法具有固有的不安全性，因而已经被抛弃(Deprecated)。那么应该怎么结束一个进程呢？官方文档中对此有详细说明：[《为何不赞成使用 Thread.stop、Thread.suspend 和 Thread.resume？》](http://download.oracle.com/javase/6/docs/technotes/guides/concurrency/threadPrimitiveDeprecation.html)。在此引用stop方法的说明：

1. Why is Thread.stop deprecated?
Because it is inherently unsafe. Stopping a thread causes it to unlock all the monitors that it has locked. (The monitors are unlocked as the ThreadDeath exception propagates up the stack.) If any of the objects previously protected by these monitors were in an inconsistent state, other threads may now view these objects in an inconsistent state. Such objects are said to be damaged. When threads operate on damaged objects, arbitrary behavior can result. This behavior may be subtle and difficult to detect, or it may be pronounced. Unlike other unchecked exceptions, ThreadDeath kills threads silently; thus, the user has no warning that his program may be corrupted. The corruption can manifest itself at any time after the actual damage occurs, even hours or days in the future.
&emsp;&emsp;大概意思是：
&emsp;&emsp;因为该方法本质上是不安全的。停止一个线程将释放它已经锁定的所有监视器（作为沿堆栈向上传播的未检查 ThreadDeath 异常的一个自然后果）。如果以前受这些监视器保护的任何对象都处于一种不一致的状态，则损坏的对象将对其他线程可见，这有可能导致任意的行为。此行为可能是微妙的，难以察觉，也可能是显著的。不像其他的未检查异常，ThreadDeath异常会在后台杀死线程，因此，用户并不会得到警告，提示他的程序可能已损坏。这种损坏有可能在实际破坏发生之后的任何时间表现出来，也有可能在多小时甚至在未来的很多天后。
&emsp;&emsp;在文档中还提到，程序员不能通过捕获ThreadDeath异常来修复已破坏的对象。具体原因见原文。既然stop方法不建议使用，那么应该用什么方法来代理stop已实现相应的功能呢？
 
### 通过修改`共享变量`来通知目标线程停止运行
 
&emsp;&emsp;大部分需要使用stop的地方应该使用这种方法来达到中断线程的目的。
 
&emsp;&emsp;这种方法有几个要求或注意事项：
&emsp;&emsp;（1）目标线程必须有规律的检查变量，当该变量指示它应该停止运行时，该线程应该按一定的顺序从它执行的方法中返回。
&emsp;&emsp;（2）该变量必须定义为volatile，或者所有对它的访问必须同步(synchronized)。
 
例如：

````
package chapter2; 

public class ThreadFlag extends Thread 
{ 
    public volatile boolean exit = false; 

    public void run() 
    { 
        while (!exit); 
    } 
    public static void main(String[] args) throws Exception 
    { 
        ThreadFlag thread = new ThreadFlag(); 
        thread.start(); 
        sleep(5000); // 主线程延迟5秒 
        thread.exit = true;  // 终止线程thread 
        thread.join(); 
        System.out.println("线程退出!"); 
    } 
} 
````

&emsp;&emsp;在上面代码中定义了一个退出标志exit，当exit为true时，while循环退出，exit的默认值为false.在定义exit时，使用了一个Java关键字volatile，这个关键字的目的是使exit同步，也就是说在同一时刻只能由一个线程来修改exit的值。

### 通过Thread.interrupt方法中断线程

&emsp;&emsp;通常情况下，我们应该使用第一种方式来代替Thread.stop方法。然而以下几种方式应该使用Thread.interrupt方法来中断线程（该方法通常也会结合第一种方法使用）。
&emsp;&emsp;一开始使用interrupt方法时，会有莫名奇妙的感觉：难道该方法有问题？
&emsp;&emsp;API文档上说，该方法用于"Interrupts this thread"。请看下面的例子：

````
package com.jvm.study.thread;

public class TestThread implements Runnable{ 
     
    boolean stop = false; 
    public static void main(String[] args) throws Exception { 
        Thread thread = new Thread(new TestThread(),"My Thread"); 
        System.out.println( "Starting thread..." ); 
        thread.start(); 
        Thread.sleep( 3000 ); 
        System.out.println( "Interrupting thread..." ); 
        thread.interrupt(); 
        System.out.println("线程是否中断：" + thread.isInterrupted()); 
        Thread.sleep( 3000 ); 
        System.out.println("Stopping application..." ); 
    } 
    public void run() { 
        while(!stop){ 
            System.out.println( "My Thread is running..." ); 
            // 让该循环持续一段时间，使上面的话打印次数少点 
            long time = System.currentTimeMillis(); 
            while((System.currentTimeMillis()-time < 1000)) { 
            } 
        } 
        System.out.println("My Thread exiting under request..." ); 
    } 
} 
````

运行后的结果是：

````
Starting thread...
My Thread is running...
My Thread is running...
My Thread is running...
My Thread is running...
Interrupting thread...
线程是否中断：true
My Thread is running...
My Thread is running...
My Thread is running...
Stopping application...
My Thread is running...
My Thread is running...
……
````

&emsp;&emsp;应用程序并不会退出，启动的线程没有因为调用interrupt而终止，可是从调用isInterrupted方法返回的结果可以清楚地知道该线程已经中断了。那位什么会出现这种情况呢？到底是interrupt方法出问题了还是isInterrupted方法出问题了？在Thread类中还有一个测试中断状态的方法（静态的）interrupted，换用这个方法测试，得到的结果是一样的。由此似乎应该是interrupt方法出问题了。实际上，在JAVA API文档中对该方法进行了详细的说明。`该方法实际上只是设置了一个中断状态，当该线程由于下列原因而受阻时，这个中断状态就起作用了：`
&emsp;&emsp;（1）如果线程在调用 Object 类的 wait()、wait(long) 或 wait(long, int) 方法，或者该类的 join()、join(long)、join(long, int)、sleep(long) 或 sleep(long, int) 方法过程中受阻，则其中断状态将被清除，它还将收到一个InterruptedException异常。这个时候，我们可以通过捕获InterruptedException异常来终止线程的执行，具体可以通过return等退出或改变共享变量的值使其退出。
&emsp;&emsp;（2）如果该线程在可中断的通道上的 I/O 操作中受阻，则该通道将被关闭，该线程的中断状态将被设置并且该线程将收到一个 ClosedByInterruptException。这时候处理方法一样，只是捕获的异常不一样而已。
 
&emsp;&emsp;其实对于这些情况有一个通用的处理方法：

````
package com.jvm.study.thread;

public class TestThread2 implements Runnable {

    boolean stop = false;

    public static void main(String[] args) throws Exception {
        Thread thread = new Thread(new TestThread2(), "My Thread2");
        System.out.println("Starting thread...");
        thread.start();
        Thread.sleep(10000);
        System.out.println("Interrupting thread...");
        thread.interrupt();
        System.out.println("线程是否中断：" + thread.isInterrupted());
        Thread.sleep(3000);
        System.out.println("Stopping application...");
        Thread.sleep(30000);
    }

    public void run() {
        while (!stop) {
            System.out.println("My Thread is running...");
            // 让该循环持续一段时间，使上面的话打印次数少点
            long time = System.currentTimeMillis();
            while ((System.currentTimeMillis() - time < 1000)) {
            }
            if (Thread.currentThread().isInterrupted()) {
                return;
            }
        }
        System.out.println("My Thread exiting under request...");
    }
}
````

&emsp;&emsp;因为调用interrupt方法后，会设置线程的中断状态，所以，通过监视该状态来达到终止线程的目的。


&emsp;&emsp;当外部线程对某线程调用了thread.interrupt()方法后，java语言的处理机制如下：

&emsp;&emsp;如果该线程处在可中断状态下，（调用了xx.wait()，或者Selector.select(),Thread.sleep()等特定会发生阻塞的api），那么该线程会立即被唤醒，同时会受到一个InterruptedException，同时，如果是阻塞在io上，对应的资源会被关闭。如果该线程接下来不执行“Thread.interrupted()方法（不是interrupt），那么该线程处理任何io资源的时候，都会导致这些资源关闭。当然，解决的办法就是调用一下interrupted()，不过这里需要程序员自行根据代码的逻辑来设定，根据自己的需求确认是否可以直接忽略该中断，还是应该马上退出。

&emsp;&emsp;如果该线程处在不可中断状态下，就是没有调用上述api，那么java只是设置一下该线程的interrupt状态，其他事情都不会发生，如果该线程之后会调用行数阻塞API，那到时候线程会马会上跳出，并抛出InterruptedException，接下来的事情就跟第一种状况一致了。如果不会调用阻塞API，那么这个线程就会一直执行下去。除非你就是要实现这样的线程，一般高性能的代码中肯定会有wait()，yield()之类出让cpu的函数，不会发生后者的情况。

&emsp;&emsp;当线程处于非运行（Run）状态
&emsp;&emsp;当线程处于下面的状况时，属于非运行状态：

&emsp;&emsp;当sleep方法被调用。

&emsp;&emsp;当wait方法被调用。

&emsp;&emsp;当被I/O阻塞，可能是文件或者网络等等。

&emsp;&emsp;当线程处于上述的状态时，使用前面介绍的方法就不可用了。这个时候，我们可以使用interrupt()来打破阻塞的情况，如：

```
public void stop() {
        Thread tmpBlinker = blinker;
        blinker = null;
        if (tmpBlinker != null) {
           tmpBlinker.interrupt();
        }
    }
````

当interrupt()被调用的时候，InterruptedException将被抛出，所以你可以再run方法中捕获这个异常，让线程安全退出：

````
try {
   ....
   wait();
} catch (InterruptedException iex) {
   throw new RuntimeException("Interrupted",iex);
}
````

&emsp;&emsp;阻塞的I/O:当线程被I/O阻塞的时候，调用interrupt()的情况是依赖与实际运行的平台的。在Solaris和Linux平台上将会抛出InterruptedIOException的异常，但是Windows上面不会有这种异常。所以，我们处理这种问题不能依靠于平台的实现。如：

````
package com.cnblogs.gpcuster

import java.net.*;
import java.io.*;

public abstract class InterruptibleReader extends Thread {

    private Object lock = new Object( );
    private InputStream is;
    private boolean done;
    private int buflen;
    protected void processData(byte[] b, int n) { }

    class ReaderClass extends Thread {

        public void run( ) {
            byte[] b = new byte[buflen];

            while (!done) {
                try {
                    int n = is.read(b, 0, buflen);
                    processData(b, n);
                } catch (IOException ioe) {
                    done = true;
                }
            }

            synchronized(lock) {
                lock.notify( );
            }
        }
    }

    public InterruptibleReader(InputStream is) {
        this(is, 512);
    }

    public InterruptibleReader(InputStream is, int len) {
        this.is = is;
        buflen = len;
    }

    public void run( ) {
        ReaderClass rc = new ReaderClass( );

        synchronized(lock) {
            rc.start( );
            while (!done) {
                try {
                    lock.wait( );
                } catch (InterruptedException ie) {
                    done = true;
                    rc.interrupt( );
                    try {
                        is.close( );
                    } catch (IOException ioe) {}
                }
            }
        }
    }
}
```` 

&emsp;&emsp;另外，我们也可以使用InterruptibleChannel接口。 实现了InterruptibleChannel接口的类可以在阻塞的时候抛出ClosedByInterruptException。如：

````
package com.cnblogs.gpcuster

import java.io.BufferedReader;
import java.io.FileDescriptor;
import java.io.FileInputStream;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.channels.Channels;

public class InterruptInput {   
    static BufferedReader in = new BufferedReader(
            new InputStreamReader(
            Channels.newInputStream(
            (new FileInputStream(FileDescriptor.in)).getChannel())));
    
    public static void main(String args[]) {
        try {
            System.out.println("Enter lines of input (user ctrl+Z Enter to terminate):");
            System.out.println("(Input thread will be interrupted in 10 sec.)");
            // interrupt input in 10 sec
            (new TimeOut()).start();
            String line = null;
            while ((line = in.readLine()) != null) {
                System.out.println("Read line:'"+line+"'");
            }
        } catch (Exception ex) {
            System.out.println(ex.toString()); // printStackTrace();
        }
    }
    
    public static class TimeOut extends Thread {
        int sleepTime = 10000;
        Thread threadToInterrupt = null;    
        public TimeOut() {
            // interrupt thread that creates this TimeOut.
            threadToInterrupt = Thread.currentThread();
            setDaemon(true);
        }
        
        public void run() {
            try {
                sleep(10000); // wait 10 sec
            } catch(InterruptedException ex) {/*ignore*/}
            threadToInterrupt.interrupt();
        }
    }
}
````
&emsp;&emsp;这里还需要注意一点，`当线程处于写文件的状态时，调用interrupt()不会中断线程。`

 
### 不提倡的stop()方法 

&emsp;&emsp;臭名昭著的stop()停止线程的方法已不提倡使用了,原因是什么呢?
&emsp;&emsp;当在一个线程对象上调用stop()方法时，这个线程对象所运行的线程就会立即停止，并抛出特殊的ThreadDeath()异常。这里的“立即”因为太“立即”了，

假如一个线程正在执行：

````
synchronized void {
 x = 3;
 y = 4;
}
 ````

&emsp;&emsp;由于方法是同步的，多个线程访问时总能保证x,y被同时赋值，而如果一个线程正在执行到x = 3;时，被调用了 stop()方法，即使在同步块中，它也干脆地stop了，这样就产生了不完整的残废数据。而多线程编程中最最基础的条件要保证数据的完整性，所以请忘记 线程的stop方法，以后我们再也不要说“停止线程”了。

&emsp;&emsp;如何才能“结束”一个线程？


### interupt()中断线程

&emsp;&emsp;一个线程从运行到真正的结束，应该有三个阶段：

&emsp;&emsp;正常运行.
&emsp;&emsp;处理结束前的工作,也就是准备结束.
&emsp;&emsp;结束退出.
&emsp;&emsp;那么如何让一个线程结束呢？既然不能调用stop，可用的只的interrupt()方法。但interrupt()方法只是改变了线程的运行状态，如何让它退出运行？对于一般逻辑，只要线程状态已经中断，我们就可以让它退出，这里我们定义一个线程类ThreadA,所以这样的语句可以保证线程在中断后就能结束运行：

````
 while(!isInterrupted()){
  正常逻辑
 }
````

&emsp;&emsp;一个测试类,ThreadDemo
&emsp;&emsp;这样ThreadDemo调用interrupt()方法，isInterrupted()为true，就会退出运行。但是如果线程正在执行wait,sleep,join方法，你调用interrupt()方法，这个逻辑就不完全了。


我们可以这样处理:

````
 public void run(){
  
  while(!isInterrupted()){
   try{
    正常工作
   }catch(InterruptedException e){
    //nothing
   }
  
  }
 } 
}
````
&emsp;&emsp;想一想，如果一个正在sleep的线程，在调用interrupt后，会如何？wait方法检查到isInterrupted()为true，抛出异常， 而你又没有处理。而一个抛出了InterruptedException的线程的状态马上就会被置为非中断状态，如果catch语句没有处理异常，则下一 次循环中isInterrupted()为false，线程会继续执行，可能你N次抛出异常，也无法让线程停止。
这个错误情况的实例代码

ThreadA

````
public class ThreadA extends Thread {
   int count=0;
   public void run(){
       System.out.println(getName()+"将要运行...");
       while(!this.isInterrupted()){
           System.out.println(getName()+"运行中"+count++);
           try{
               Thread.sleep(400);
           }catch(InterruptedException e){
               System.out.println(getName()+"从阻塞中退出...");
               System.out.println("this.isInterrupted()="+this.isInterrupted());

           }
       }
       System.out.println(getName()+"已经终止!");
   }
}
````

ThreadDemo

````
public class ThreadDemo {
    
    public static void main(String argv[])throws InterruptedException{
        ThreadA ta=new ThreadA();
        ta.setName("ThreadA");
        ta.start();
        Thread.sleep(2000);
        System.out.println(ta.getName()+"正在被中断...");
        ta.interrupt();
        System.out.println("ta.isInterrupted()="+ta.isInterrupted());
    }

}
````

&emsp;&emsp;那么如何能确保线程真正停止？在线程同步的时候我们有一个叫“二次惰性检测”(double check)，能在提高效率的基础上又确保线程真正中同步控制中。那么我把线程正确退出的方法称为“双重安全退出”，即不以isInterrupted ()为循环条件。而以一个标记作为循环条件：

正确的ThreadA代码是:
 
````
public class ThreadA extends Thread {
    private boolean isInterrupted=false;
   int count=0;
   
   public void interrupt(){
       isInterrupted = true;
       super.interrupt();
      }
   
   public void run(){
       System.out.println(getName()+"将要运行...");
       while(!isInterrupted){
           System.out.println(getName()+"运行中"+count++);
           try{
               Thread.sleep(400);
           }catch(InterruptedException e){
               System.out.println(getName()+"从阻塞中退出...");
               System.out.println("this.isInterrupted()="+this.isInterrupted());

           }
       }
       System.out.println(getName()+"已经终止!");
   }
}
````
 
&emsp;&emsp; 在java多线程编程中，线程的终止可以说是一个必然会遇到的操作。但是这样一个常见的操作其实并不是一个能够轻而易举实现的操作，而且在某些场景下情况会变得更复杂更棘手。

&emsp;&emsp;Java标准API中的Thread类提供了stop方法可以终止线程，但是很遗憾，这种方法不建议使用，原因是这种方式终止线程中断临界区代码执行，并会释放线程之前获取的监控器锁，这样势必引起某些对象状态的不一致（因为临界区代码一般是原子的，不会被干扰的），具体原因可以参考资料[1]。这样一来，就必须根据线程的特点使用不同的替代方案以终止线程。根据停止线程时线程执行状态的不同有如下停止线程的方法。

#### 处于运行状态的线程停止

&emsp;&emsp;处于运行状态的线程就是常见的处于一个循环中不断执行业务流程的线程，这样的线程需要通过设置停止变量的方式，在每次循环开始处判断变量是否改变为停止，以达到停止线程的目的，比如如下代码框架：

````
private volatile Thread blinker;  
public void stop() {  
        blinker = null;  
}  
public void run() {  
        Thread thisThread = Thread.currentThread();  
        while (blinker == thisThread) {  
            try {  
                //业务流程  
            } catch (Exception e){}  
        }  
}<span style="font-size:12px;">  
</span>  
````

&emsp;&emsp;如果主线程调用该线程对象的stop方法，blinker对象被设置为null，则线程的下次循环中blinker！=thisThread，因而可以退出循环，并退出run方法而使线程结束。将引用变量blinker的类型前加上volatile关键字的目的是防止编译器对该变量存取时的优化，这种优化主要是缓存对变量的修改，这将使其他线程不会立刻看到修改后的blinker值，从而影响退出。此外，Java标准保证被volatile修饰的变量的读写都是原子的。

&emsp;&emsp;上述的Thread类型的blinker完全可以由更为简单的boolean类型变量代替。

 

####  即将或正在处于非运行态的线程停止

&emsp;&emsp; 线程的非运行状态常见的有如下两种情况：

&emsp;&emsp;可中断等待：线程调用了sleep或wait方法，这些方法可抛出InterruptedException；

&emsp;&emsp;Io阻塞：线程调用了IO的read操作或者socket的accept操作，处于阻塞状态。

#### 处于可中断等待线程的停止

&emsp;&emsp;如果线程调用了可中断等待方法，正处于等待状态，则可以通过调用Thread的interrupt方法让等待方法抛出InterruptedException异常，然后在循环外截获并处理异常，这样便跳出了线程run方法中的循环，以使线程顺利结束。

&emsp;&emsp;上述的stop方法中需要做的修改就是在设置停止变量之后调用interrupt方法：

````
private volatile Thread blinker;  
public void stop() {  
        Thread tmp = blinker;  
        blinker = null;  
        if(tmp!=null){  
            tmp.interrupt();  
        }  
}  
````

&emsp;&emsp;特别的，Thread对象的interrupt方法会设置线程的interruptedFlag，所以我们可以通过判断Thread对象的isInterrupted方法的返回值来判断是否应该继续run方法内的循环，从而代替线程中的volatile停止变量。这时的上述run方法的代码框架就变为如下：

````
public void run() {  
        while (!Thread.currentThread().isInterrupted()) {  
            try {  
                //业务流程  
            } catch (Exception e){}  
        }  
} 
````

&emsp;&emsp;需要注意的是Thread对象的isInterrupted不会清除interrupted标记，但是Thread对象的interrupted方法（与interrupt方法区别）会清除该标记。

#### 处于IO阻塞状态线程的停止
&emsp;&emsp;Java中的输入输出流并没有类似于Interrupt的机制，但是Java的InterruptableChanel接口提供了这样的机制，任何实现了InterruptableChanel接口的类的IO阻塞都是可中断的，中断时抛出ClosedByInterruptedException，也是由Thread对象调用Interrupt方法完成中断调用。IO中断后将关闭通道。

&emsp;&emsp;以文件IO为例，构造一个可中断的文件输入流的代码如下：

````
new InputStreamReader(  
           Channels.newInputStream(  
                   (new FileInputStream(FileDescriptor.in)).getChannel()))); 
````

&emsp;&emsp;实现InterruptableChanel接口的类包括FileChannel,ServerSocketChannel, SocketChannel, Pipe.SinkChannel andPipe.SourceChannel，也就是说，原则上可以实现文件、Socket、管道的可中断IO阻塞操作。

&emsp;&emsp;虽然解除IO阻塞的方法还可以直接调用IO对象的Close方法，这也会抛出IO异常。但是InterruptableChanel机制能够使处于IO阻塞的线程能够有一个和处于中断等待的线程一致的线程停止方案。

#### 处于大数据IO读写中的线程停止
&emsp;&emsp;处于大数据IO读写中的线程实际上处于运行状态，而不是等待或阻塞状态，因此上面的interrupt机制不适用。线程处于IO读写中可以看成是线程运行中的一种特例。停止这样的线程的办法是强行close掉io输入输出流对象，使其抛出异常，进而使线程停止。

&emsp;&emsp;最好的建议是将大数据的IO读写操作放在循环中进行，这样可以在每次循环中都有线程停止的时机，这也就将问题转化为如何停止正在运行中的线程的问题了。

#### 在线程运行前停止线程
&emsp;&emsp;有时，线程中的run方法需要足够健壮以支持在线程实际运行前终止线程的情况。即在Thread创建后，到Thread的start方法调用前这段时间，调用自定义的stop方法也要奏效。从上述的停止处于等待状态线程的代码示例中，stop方法并不能终止运行前的线程，因为在Thread的start方法被调用前，调用interrupt方法并不会将Thread对象的中断状态置位，这样当run方法执行时，currentThread的isInterrupted方法返回false，线程将继续执行下去。

&emsp;&emsp; 为了解决这个问题，不得不自己再额外创建一个volatile标志量，并将其加入run方法的最开头：

````
public void run() {  
        if (myThread == null) {  
           return; // stopped before started.  
        }  
        while(!Thread.currentThread().isInterrupted()){  
    //业务逻辑  
        }  
}
````

&emsp;&emsp;还有一种解决方法，也可以在run中直接使用该自定义标志量，而不使用isInterrupted方法判断线程是否应该停止。这种方法的run代码框架实际上和停止运行时线程的一样。


## 保证线程同步的方法汇总（未完待续）