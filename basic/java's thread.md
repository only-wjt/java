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

和Runnable接口不一样，Callable接口提供了一个call（）方法作为线程执行体，call()方法比run()方法功能要强大。

》call()方法可以有返回值

》call()方法可以声明抛出异常

Java5提供了Future接口来代表Callable接口里call()方法的返回值，并且为Future接口提供了一个实现类FutureTask，这个实现类既实现了Future接口，还实现了Runnable接口，因此可以作为Thread类的target。在Future接口里定义了几个公共方法来控制它关联的Callable任务。

>boolean cancel(boolean mayInterruptIfRunning)：视图取消该Future里面关联的Callable任务

>V get()：返回Callable里call（）方法的返回值，调用这个方法会导致程序阻塞，必须等到子线程结束后才会得到返回值

>V get(long timeout,TimeUnit unit)：返回Callable里call（）方法的返回值，最多阻塞timeout时间，经过指定时间没有返回抛出TimeoutException

>boolean isDone()：若Callable任务完成，返回True

>boolean isCancelled()：如果在Callable任务正常完成前被取消，返回True

介绍了相关的概念之后，创建并启动有返回值的线程的步骤如下：

1】创建Callable接口的实现类，并实现call()方法，然后创建该实现类的实例（从java8开始可以直接使用Lambda表达式创建Callable对象）。

2】使用FutureTask类来包装Callable对象，该FutureTask对象封装了Callable对象的call()方法的返回值

3】使用FutureTask对象作为Thread对象的target创建并启动线程（因为FutureTask实现了Runnable接口）

4】调用FutureTask对象的get()方法来获得子线程执行结束后的返回值

代码实例：

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

---------------------

本文来自 愿好 的CSDN 博客 ，全文地址请点击：https://blog.csdn.net/m0_37840000/article/details/79756932?utm_source=copy 