---
title: Singleton
tags: spring,singleton
grammar_cjkRuby: true
---

&emsp;&emsp;如果我的程序要很多次调用某个配置文件，那么我们是不是每次都是要new出一个实例出来呢，这样的话我们的程序跑起来的话就可以很慢，很吃力，内存就很快溢出了，一个相同的东西，我们要多次去产生它的实例，这多麻烦啊，如果只产生一个实例那该多好啊，所有就有了单例模式的出现了

## 单例模式定义：

&emsp;&emsp;单例模式确保某个类只有一个实例，而且自行实例化并向整个系统提供这个实例。在计算机系统中，线程池、缓存、日志对象、对话框、打印机、显卡的驱动程序对象常被设计成单例。这些应用都或多或少具有资源管理器的功能。每台计算机可以有若干个打印机，但只能有一个Printer Spooler，以避免两个打印作业同时输出到打印机中。每台计算机可以有若干通信端口，系统应当集中管理这些通信端口，以避免一个通信端口同时被两个请求同时调用。总之，选择`单例模式就是为了避免不一致状态，避免政出多头`。

### 单例模式特点：
　　1、单例类只能有一个实例。
　　2、单例类必须`自己创建自己的唯一实例`。
　　3、单例类`必须给所有其他对象提供这一实例`。

&emsp;&emsp;单例模式保证了全局对象的唯一性，比如系统启动读取配置文件就需要单例保证配置的一致性。

### 线程安全的问题

&emsp;&emsp;一方面在获取单例的时候，要保证不能产生多个实例对象，后面会详细讲到五种实现方式；
&emsp;&emsp;另一方面，在使用单例对象的时候，要注意`单例对象内的实例变量是会被多线程共享的`，推荐使用无状态的对象，不会因为多个线程的交替调度而破坏自身状态导致线程安全问题，比如我们常用的VO，DTO等（局部变量是在用户栈中的，而且`用户栈本身就是线程私有的内存区域，所以不存在线程安全问题`）。

### 单例模式的选择

&emsp;&emsp;还记得我们最早使用的MVC框架Struts1中的action就是单例模式的，而到了Struts2就使用了多例。在Struts1里，当有多个请求访问，每个都会分配一个新线程，在这些线程，操作的都是同一个action对象，每个用户的数据都是不同的，而action却只有一个。到了Struts2， action对象为每一个请求产生一个实例，并不会带来线程安全问题。

### Spring中的单例模式[详细请见同目录下的spring's singleton](https://raw.githubusercontent.com/only-wjt/java/master/basic/spring's%20singleton.md)

&emsp;&emsp;Spring单例Bean与单例模式的区别在于它们关联的环境不一样，`单例模式是指在一个JVM进程中仅有一个实例`，而Spring单例是指一个Spring Bean容器(ApplicationContext)中仅有一个实例。

&emsp;&emsp;Spring的单例Bean是与其容器（ApplicationContext）密切相关的，所以在一个JVM进程中，`如果有多个Spring容器，即使是单例bean，也一定会创建多个实例`，代码示例如下：

````
//  第一个Spring Bean容器
ApplicationContext context_1 = new FileSystemXmlApplicationContext("classpath:/ApplicationContext.xml");
Person yiifaa_1 = context_1.getBean("yiifaa", Person.class);
//  第二个Spring Bean容器
ApplicationContext context_2 = new FileSystemXmlApplicationContext("classpath:/ApplicationContext.xml");
Person yiifaa_2 = context_2.getBean("yiifaa", Person.class);
//  这里绝对不会相等，因为创建了多个实例
System.out.println(yiifaa_1 == yiifaa_2);
````

以下是Spring的配置文件：

````
<!-- 即使声明了为单例，只要有多个容器，也一定会创建多个实例 -->
<bean id="yiifaa" class="com.stixu.anno.Person" scope="singleton">
    <constructor-arg name="username">
        <value>yiifaa</value>
    </constructor-arg>
</bean>
````
&emsp;&emsp;总结:Spring的单例bean与Spring bean管理容器密切相关，每个容器都会创建自己独有的实例，所以与GOF设计模式中的单例模式相差极大，但在实际应用中，如果将对象的生命周期完全交给Spring管理(不在其他地方通过new、反射等方式创建)，其实也能达到单例模式的效果。


