---
title: Singleton
tags: spring,singleton
grammar_cjkRuby: true
---


### Spring中的单例模式[详细请见同目录下的spring's singleton](https://github.com/only-wjt/java/blob/master/basic/spring's%20singleton.md)

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


## 单例模式概念以及优缺点：

### （1）定义：

&emsp;&emsp;要求一个类只能生成一个对象，所有对象对它的依赖相同。
### （2）优点：

&emsp;&emsp;1. 只有一个实例，减少内存开支。应用在一个经常被访问的对象上

&emsp;&emsp;2. 减少系统的性能开销，应用启动时，直接产生一单例对象，用永久驻留内存的方式。

&emsp;&emsp;3.避免对资源的多重占用

&emsp;&emsp;4.可在系统设置全局的访问点，优化和共享资源访问。

### （3）缺点：

&emsp;&emsp;1.一般没有接口，扩展困难。原因：接口对单例模式没有任何意义；要求“自行实例化”，并提供单一实例，接口或抽象类不可能被实例化。（当然，单例模式可以实现接口、被继承，但需要根据系统开发环境判断）

&emsp;&emsp;2.单例模式对测试是不利的。如果单例模式没完成，是不能进行测试的。

&emsp;&emsp;3.单例模式与单一职责原则有冲突。原因：一个类应该只实现一个逻辑，而不关心它是否是单例，是不是要单例取决于环境；单例模式把“要单例”和业务逻辑融合在一个类。

### （4）使用场景：
&emsp;&emsp;1.要求生成唯一序列化的环境

&emsp;&emsp;2.项目需要的一个共享访问点或共享的数据点

&emsp;&emsp;3.创建一个对象需要消耗资源过多的情况。如：要访问IO和 数据库等资源。

&emsp;&emsp;4.需要定义大量的静态常量和静态方法（如工具类）的环境。可以采用单例模式或者直接声明static的方式。

### （5）注意事项：

&emsp;&emsp;1.类中其他方法，尽量是static

&emsp;&emsp;2.注意JVM的垃圾回收机制。

&emsp;&emsp;如果一个单例对象在内存长久不使用，JVM就认为对象是一个垃圾。所以如果针对一些状态值，如果回收的话，应用就会出现故障。

## 采用单例模式来记录状态值的类的两大方法：

### &emsp;&emsp;（一）、由容器管理单例的生命周期。Java EE容器或者框架级容器，自行管理对象的生命周期。

### &emsp;&emsp;（二）状态随时记录。异步记录的方式或者使用观察者模式，记录状态变化，确保重新初始化也可从资源环境获得销毁前的数据。

## 各式各样的单例及其线程安全问题：

### （1）懒汉式单例：

