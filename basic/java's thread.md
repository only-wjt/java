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

&emsp;&emsp;如果对象调用了wait方法就会使持有该对象的线程把该对象的控制权交出去，然后处于等待状态。
&emsp;&emsp;如果对象调用了notify方法就会通知某个正在等待这个对象的控制权的线程可以继续运行。
&emsp;&emsp;如果对象调用了notifyAll方法就会通知所有等待这个对象控制权的线程继续运行。
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

&emsp;&emsp;在wait和notify的地方都报错java.lang.IllegalMonitorStateException，前面也讲过，wait和notify方法一定要在synchronized里面，更具体点说有：

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
复制代码
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

&emsp;&emsp;notifyAll使所有原来在该对象上等待被notify的所有线程统统退出wait的状态，变成等待该对象上的锁，一旦该对象被解锁，他们就会去竞争。
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