## 实现单例模式的方式

### 饿汉式单例（立即加载方式）

````
	// 饿汉式单例
	public class Singleton1 {
    // 私有构造
    	private Singleton1() {}
   	 	private static Singleton1 single = new Singleton1();
    // 静态工厂方法
   	 	public static Singleton1 getInstance() {
        return single;
    	}
	}
````

&emsp;&emsp;饿汉式单例在`类加载初始化时就创建好一个静态的对象供外部使用`，除非系统重启，这个对象不会改变，所以本身就是线程安全的。

&emsp;&emsp;Singleton通过将`构造方法限定为private避免了类在外部被实例化`，`在同一个虚拟机范围内，Singleton的唯一实例只能通过getInstance()方法访问`。（事实上，通过Java反射机制是能够实例化构造方法为private的类的，那基本上会使所有的Java单例实现失效。此问题在此处不做讨论，姑且闭着眼就认为反射机制不存在。）

2.懒汉式单例（延迟加载方式）

复制代码
// 懒汉式单例
public class Singleton2 {

    // 私有构造
    private Singleton2() {}

    private static Singleton2 single = null;

    public static Singleton2 getInstance() {
        if(single == null){
            single = new Singleton2();
        }
        return single;
    }
}
复制代码
 

该示例虽然用延迟加载方式实现了懒汉式单例，但在多线程环境下会产生多个single对象，如何改造请看以下方式:

使用synchronized同步锁

复制代码
public class Singleton3 {
    // 私有构造
    private Singleton3() {}

    private static Singleton3 single = null;

    public static Singleton3 getInstance() {
        
        // 等同于 synchronized public static Singleton3 getInstance()
        synchronized(Singleton3.class){
          // 注意：里面的判断是一定要加的，否则出现线程安全问题
            if(single == null){
                single = new Singleton3();
            }
        }
        return single;
    }
}
复制代码
在方法上加synchronized同步锁或是用同步代码块对类加同步锁，此种方式虽然解决了多个实例对象问题，但是该方式运行效率却很低下，下一个线程想要获取对象，就必须等待上一个线程释放锁之后，才可以继续运行。

复制代码
public class Singleton4 {
    // 私有构造
    private Singleton4() {}

    private static Singleton4 single = null;

    // 双重检查
    public static Singleton4 getInstance() {
        if (single == null) {
            synchronized (Singleton4.class) {
                if (single == null) {
                    single = new Singleton4();
                }
            }
        }
        return single;
    }
}
复制代码
 

使用双重检查进一步做了优化，可以避免整个方法被锁，只对需要锁的代码部分加锁，可以提高执行效率。

3.静态内部类实现

复制代码
public class Singleton6 {
    // 私有构造
    private Singleton6() {}

    // 静态内部类
    private static class InnerObject{
        private static Singleton6 single = new Singleton6();
    }
    
    public static Singleton6 getInstance() {
        return InnerObject.single;
    }
}
复制代码
 

静态内部类虽然保证了单例在多线程并发下的线程安全性，但是在遇到序列化对象时，默认的方式运行得到的结果就是多例的。这种情况不多做说明了，使用时请注意。

4.static静态代码块实现

复制代码
public class Singleton6 {
    
    // 私有构造
    private Singleton6() {}
    
    private static Singleton6 single = null;

    // 静态代码块
    static{
        single = new Singleton6();
    }
    
    public static Singleton6 getInstance() {
        return single;
    }
}
复制代码
 

5.内部枚举类实现

复制代码
public class SingletonFactory {
    
    // 内部枚举类
    private enum EnmuSingleton{
        Singleton;
        private Singleton8 singleton;
        
        //枚举类的构造方法在类加载是被实例化 
        private EnmuSingleton(){
            singleton = new Singleton8();
        }
        public Singleton8 getInstance(){
            return singleton;
        }
    }
    public static Singleton8 getInstance() {
        return EnmuSingleton.Singleton.getInstance();
    }
}

class Singleton8{
    public Singleton8(){}
}
复制代码
 

以上就是本文要介绍的所有单例模式的理解和实现，相信这篇文章能让大家更清楚的理解单例模式，希望大家有问题可以探讨，多多指教！