&emsp;&emsp;意思：就是需要使用这个对象的时候才去创建这个对象。
````
//懒汉式单例
public class Singleton1 {
    private static Singleton1 singleton1=null;
    public Singleton1(){

    }
    public static Singleton1 getInstance(){
        if (singleton1==null){
            try {
                Thread.sleep(200);//我们知道初始化一个对象需要一定时间的嘛，我们用sleep假设这个时间
                singleton1 = new Singleton1();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        return singleton1;
    }
}
````

````
//测试线程
public class SingleThread1 extends Thread {
//哈希值对应的是唯一的嘛，如果不一样了，就说明使用的不是同一个对象咯。
    @Override
    public void run() {
        System.out.println(Singleton1.getInstance().hashCode());
    }

}
````

````
//测试类
public class SingletonTest {

    public static void main(String []args){
        SingleThread1[] thread1s = new SingleThread1[10];
        for (int i= 0;i<thread1s.length;i++){
            thread1s[i] = new SingleThread1();
        }
        for (int j = 0; j < thread1s.length; j++) {
            thread1s[j].start();
        }
    }
}

//打印的结果：
569219718
1259146238
565373737
732830316
679555294
1886445805
1557403724
635681435
622018771
1439317371
````

#### 线程安全的懒汉式单例设计：
&emsp;&emsp;1.锁住获取方法方式：

````
public class Singleton3 {
    private static Singleton3 instance = null;

    private Singleton3(){}
    //锁住获取方法的方式
    public synchronized static Singleton3 getInstance() {
        try {
            if(instance != null){
            }else{
                Thread.sleep(300);
                instance = new Singleton3();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return instance;
    }
}
````

&emsp;&emsp;锁住部分代码块的方式：

````
public class Singleton2 {
    private static Singleton2 instance = null;
     private Singleton2(){
     }
    public static Singleton2 getInstance() {
        try {
            //锁住代码块的方式
            synchronized (Singleton2.class) {
                if(instance != null){

                }else{
                    Thread.sleep(200);
                    instance = new Singleton2();
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return instance;
    }
}
````

&emsp;&emsp;锁住初始化对象操作的方式：但是！！！这不是线程安全的！！一会有这个方式的优化从而实现线程安全。为什么？？因为多个访问已经进入到创建的那里了。

````
public class Singleton4 {
    private static Singleton4 instance = null;
    private Singleton4(){}
    public static Singleton4 getInstance() {
        try {
            if(instance != null){
            }else{
                Thread.sleep(300);
                //只锁住初始化操作的方式
                synchronized (Singleton4.class) {
                    instance = new Singleton4();
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return instance;
    }
}
````

&emsp;&emsp;锁住初始化对象操作的方式，但有个再检查操作：

````
public class Singleton5 {
    //使用volatile关键字保其可见性
    volatile private static Singleton5 instance = null;
    private Singleton5(){}
    public static Singleton5 getInstance() {
        try {
            if(instance != null){
            }else{
                Thread.sleep(300);
                //锁住初始化操作的方式
                synchronized (Singleton5.class) {
                    if(instance == null){//二次检查
                        instance = new Singleton5();
                    }
                }
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return instance;
    }
}
````

&emsp;&emsp;使用了volatile关键字来保证其线程间的可见性；在同步代码块中使用二次检查，以保证其不被重复实例化。集合其二者，这种实现方式既保证了其高效性，也保证了其线程安全性。
解析volatile在此的作用：

&emsp;&emsp;volatile（涉及java内存模型的知识）会禁止CPU对内存访问重排序（并不一定禁止指令重排），也就是CPU执行初始化操作，那么他会保证其他CPU看到的操作顺序是1.给 instance 分配内存–2.调用 Singleton 的构造函数来初始化成员变量–3.将instance对象指向分配的内存空间（执行完这步 instance 就为非 null 了），（虽然在CPU内由于流水线多发射并不一定是这个顺序）
&emsp;&emsp;&emsp;&emsp;不使用volatile的问题是什么呢？？

&emsp;&emsp;在 JVM 的即时编译器中存在指令重排序的优化。也就是说上面的第二步和第三步的顺序是不能保证的，最终的执行顺序可能是 1-2-3 也可能是 1-3-2。如果是后者，则在 3 执行完毕、2 未执行之前，被线程二抢占了，这时 instance 已经是非 null 了（但却没有初始化），所以线程二会直接返回 instance，然后使用，然后顺理成章地报错。

&emsp;&emsp;用volatile的意义并不在于其他线程一定要去内存总读取instance，而在于它限制了CPU对内存操作的重拍序，使其他线程在看到3之前2一定是执行过的。

### 饿汉式单例：

&emsp;&emsp;意思是：类装载时就实例化该单例类


````
public class Singleton6 {
    //一初始化类就初始化这个单例了！！！
    private static Singleton6 singleton6= new Singleton6();
    private Singleton6(){

    }
    public static Singleton6 getInstance(){
        return singleton6;
    }
}
````

&emsp;&emsp;基于classloder机制避免了多线程的同步问题，不过，instance在类装载时就实例化，虽然导致类装载的原因有很多种，在单例模式中大多数都是调用getInstance方法， 但是也不能确定有其他的方式（或者其他的静态方法）导致类装载，这时候初始化instance显然没有达到lazy loading的效果。这个是没有懒加载的功能的！！！

#### 饿汉式单例变种：

````
public class Singleton7 {
    private static Singleton7 instance = null;
    static {
        instance = new Singleton7();
    }
    private Singleton7() {
    }
    public static Singleton7 getInstance() {
        return instance;
    }
}
````

### 静态内部类实现懒加载：

````
//静态内部类单例
public class Singleton8 {
    private static class SingletonHolder {
        private static final Singleton8 INSTANCE = new Singleton8();
    }
    private Singleton8 (){}
    public static final Singleton8 getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
````

&emsp;&emsp;同样利用了classloder的机制来保证初始化instance时只有一个线程，它跟饿汉式的两种方式不同的是：饿汉式的两种方式是只要Singleton类被装载了，那么instance就会被实例化（没有达到lazy loading效果），而这种方式是Singleton类被装载了，instance还未被初始化。因为SingletonHolder类没有被主动使用，只有显示通过调用getInstance方法时，才会显示装载SingletonHolder类，从而实例化instance。想象一下，如果实例化instance很消耗资源，我想让他延迟加载，另外一方面，我不希望在Singleton类加载时就实例化，因为我不能确保Singleton类还可能在其他的地方被主动使用从而被加载，那么这个时候实例化instance显然是不合适的。

### 静态内部类方式单例再度研究：序列化和反序列化问题：

````
public class MySingleton implements Serializable {

    private static final long serialVersionUID = 1L;

    //内部类
    private static class MySingletonHandler{
        private static MySingleton instance = new MySingleton();
    }

    private MySingleton(){}

    public static MySingleton getInstance() {
        return MySingletonHandler.instance;
    }
}
````

````
public class SaveAndReadForSingleton {
    public static void main(String[] args) {
        MySingleton singleton = MySingleton.getInstance();
        //创建个文件流
        File file = new File("MySingleton.txt");
        //使用节点流，直接与文件关联
        try {
            //写入文件
            FileOutputStream fos = new FileOutputStream(file);
            ObjectOutputStream oos = new ObjectOutputStream(fos);
            oos.writeObject(singleton);
            fos.close();
            oos.close();
            System.out.println(singleton.hashCode());
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

        try {
            //读取文件流
            FileInputStream fis = new FileInputStream(file);
            ObjectInputStream ois = new ObjectInputStream(fis);
            MySingleton rSingleton = (MySingleton) ois.readObject();
            fis.close();
            ois.close();
            System.out.println(rSingleton.hashCode());
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }

    }
}
````

&emsp;&emsp;这样的单例测试出来时，hash是不一样的，因为没有同步到序列化与反序列化问题。说明反序列化后返回的对象是重新实例化的，单例被破坏了。

&emsp;&emsp;解决：当JVM从内存中反序列化地”组装”一个新对象时,就会自动调用readResolve方法来返回我们指定好的对象，readResolve允许class在反序列化返回对象前替换、解析在流中读出来的对象。实现readResolve方法，一个class可以直接控制反序化返回的类型和对象引用。

````
public class MySingleton1 implements Serializable {

    private static final long serialVersionUID = 1L;

    //内部类
    private static class MySingletonHandler{
        private static MySingleton1 instance = new MySingleton1();
    }

    private MySingleton1(){}

    public static MySingleton1 getInstance() {
        return MySingletonHandler.instance;
    }

    //该方法在反序列化时会被调用，该方法不是接口定义的方法，有点儿约定俗成的感觉
    protected Object readResolve() throws ObjectStreamException {
        System.out.println("调用了readResolve方法！");
        return MySingletonHandler.instance;
    }
}

修改SaveAndReadForSingleton文件中的MySingleton，输出2133927002
调用了readResolve方法！解决序列化与反序列化问题！2133927002
````

### 枚举：

````
//枚举实现单例
public enum EnumSingletonFactory {
    singletonFactory;
    private EnumSingleton instance;
    private EnumSingletonFactory(){//枚举类的构造方法在类加载是被实例化
        instance = new EnumSingleton();
    }
    public EnumSingleton getInstance(){
        return instance;
    }
}
````

````
在thread中调用实现：
@Override  
    public void run() {     System.out.println(EnumFactory.singletonFactory.getInstance().hashCode());  
    }
````

&emsp;&emsp;但是此博客 引起我思考，是违反单一职责的，因为它暴露了枚举的细节，所以我们需要改造他。

````
//使用工厂来生成枚举类
//通过工厂类的静态方法去访问枚举类，然后通过枚举类访问它的单例。
public class ClassFactory {
    private enum MyEnumSingleton{
        singletonFactory;

        private EnumSingleton instance;

        private MyEnumSingleton(){//枚举类的构造方法在类加载是被实例化
            instance = new EnumSingleton();
        }

        public EnumSingleton getInstance(){
            return instance;
        }
    }

    public static EnumSingleton getInstance(){
        return MyEnumSingleton.singletonFactory.getInstance();
    }
}
````

````
在thread中调用实现：
 @Override  
    public void run() {   
        System.out.println(ClassFactory.getInstance().hashCode());  
    }  
````
&emsp;&emsp;枚举类的方式不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象。不过实际工程代码中，很少去用此方式。

### 推荐使用：

&emsp;&emsp;上述的各种单例都讲完了：基本是五种写法。懒汉，恶汉，双重校验锁，枚举和静态内部类。

#### （1）饿汉式单例。

&emsp;&emsp;原因：类的加载机制保证了，类初始化时，只执行一次静态代码块以及类变量初始化。直接保证了唯一性，保证了线程安全。（一般使用非静态代码块方式）

#### （2）静态内部类方式：

&emsp;&emsp;原因：懒加载呗！！！应用在一些十分巨大的单例bean中。