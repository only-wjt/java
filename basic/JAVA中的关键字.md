---
title: JAVA中的关键字 
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---
[未完待续](https://blog.csdn.net/z1002137615/article/details/50943869)

## Static 

### static的用途

&emsp;&emsp;<java编程思想>中有这样一句话

> static方法就是没有this的方法。在static方法内部不能调用非静态方法，反过来是可以的。而且可以在没有创建
> 任何对象的前提下，仅仅通过类本身来调用static方法。这实际上正是static方法的主要用途。

&emsp;&emsp;这段话虽然只是说明了static方法的特殊之处，但是可以看出static关键字的基本作用，简而言之，一句话来描述就是：

&emsp;&emsp;`方便在没有创建对象的情况下来进行调用（方法/变量）`

&emsp;&emsp;很显然，被static关键字修饰的方法或者变量不需要依赖于对象来进行访问，只要类被加载了，就可以通过类名去进行访问。

&emsp;&emsp;static可以用来修饰类的成员方法、类的成员变量，另外可以编写static代码块来优化程序性能。

#### static修饰方法

&emsp;&emsp;static方法一般称作静态方法，由于静态方法不依赖于任何对象就可以进行访问，因此对于静态方法来说，是没有this的，因为它不依附于任何对象，既然都没有对象，就谈不上this了。并且由于这个特性，在静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用。

&emsp;&emsp;但是要注意的是，虽然在静态方法中不能访问非静态成员方法和非静态成员变量，但是在非静态成员方法中是可以访问静态成员方法/变量的。

&emsp;&emsp;而对于非静态成员方法，它访问静态成员方法/变量显然是毫无限制的。

&emsp;&emsp;因此，如果说想在不创建对象的情况下调用某个方法，就可以将这个方法设置为static。我们最常见的static方法就是main方法，至于为什么main方法必须是static的，现在就很清楚了。因为程序在执行main方法的时候没有创建任何对象，因此只有通过类名来访问。

&emsp;&emsp;有这么一个说法构造器是可以是静态的，这个说法是错误的，因为`构造器里面隐藏有this关键字`，但是static方法里面不能有this关键字，this是指调用当前方法的对象，而静态方法不属于任何对象。构造器只负责初始化，不负责创建实例

#### staitc修饰成员变量

&emsp;&emsp;static变量也称作静态变量，静态变量和非静态变量的区别是：静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。而非静态变量是对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。

&emsp;&emsp;static成员变量的初始化顺序按照定义的顺序进行初始化。

#### 修饰代码块

&emsp;&emsp;static关键字还有一个比较关键的作用就是 用来形成静态代码块以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

&emsp;&emsp;为什么说static块可以用来优化程序性能，是因为它的特性:只会在类加载的时候执行一次。下面看个例子:

```
class Person{
    private Date birthDate;
     
    public Person(Date birthDate) {
        this.birthDate = birthDate;
    }
     
    boolean isBornBoomer() {
        Date startDate = Date.valueOf("1946");
        Date endDate = Date.valueOf("1964");
        return birthDate.compareTo(startDate)>=0 && birthDate.compareTo(endDate) < 0;
    }
}
````

&emsp;&emsp;isBornBoomer是用来这个人是否是1946-1964年出生的，而每次isBornBoomer被调用的时候，都会生成startDate和birthDate两个对象，造成了空间浪费，如果改成这样效率会更好：

````
class Person{
    private Date birthDate;
    private static Date startDate,endDate;
    static{
        startDate = Date.valueOf("1946");
        endDate = Date.valueOf("1964");
    }
     
    public Person(Date birthDate) {
        this.birthDate = birthDate;
    }
     
    boolean isBornBoomer() {
        return birthDate.compareTo(startDate)>=0 && birthDate.compareTo(endDate) < 0;
    }
}
````

## static存在的误区

### static关键字会改变类中成员的访问权限吗？
&emsp;&emsp;在Java中能够影响到访问权限的只有private、public、protected（包括包访问权限）这几个关键字。

### 能通过this访问静态成员变量吗？

````
public class Main {　　
    static int value = 33;
 
    public static void main(String[] args) throws Exception{
        new Main().printValue();
    }
 
    private void printValue(){
        int value = 3;
        System.out.println(this.value);
    }
}
````

&emsp;&emsp;这里面主要考察队this和static的理解。this代表什么？this代表当前对象，那么通过new Main()来调用printValue的话，当前对象就是通过new Main()生成的对象。而static变量是被对象所享有的，因此在printValue中的this.value的值毫无疑问是33。在printValue方法内部的value是局部变量，根本不可能与this关联，所以输出结果是33。在这里永远要记住一点：静态成员变量虽然独立于对象，但是不代表不可以通过对象去访问，所有的静态方法和静态变量都可以通过对象访问（只要访问权限足够）。

### static能作用于局部变量么？

&emsp;&emsp;但是在Java中切记：static是不允许用来修饰局部变量。不要问为什么，这是Java语法的规定。

&emsp;&emsp;static 变量是给类用的。这样类初始化的时候，就会给static进行初始化.如果你在方法里面定义一个static。这时候编译器就不知道你这个变量怎么初始化了这个是和java的设计相关的。java全是面向对象设计的，单独一个方法不能持有一块空间。

## 总结

&emsp;&emsp;static可以修饰方法，修饰成员变量，修饰代码块，static内部不能调用非静态方法，可以在不创建对象的情况下，通过类本身来调用static方法和变量。被static修饰的方法、成员变量、代码块在类加载的时候加载，他是被所有的实例对象共享
````
栈内存：局部变量和对象的引用变量；
堆内存：对象；
````

&emsp;&emsp;Static是在堆内存的数据区

&emsp;&emsp;修饰方法时，应为static修饰的方法是属于类的，不需要通过对象进行调用，所以static方法里面没有`this关键字，所以同时也不能调用非静态变量和方法。Main方法`

&emsp;&emsp;修饰变量
&emsp;&emsp;静态变量是被所有的对象所共享的，在内存只有一份，是在类的加载而加载的。而非静态变量是属于实例对象的，在内存中有多个备份，创建的对象的时候初始化，存在多个副本，而且各个副本互不影响

&emsp;&emsp;修饰代码块
&emsp;&emsp;用来形成静态代码块以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

&emsp;&emsp;在单例模式中会用到static，详细请见同目录下的单例模式详解[单例模式](https://github.com/only-wjt/java/blob/master/basic/Singleton.md)


## abstract

&emsp;&emsp;使用了abstract关键字的类就是抽象类，使用了abstract关键字的方法就是抽象方法。按照常理来说，抽象类会包含一个以上的抽象方法。言下之意，你非要让一个抽象类不包含抽象方法，也是可以的。但是只要包含了一个抽象方法，这个类就必须是抽象类。抽象类（抽象方法）通常都是继承时会用的到，是java多态性的体现。

&emsp;&emsp;创建抽象类和方法有时对我们非常有用，因为它们使一个类的抽象变成明显的事实，可明确告诉用户和编译器自己打算如何用它。

&emsp;&emsp;abstract用法其实比较简单：

````
abstract class Instrument4 {
	int i; // storage allocated for each
	public abstract void play();
	public String what() {
	    return "Instrument4";
    }
    public abstract void adjust();
}
 
class Wind4 extends Instrument4 {
	public void play() {
	    System.out.println("Wind4.play()");
	}
	public String what() {
		return "Wind4"; 
	}
	public void adjust() {}
}
 
class Percussion4 extends Instrument4 {
	public void play() {
	    System.out.println("Percussion4.play()");
	}
	public String what() {
		return "Percussion4";
	}
	public void adjust() {}
}
 
class Stringed4 extends Instrument4 {
	public void play() {
	    System.out.println("Stringed4.play()");
	}
	public String what() {
		return "Stringed4";
	}
	public void adjust() {}
}
 
class Brass4 extends Wind4 {
	public void play() {
	    System.out.println("Brass4.play()");
	}
	public void adjust() {
	    System.out.println("Brass4.adjust()");
	}
}
 
class Woodwind4 extends Wind4 {
	public void play() {
	    System.out.println("Woodwind4.play()");
	}
	public String what() {
		return "Woodwind4"; 
	}
}
 
class Music4 {
	// Doesn't care about type, so new types
	// added to the system still work right:
	static void tune(Instrument4 i) {
		// ...
		i.play();
	}
	static void tuneAll(Instrument4[] e) {
		for(int i = 0; i < e.length; i++)
		    tune(e[i]);
	}
	public static void main(String[] args) {
		Instrument4[] orchestra = new Instrument4[5];
		int i = 0;
		// Upcasting during addition to the array:
		orchestra[i++] = new Wind4();
		orchestra[i++] = new Percussion4();
		orchestra[i++] = new Stringed4();
		orchestra[i++] = new Brass4();
		orchestra[i++] = new Woodwind4();
		tuneAll(orchestra);
	}
} 

````

&emsp;&emsp;注意点：

&emsp;&emsp;1.一个抽象类不一定有抽象方法，但只要有一个抽象方法就一定是抽象类。

&emsp;&emsp;2.抽象类不可创建新对象。但是抽象类允许有构造方法。（子类可以继承构造方法）

&emsp;&emsp;3.子类继承抽象类必须实现其中抽象方法，除非子类也是抽象类。

&emsp;&emsp;4.抽象类的非抽象方法都可以被正常调用。调用方式包括通过子类调用，直接调用static方法等。

&emsp;&emsp;5.abstract 不能与private、static、final、native、synchronized共用。（相互之间矛盾，直接编译报错）

## interface
&emsp;&emsp;接口是抽象方法的集合。如果一个类实现了某个接口，那么它就继承了这个接口的抽象方法。这就像契约模式，如果实现了这个接口，那么就必须确保使用这些方法。接口只是一种形式，接口自身不能做任何事情。

### 接口和抽象类的区别

![接口和抽象类的区别](./images/接口和抽象类的区别.png)

### 什么时候使用抽象类和接口

&emsp;&emsp;如果你拥有一些方法并且想让它们中的一些有默认实现，那么使用抽象类吧。

&emsp;&emsp;如果你想实现多重继承，那么你必须使用接口。由于Java不支持多继承，子类不能够继承多个类，但可以实现多个接口。因此你就可以使用接口来解决它。

&emsp;&emsp;如果基本功能在不断改变，那么就需要使用抽象类。如果不断改变基本功能并且使用接口，那么就需要改变所有实现了该接口的类。


## final[转载](http://www.cnblogs.com/dolphin0520/p/3736238.html)

&emsp;&emsp;前言：final关键字一般会在匿名内部类用到，而且，我们常用的String也是用fianl修饰的

### final关键字的基本用法

&emsp;&emsp;在Java中，final关键字可以用来修饰类、方法和变量（包括成员变量和局部变量）。

#### final用来修饰类

&emsp;&emsp;当用final修饰一个类时，表明这个类不能被继承。也就是说，如果一个类你永远不会让他被继承，就可以用final进行修饰。final类中的成员变量可以根据需要设为final，但是要注意`final类中的所有成员方法都会被隐式地指定为final方法。`

&emsp;&emsp;`在使用final修饰类的时候，要注意谨慎选择，除非这个类真的在以后不会用来继承或者出于安全的考虑，尽量不要将类设计为final类。`

#### final用来修饰方法

&emsp;&emsp;<java编程思想>

> 使用final方法的原因有两个。第一个原因是把方法锁定，以防任何继承类修改它的含义；第二个原因是效率。在早期的Java实现版本中，会将final方法
> 转为内嵌调用。但是如果方法过于庞大，可能看不到内嵌调用带来的任何性能提升。在最近的Java版本中，不需要使用final方法进行这些优化了。

&emsp;&emsp;因此，如果只有在想明确禁止 该方法在子类中被覆盖的情况下才将方法设置为final的。

&emsp;&emsp;注：类的private方法会隐式地被指定为final方法。

#### fianl用来修饰变量

&emsp;&emsp;修饰变量是final用得最多的地方，也是本文接下来要重点阐述的内容。首先了解一下final变量的基本语法：

&emsp;&emsp;对于一个final变量，`如果是基本数据类型的变量，则其数值一旦在初始化之后便不能更改；``如果是引用类型的变量，则在对其初始化之后便不能再让其指向另一个对象。`


### 深入了解final关键字

#### 类的final变量和普通变量有什么区别？

&emsp;&emsp;当用final作用于类的成员变量时，成员变量（注意是类的成员变量，局部变量只需要保证在使用之前被初始化赋值即可）必须在定义时或者构造器中进行初始化赋值，而且final变量一旦被初始化赋值之后，就不能再被赋值了。

````
public class Test {
    public static void main(String[] args)  {
        String a = "hello2"; 
        final String b = "hello";
        String d = "hello";
        String c = b + 2; 
        String e = d + 2;
        System.out.println((a == c));
        System.out.println((a == e));
    }
}
````

````
true
false
````

&emsp;&emsp;为什么第一个比较结果为true，而第二个比较结果为fasle。这里面就是final变量和普通变量的区别了，当final变量是基本数据类型以及String类型时，如果在编译期间能知道它的确切值，则编译器会把它当做编译期常量使用。也就是说在用到该final变量的地方，相当于直接访问的这个常量，不需要在运行时确定。这种和C语言中的宏替换有点像。因此在上面的一段代码中，由于变量b被final修饰，因此会被当做编译器常量，所以在使用到b的地方会直接将变量b 替换为它的  值。而对于变量d的访问却需要在运行时通过链接来进行。想必其中的区别大家应该明白了，不过要注意，只有在编译期间能确切知道final变量值的情况下，编译器才会进行这样的优化，比如下面的这段代码就不会进行优化：

````
public class Test {
    public static void main(String[] args)  {
        String a = "hello2"; 
        final String b = getHello();
        String c = b + 2; 
        System.out.println((a == c));
 
    }
     
    public static String getHello() {
        return "hello";
    }
}
````

#### 被final修饰的引用变量指向的对象内容可变吗？(可变)

````
public class Test {
    public static void main(String[] args)  {
        final MyClass myClass = new MyClass();
        System.out.println(++myClass.i);
 
    }
}
 
class MyClass {
    public int i = 0;
}
````

&emsp;&emsp;这段代码可以顺利编译通过并且有输出结果，输出结果为1。这说明引用变量被final修饰之后，虽然不能再指向其他对象，但是它指向的对象的内容是可变的。

#### final和static

&emsp;&emsp;很多时候会容易把static和final关键字混淆，static作用于成员变量用来表示只保存一份副本，而final的作用是用来保证变量不可变。看下面这个例子：

````
public class Test {
    public static void main(String[] args)  {
        MyClass myClass1 = new MyClass();
        MyClass myClass2 = new MyClass();
        System.out.println(myClass1.i);
        System.out.println(myClass1.j);
        System.out.println(myClass2.i);
        System.out.println(myClass2.j);
 
    }
}
 
class MyClass {
    public final double i = Math.random();
    public static double j = Math.random();
}
````
&emsp;&emsp;运行这段代码就会发现，每次打印的两个j值都是一样的，而i的值却是不同的。从这里就可以知道final和static变量的区别了。

#### 匿名内部类中使用的外部局部变量为什么只能是final变量？

见内部类（暂时未完成）

#### 关于final参数的问题

&emsp;&emsp;关于网上流传的”当你在方法中不需要改变作为参数的对象变量时，明确使用final进行声明，会防止你无意的修改而影响到调用方法外的变量“这句话，我个人理解这样说是不恰当的。

&emsp;&emsp;因为无论参数是基本数据类型的变量还是引用类型的变量，使用final声明都不会达到上面所说的效果。

````
  public class Test {
    public static void main(String[] args)  {
        MyClass myClass = new MyClass();
        StringBuffer buffer = new StringBuffer("hello");
        myClass.changeValue(buffer);
        System.out.println(buffer.toString());
    }
}
 
class MyClass {
     
    void changeValue(final StringBuffer buffer) {
        buffer.append("world");
    }
}
````

&emsp;&emsp;运行这段代码就会发现输出结果为 helloworld。很显然，用final进行修饰并没有阻止在changeValue中改变buffer指向的对象的内容。有人说假如把final去掉了，万一在changeValue中让buffer指向了其他对象怎么办。有这种想法的朋友可以自己动手写代码试一下这样的结果是什么，如果把final去掉了，然后在changeValue中让buffer指向了其他对象，也不会影响到main方法中的buffer，`原因在于java采用的是值传递，对于引用变量，传递的是引用的值，也就是说让实参和形参同时指向了同一个对象，因此让形参重新指向另一个对象对实参并没有任何影响。`

#### 下面总结了一些使用final关键字的好处

&emsp;&emsp;final关键字提高了性能。JVM和Java应用都会缓存final变量。
&emsp;&emsp;final变量可以安全的在多线程环境下进行共享，而不需要额外的同步开销。
&emsp;&emsp;使用final关键字，JVM会对方法、变量及类进行优化。

#### 关于final的重要知识点

&emsp;&emsp;final关键字可以用于成员变量、本地变量、方法以及类。
&emsp;&emsp;final成员变量必须在声明的时候初始化或者在构造器中初始化，否则就会报编译错误。
&emsp;&emsp;你不能够对final变量再次赋值。
&emsp;&emsp;本地变量必须在声明时赋值。
&emsp;&emsp;在匿名类中所有变量都必须是final变量。
&emsp;&emsp;final方法不能被重写。
&emsp;&emsp;final类不能被继承。
&emsp;&emsp;final关键字不同于finally关键字，后者用于异常处理。
&emsp;&emsp;final关键字容易与finalize()方法搞混，后者是在Object类中定义的方法，是在垃圾回收之前被JVM调用的方法。
&emsp;&emsp;接口中声明的所有变量本身是final的。
&emsp;&emsp;final和abstract这两个关键字是反相关的，final类就不可能是abstract的。
&emsp;&emsp;final方法在编译阶段绑定，称为静态绑定(static binding)。
&emsp;&emsp;没有在声明时初始化final变量的称为空白final变量(blank final variable)，它们必须在构造器中初始化，或者调用this()初始化。不这么做的话，编译器会报错“final变量(变量名)需要进行初始化”。
&emsp;&emsp;将类、方法、变量声明为final能够提高性能，这样JVM就有机会进行估计，然后优化。
&emsp;&emsp;按照Java代码惯例，final变量就是常量，而且通常常量名要大写


## finally[详细见这里](https://blog.csdn.net/moliilom/article/details/50909096)
&emsp;&emsp;finally作为异常处理的一部分，它只能用在try/catch语句中，`并且附带一个语句块，表示这段语句最终一定会被执行（不管有没有抛出异常），`经常被用在需要释放资源的情况下。

&emsp;&emsp;之前在写爬虫的时候数据库连接的频率很高，有时候数据处理的不好，sql报错后，抛出异常但后边的数据库连接没有断开。导致最后数据库连接数过大，不让再连接了（因为是个人库，所以直接重启了一下）。这个释放数据库连接的操作就可以用finally来进行。

&emsp;&emsp;异常处理是大多数编程语言需要遇到的问题,java中的try-catch块和C++差不多,但是java中却多了一个finally块。这个块无论异常是否发生都会执行。但是如果在try块中或者在catch块中有return语句,那会先执行return语句吗?答案是否定的,程序会先执行finally语句块,然后再执行try块或catch块的return语句(这里先假设finally块中没有return语句,finally块中有return语句的下面再讨论)。

&emsp;&emsp;下面有一道题：

````
public static int func (){
    try{
        return 1;
    }catch (Exception e){
        return 2;
    }finally{
        return 3;
    }
}
````

&emsp;&emsp;最终输出3。


&emsp;&emsp;finally的优先覆盖
&emsp;&emsp;这里还需要指出，finally子句有最高的控制流返回权，其可以覆盖try、catch块内的任意Exception值、return值。如下的函数，将会返回12，而不是10。

````
public static int getMonthsInYear(){
    try{
        return 10;
    } finally {
        return 12;
    }
}
````

&emsp;&emsp;同样地，如下函数，最后并不会抛出异常，而是返回值。

````
public static int getMonthsInYear(){
    try{
        throw new RuntimeException();
    } finally {
        return 12;
    }
}
````

&emsp;&emsp;再对比这个函数，却抛出异常，并不返回值。

````
public static int getMonthsInYear(){
    try{
        return 12;          
    } finally {
        throw new RuntimeException();
    }
}
````

## finalize()方法

### Java垃圾回收机制

&emsp;&emsp;Java有垃圾回收期负责回收无用对象占据的内存空间。但也有特殊情况:假定你的对象(并非使用new)获得了一块“特殊”的内存区域，由于垃圾回收期只知道释放那些经由new分配的内存，所以它不知道该如何释放该对象的这块“特殊”内存。
&emsp;&emsp;Java允许在类中定义一个名为finalize()方法。一旦垃圾回收期准备好释放对象占用的内存空间，首先调用其finalize()方法，`并且在下一次垃圾回收动作发生时，才会真正回收对象占用的内存。`在Java中，对象并非总是被垃圾回收，只要程序没有濒临内存空间用完的那一刻，对象占用的空间就中也得不到释放。如果程序执行结束，并且垃圾回收器一直没有释放创建的对象，程序结束退出后资源也会全部返回给操作系统。
&emsp;&emsp;`这个垃圾回收策略避免了频繁垃圾回收造成的开销，Java虚拟机并未面临内存耗尽的情形，它是不会浪费时间去执行垃圾回收以恢复内存的`


### finalize方法是Object提供的的实例方法，使用规则如下：

&emsp;&emsp;当对象不再被任何对象引用时，GC会调用该对象的finalize()方法

&emsp;&emsp;finalize()是Object的方法，子类可以覆盖这个方法来做一些系统资源的释放或者数据的清理

&emsp;&emsp;可以在finalize()让这个对象再次被引用，避免被GC回收；但是最常用的目的还是做cleanup

&emsp;&emsp;Java不保证这个finalize()一定被执行；但是保证调用finalize的线程没有持有任何user-visible同步锁。

&emsp;&emsp;在finalize里面抛出的异常会被忽略，同时方法终止。

&emsp;&emsp;当finalize被调用之后，JVM会再一次检测这个对象是否能被存活的线程访问得到，如果不是，则清除该对象。也就是finalize只能被调用一次；也就是说，`覆盖了finalize方法的对象需要经过两个GC周期才能被清除。`

### finalize机制本身就是存在问题的。

&emsp;&emsp;finalize机制可能会导致性能问题，死锁和线程挂起。

&emsp;&emsp;finalize中的错误可能导致内存泄漏；如果不在需要时，也没有办法取消垃圾回收；并且没有指定不同执行finalize对象的执行顺序。此外，没有办法保证finlize的执行时间。

&emsp;&emsp;遇到这些情况，对象调用finalize方法只有被无限期延后。

&emsp;&emsp;Java9中finalize方法已经被废弃。

&emsp;&emsp;占有非堆资源的对象实例，类应该提供一个方法以明确释放这些资源，如果合适的话他们也应该实现AutoCloseable接口。


## Native[详细见这里](https://www.cnblogs.com/Qian123/p/5702574.html)

>public native int hashCode();

&emsp;&emsp;为什么会有native关键字的出现

### 认识 native 即 JNI,Java Native Interface

&emsp;&emsp;凡是一种语言，都希望是纯。比如解决某一个方案都喜欢就单单这个语言来写即可。Java平台有个用户和本地C代码进行互操作的API，称为Java Native Interface (Java本地接口)。

![JNI](./images/JNI.png)

### 用 Java 调用 C 的“Hello，JNI”

#### 1.创建一个Java类，里面包含着一个 native 的方法和加载库的方法 loadLibrary。

#### 2、运行javah，得到包含该方法的C声明头文件.h

#### 3、根据头文件，写C实现本地方法。

### 4、生成dll共享库，然后Java程序load库，调用即可。

![JNI调用C流程](./images/JNI调用C流程.png)

### 其他介绍

&emsp;&emsp;native是与C++联合开发的时候用的！java自己开发不用的！

&emsp;&emsp;使用native关键字说明这个方法是原生函数，也就是这个方法是用C/C++语言实现的，并且被编译成了DLL，由java去调用。
&emsp;&emsp;这些函数的实现体在DLL中，JDK的源代码中并不包含，你应该是看不到的。对于不同的平台它们也是不同的。这也是java的底层机制，实际上java就是在不同的平台上调用不同的native方法实现对操作系统的访问的。

&emsp;&emsp;1。native 是用做java 和其他语言（如c++）进行协作时用的也就是native 后的函数的实现不是用java写的
&emsp;&emsp;2。既然都不是java，那就别管它的源代码了，呵呵

&emsp;&emsp;native的意思就是通知操作系统，这个函数你必须给我实现，因为我要使用。所以native关键字的函数都是操作系统实现的，java只能调用。

&emsp;&emsp;java是跨平台的语言，既然是跨了平台，所付出的代价就是牺牲一些对底层的控制，而java要实现对底层的控制，就需要一些其他语言的帮助，这个就是native的作用了

&emsp;&emsp;Java不是完美的，Java的不足除了体现在运行速度上要比传统的C++慢许多之外，Java无法直接访问到操作系统底层（如系统硬件等)，为此Java使用native方法来扩展Java程序的功能。
&emsp;&emsp;可以将native方法比作Java程序同Ｃ程序的接口，其实现步骤：
&emsp;&emsp;１、在Java中声明native()方法，然后编译；
&emsp;&emsp;２、用javah产生一个.h文件；
&emsp;&emsp;３、写一个.cpp文件实现native导出方法，其中需要包含第二步产生的.h文件（注意其中又包含了JDK带的jni.h文件）；
&emsp;&emsp;４、将第三步的.cpp文件编译成动态链接库文件；
&emsp;&emsp;５、在Java中用System.loadLibrary()方法加载第四步产生的动态链接库文件，这个native()方法就可以在Java中被访问了。

&emsp;&emsp;JAVA本地方法适用的情况 
&emsp;&emsp;1.为了使用底层的主机平台的某个特性，而这个特性不能通过JAVA API访问

&emsp;&emsp;2.为了访问一个老的系统或者使用一个已有的库，而这个系统或这个库不是用JAVA编写的

&emsp;&emsp;3.为了加快程序的性能，而将一段时间敏感的代码作为本地方法实现。


## super

&emsp;&emsp;super关键字表示对某个类的父类的引用。一般而言，super有两种通用形式：第一种用来访问被子类的成员隐藏的父类成员；第二种则是可以调用父类的构造函数。接下来说一下两种使用形式的方法和规则。

&emsp;&emsp;第一种：如子类和父类有同名的成员变量或方法，则父类的成员将会被覆盖，此时可用下面的方式来引用父类的成员：

````
super.<成员变量名>
super.<成员方法名>
````

&emsp;&emsp;在Java语言中，`用过继承关系实现对成员的访问是按照最近匹配原则进行的，`规则如下：

&emsp;&emsp;（1）在子类中访问成员变量和方法时将优先查找是否在本类中已经定义，如果该成员在本类中存在，则使用本类的，否则，按照继承层次的顺序往父类查找，如果未找到，继续逐层向上到其祖先类查找。

&emsp;&emsp;（2）super特指访问父类的成员，使用super首先到直接父类查找匹配成员，如果未找到，再逐层向上到祖先类查找。

&emsp;&emsp;第二种：`子类可以通过super关键字调用父类中定义的构造方法，`格式如下：

&emsp;&emsp;super(调用参数列表),`其中调用参数列表必须和父类的某个构造函数方法的参数列表完全匹配。`

&emsp;&emsp;子类与其直接父类之间的构造方法存在约束关系，有以下几条重要原则：

&emsp;&emsp;（1）按继承关系，构造方法是从顶向下进行调用的。

&emsp;&emsp;（2）如果子类没有构造方法，则它默认调用父类无参的构造方法，如果父类中没有无参数的构造方法，则将产生错误。

&emsp;&emsp;（3）`如果子类有构造方法，那么创建子类的对象时，先执行父类的构造方法，再执行子类的构造方法。`子类的构造函数默认第一行会默认调用父类无参的构造函数，隐式语句

&emsp;&emsp;（4）如果子类有构造方法，但子类的构造方法中没有super关键字，则系统默认执行该构造方法时会产生super()代码，即该构造方法会调用父类无参数的构造方法。

&emsp;&emsp;（5）对于父类中包含有参数的构造方法，子类可以通过在自己的构造方法中使用super关键字来引用，而且必须是子类构造函数方法中的第一条语句。

&emsp;&emsp;（6）Java语言中规定当一个类中含有一个或多个有参构造方法，系统不提供默认的构造方法（即不含参数的构造方法），所以当父类中定义了多个有参数构造方法时，应考虑写一个无参数的构造方法，以防子类省略super关键字时出现错误。


## this

### this关键字主要有三个应用：
&emsp;&emsp; (1)this调用本类中的属性，也就是类中的成员变量；
 &emsp;&emsp;(2)this调用本类中的其他方法；
 &emsp;&emsp;(3)this调用本类中的其他构造方法，调用时要放在构造方法的首行。

 

### 引用成员变量
 
````
Public Class Student { 
 String name; //定义一个成员变量name
 private void SetName(String name) { //定义一个参数(局部变量)name
  this.name=name; //将局部变量的值传递给成员变量
 }
}
````

&emsp;&emsp;this这个关键字其代表的就是对象中的成员变量或者方法。也就是说，`如果在某个变量前面加上一个this关键字，其指的就是这个对象的成员变量或者方法，`而不是指成员方法的形式参数或者局部变量。

&emsp;&emsp;为此在上面这个代码中，this.name代表的就是对象中的成员变量，又叫做对象的属性，而后面的name则是方法的形式参数，代码this.name=name就是将形式参数的值传递给成员变量。
 

### 调用类的构造方法

````
public class Student { //定义一个类，类的名字为student。 
 public Student() { //定义一个方法，名字与类相同故为构造方法
  this(“Hello!”);
 }
 public Student(String name) { //定义一个带形式参数的构造方法
 }
}
````

&emsp;&emsp;在一个Java类中，`其方法可以分为成员方法和构造方法`两种。

&emsp;&emsp;构造方法是一个与类同名的方法，在Java类中必须存在一个构造方法。

&emsp;&emsp;如果在代码中没有显示的体现构造方法的话，那么编译器在编译的时候会自动添加一个没有形式参数的构造方法。这个构造方法跟普通的成员方法还是有很多不同的地方。

&emsp;&emsp;如构造方法一律是没有返回值的，而且也不用void关键字来说明这个构造方法没有返回值。而普通的方法可以有返回值、也可以没有返回值，程序员可以根据自己的需要来定义。不过如果普通的方法没有返回值的话，那么一定要在方法定义的时候采用void关键字来进行说明。

&emsp;&emsp;其次构造方法的名字有严格的要求，即必须与类的名字相同。也就是说，Java编译器发现有个方法与类的名字相同才把其当作构造方法来对待。

&emsp;&emsp;而对于普通方法的话，则要求不能够与类的名字相同，而且多个成员方法不能够采用相同的名字。在一个类中可以存在多个构造方法，这些构造方法都采用相同的名字，只是形式参数不同。Java语言就凭形式参数不同来判断调用那个构造方法。


### 返回对象的值

&emsp;&emsp;this关键字除了可以引用变量或者成员方法之外，还有一个重大的作用就是返回类的引用。

&emsp;&emsp;如在代码中，`可以使用return this，来返回某个类的引用。``此时这个this关键字就代表类的名称。`如代码在上面student类中，那么代码代表的含义就是return student。可见，这个this关键字除了可以引用变量或者成员方法之外，还可以作为类的返回值，这才是this关键字最引人注意的地方。

````
public Class Student {

 String name; //定义一个成员变量name

 private void SetName(String name) { //定义一个参数(局部变量)name

 this.name=name; //将局部变量的值传递给成员变量

 

 }

Return this

}
````


## synchronized

&emsp;&emsp;因为synchronized关键字涉及到锁的概念，所以先来了解一些相关的锁知识。

&emsp;&emsp;java的内置锁：每个java对象都可以用做一个实现同步的锁，这些锁成为内置锁。线程进入同步代码块或方法的时候会自动获得该锁，在退出同步代码块或方法时会释放该锁。获得内置锁的唯一途径就是进入这个锁的保护的同步代码块或方法。

&emsp;&emsp;java内置锁是一个互斥锁，这就是意味着最多只有一个线程能够获得该锁，当线程A尝试去获得线程B持有的内置锁时，线程A必须等待或者阻塞，知道线程B释放这个锁，如果B线程不释放这个锁，那么A线程将永远等待下去。

&emsp;&emsp;java的对象锁和类锁：java的对象锁和类锁在锁的概念上基本上和内置锁是一致的，但是，两个锁实际是有很大的区别的，对象锁是用于对象实例方法，或者一个对象实例上的，类锁是用于类的静态方法或者一个类的class对象上的。我们知道，类的对象实例可以有很多个，但是每个类只有一个class对象，所以不同对象实例的对象锁是互不干扰的，但是每个类只有一个类锁。但是有一点必须注意的是，其实类锁只是一个概念上的东西，并不是真实存在的，它只是用来帮助我们理解锁定实例方法和静态方法的区别的

&emsp;&emsp;上面已经对锁的一些概念有了一点了解，下面探讨synchronized关键字的用法。

&emsp;&emsp;synchronized的用法：synchronized修饰方法和synchronized修饰代码块。

&emsp;&emsp;下面分别分析这两种用法在对象锁和类锁上的效果。

&emsp;&emsp;对象锁的synchronized修饰方法和代码块：

````
public class TestSynchronized 
{  
    public void test1() 
    {  
         synchronized(this) 
         {  
              int i = 5;  
              while( i-- > 0) 
              {  
                   System.out.println(Thread.currentThread().getName() + " : " + i);  
                   try 
                   {  
                        Thread.sleep(500);  
                   } 
                   catch (InterruptedException ie) 
                   {  
                   }  
              }  
         }  
    }  
    public synchronized void test2() 
    {  
         int i = 5;  
         while( i-- > 0) 
         {  
              System.out.println(Thread.currentThread().getName() + " : " + i);  
              try 
              {  
                   Thread.sleep(500);  
              } 
              catch (InterruptedException ie) 
              {  
              }  
         }  
    }  
    public static void main(String[] args) 
    {  
         final TestSynchronized myt2 = new TestSynchronized();  
         Thread test1 = new Thread(  new Runnable() {  public void run() {  myt2.test1();  }  }, "test1"  );  
         Thread test2 = new Thread(  new Runnable() {  public void run() { myt2.test2();   }  }, "test2"  );  
         test1.start();;  
         test2.start();  
//         TestRunnable tr=new TestRunnable();
//         Thread test3=new Thread(tr);
//         test3.start();
    } 
}
 

 

test2 : 4
test2 : 3
test2 : 2
test2 : 1
test2 : 0
test1 : 4
test1 : 3
test1 : 2
test1 : 1
test1 : 0

````

&emsp;&emsp;上述的代码，第一个方法时用了同步代码块的方式进行同步，传入的对象实例是this，表明是当前对象，当然，如果需要同步其他对象实例，也不可传入其他对象的实例；第二个方法是修饰方法的方式进行同步。因为第一个同步代码块传入的this，所以两个同步代码所需要获得的对象锁都是同一个对象锁，下面main方法时分别开启两个线程，分别调用test1和test2方法，那么两个线程都需要获得该对象锁，另一个线程必须等待。上面也给出了运行的结果可以看到：直到test2线程执行完毕，释放掉锁，test1线程才开始执行。（可能这个结果有人会有疑问，代码里面明明是先开启test1线程，为什么先执行的是test2呢？这是因为java编译器在编译成字节码的时候，会对代码进行一个重排序，也就是说，编译器会根据实际情况对代码进行一个合理的排序，编译前代码写在前面，在编译后的字节码不一定排在前面，所以这种运行结果是正常的， 这里是题外话，最主要是检验synchronized的用法的正确性）

&emsp;&emsp;如果我们把test2方法的synchronized关键字去掉，执行结果会如何呢？

````
test1 : 4
test2 : 4
test2 : 3
test1 : 3
test1 : 2
test2 : 2
test2 : 1
test1 : 1
test2 : 0
test1 : 0
````

&emsp;&emsp;上面是执行结果，我们可以看到，结果输出是交替着进行输出的，这是因为，某个线程得到了对象锁，但是另一个线程还是可以访问没有进行同步的方法或者代码。进行了同步的方法（加锁方法）和没有进行同步的方法（普通方法）是互不影响的，一个线程进入了同步方法，得到了对象锁，其他线程还是可以访问那些没有同步的方法（普通方法）。这里涉及到内置锁的一个概念（此概念出自java并发编程实战第二章）：对象的内置锁和对象的状态之间是没有内在的关联的，虽然大多数类都将内置锁用做一种有效的加锁机制，但对象的域并不一定通过内置锁来保护。当获取到与对象关联的内置锁时，并不能阻止其他线程访问该对象，当某个线程获得对象的锁之后，只能阻止其他线程获得同一个锁。之所以每个对象都有一个内置锁，是为了免去显式地创建锁对象。

&emsp;&emsp;所以synchronized只是一个内置锁的加锁机制，当某个方法加上synchronized关键字后，就表明要获得该内置锁才能执行，并不能阻止其他线程访问不需要获得该内置锁的方法。
