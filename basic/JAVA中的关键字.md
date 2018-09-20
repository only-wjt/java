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


## abstract

&emsp;&emsp;使用了abstract关键字的类就是抽象类，使用了abstract关键字的方法就是抽象方法。按照常理来说，抽象类会包含一个以上的抽象方法。言下之意，你非要让一个抽象类不包含抽象方法，也是可以的。但是只要包含了一个抽象方法，这个类就必须是抽象类。抽象类（抽象方法）通常都是继承时会用的到，是java多态性的体现。

&emsp;&emsp;&emsp;&emsp;创建抽象类和方法有时对我们非常有用，因为它们使一个类的抽象变成明显的事实，可明确告诉用户和编译器自己打算如何用它。

&emsp;&emsp;abstract用法其实比较简单：