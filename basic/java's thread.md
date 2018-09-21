---
title: java's thread 
tags: java,thread,basic
grammar_cjkRuby: true
---
[参考](https://www.cnblogs.com/duanxz/category/1026463.html)

## 进程和线程

## Java 多线程编程
&emsp;&emsp;Java 给多线程编程提供了内置的支持。 `一条线程指的是进程中一个单一顺序的控制流`，一个进程中可以并发多个线程，每条线程并行执行不同的任务。
多线程是多任务的一种特别的形式，但多线程使用了更小的资源开销。
&emsp;&emsp;这里定义和线程相关的另一个术语 - 进程：一个进程包括由操作系统分配的内存空间，包含一个或多个线程。`一个线程不能独立的存在，它必须是进程的一部分`。一个进程一直运行，直到所有的非守护线程都结束运行后才能结束。
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
&emsp;&emsp;&emsp;&emsp;如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种：

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

　　waite() 和notify()必须在synchronized函数或synchronized　block中进行调用。如果在non-synchronized函数或non-synchronized　block中进行调用，虽然能编译通过，但在运行时会发生 IllegalMonitorStateException的异常。

#### yield方法

　　暂停当前正在执行的线程对象。

　　yield()只是使当前线程重新回到可执行状态，所以执行yield()的线程有可能在进入到可执行状态后马上又被执行。

　　yield()只能使同优先级或更高优先级的线程有执行的机会。

#### join方法

　　等待该线程终止。

　　等待调用join方法的线程结束，再继续执行。如：t.join();//主要用于等待t线程运行结束，若无此句，main则会执行完毕，导致结果不可预测。