---
title: reflect
tags: java,reflect,base
grammar_cjkRuby: true
---

### 反射的性能
&emsp;&emsp;反射是比较浪费性能的。 目前最常见的优化反射性能的方法就是采用委托：用委托的方式调用需要反射调用的方法（或者属性、字段）。

那么如何得到委托呢？ 目前最常见也就是二种方法：Emit, ExpressionTree 。其中ExpressionTree可认为是Emit方法的简化版本， 所以Emit是最根本的方法，它采用在运行时动态构造一段IL代码来包装需要反射调用的代码， 这段动态生成的代码满足某个委托的签名，因此最后可以采用委托的方式代替反射调用。

### (1)反射的概念
&emsp;&emsp;反射是指计算机程序在运行时（Run time）可以访问、检测和修改它本身状态或行为的一种能力。用比喻来说，反射就是程序在运行的时候能够“观察”并且修改自己的行为。反射是Java编程语言的一个特性，它允许运行时的Java程序检测自身状态，并且修改程序的内部属性。利用Elipse开发时，经常可以用“ctr+/”出来属性等，就是利用了JAVA的反射特性来获取的。
````
import java.lang.reflect.Method;

public class DumpMethods {

    public static void main(String args[]) {
        try {
            Class c = Class.forName("java.lang.String");//反射的其中一种方式，利用这种方式这个类
														//返回：具有指定名的类的 Class 对象。
														//通俗的说就是：获得字符串参数中指定的类，并初始化该类类装载			
            Method m[] = c.getDeclaredMethods();//通过getDeclaredMethods()方法来获取java.lang.String里面的方法名和方法返回值
            for (int i = 0; i < m.length; i++) {
                System.out.println(m[i].toString());//打印java.lang.String中的第一个方法
            }
        } catch (Throwable e) {
            System.err.println(e);
        }
    }
}
````
&emsp;&emsp;在java中Class.forName()和ClassLoader都可以对类进行加载。ClassLoader就是遵循 双亲委派模型 最终调用启动类加载器的类加载器，实现的功能是“通过一个类的全限定名来获取描述此类的二进制字节流”，获取到二进制流后放到JVM中。Class.forName()方法实际上也是调用的ClassLoader来实现的。
&emsp;&emsp;底层最后调用的方法是==forName() #803100==这个方法，在这个forName()方法中的第二个参数被默认设置为了true，true代表是否对加载的类进行初始化，对类进行
初始化，代表会执行类中的静态代码块，以及对静态变量的赋值等操作


&emsp;&emsp;如果使用，GetMethods()方法获取的不仅仅是本类的方法，还有继承类的方法
&emsp;&emsp;以上只是反射获取对象的一种方式,还有另外一种
````
ClassLoader cl = new  ClassLoader(); 
Class cl.loadClass( String name, boolean resolve )
````
&emsp;&emsp;还有一种是通过此类类名.class的方式    &emsp;&emsp;ex:int.class 或者 Integer.Type


### (2)模拟instanceof操作符
````
package com.wjt.reflect;

import java.lang.reflect.Method;

class A {
	public void add(){
		System.out.println("Hello World");
	}
}

public class Reflect02 {
    public static void main(String args[]) {
        try {
        	A a = new A();
        	Class<? extends A> class1 = a.getClass();
        	Method[] declaredMethods = class1.getDeclaredMethods();
        	System.out.println(declaredMethods[0]);
        	boolean b1 = class1.isInstance(new Integer(37));//相当于instanceof操作符，可以判断Integer(37)是不是cls的实例
        	System.out.println(b1);
        	boolean b2 = class1.isInstance(new A());
        	System.out.println(b2);
        } catch (Throwable e) {
            System.err.println(e);
        }
    }
}
````
&emsp;&emsp;此代码可以看出new Integer(37)并不是class1的实例，new A()才是。
### (3)获取类的构造方法
````
package com.wjt.reflect;

import java.lang.reflect.Constructor;

public class Reflect03 {

    public Reflect03() {
    }

    protected Reflect03(int i, double d) {
    }

    public static void main(String args[]) {
        try {
			
        	Reflect03 reflect03 = new Reflect03();
        	Class<? extends Reflect03> Refmark textlecct03 = reflect03.getClass();
            Constructor[] consList = Reflecct03.getDeclaredConstructors();
			
            for (int i = 0; i < consList.length; i++) {
                Constructor ct = consList[i];
                System.out.println("name = " + ct.getName());
                System.out.println("decl class = " + ct.getDeclaringClass());

                Class pvec[] = ct.getParameterTypes();//获取构造方法里面的参数类型
                for (int j = 0; j < pvec.length; j++) {
                    System.out.println("param #" + j + " " + pvec[j]);
                }

                Class evec[] = ct.getExceptionTypes();//获取构造方法的返回类型，但是构造方法无返回类型，所以此处没有得到值
                for (int j = 0; j < evec.length; j++) {
                    System.out.println("exc #" + j + " " + evec[j]);
                }
                System.out.println("-----");
            }
        } catch (Throwable e) {
            System.err.println(e);
        }
    }
}
````
&emsp;&emsp;一个类中有可能不止一个构造方法，如果没有自己自定义构造方法，则会自动生成一个无参的构造函数
### (4)获取类的属性
````
package com.wjt.reflect;

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

public class Reflect04 {
    private double d;
    public static final int i = 37;
    String s = "testing";

    public static void main(String args[]) {
        try {
            Reflect04 reflect04 = new Reflect04();
            Class<? extends Reflect04> cls = reflect04.getClass();

            Field fieldlist[] = cls.getDeclaredFields();//获取Reflect04属性并存入数组
            for (int i = 0; i < fieldlist.length; i++) {
                Field fld = fieldlist[i];
                System.out.println("name = " + fld.getName());//获取成员属性名
                System.out.println("decl class = " + fld.getDeclaringClass());//获取类名路径
                System.out.println("type = " + fld.getType());//获取成员属性类型

                int mod = fld.getModifiers();//获取属性修饰符的类
                System.out.println("modifiers = " + Modifier.toString(mod));
                System.out.println("-----");
            }
        } catch (Throwable e) {
            System.err.println(e);
        }
    }
}
````
&emsp;&emsp;我们可以使用getDeclaredFields获取类中定义的属性，也可以==使用getFields额外获取父类中定义的属性 #801600==。需要注意的==后者返回的是包含父类属性中修饰符为public的属性。 #800600==

### (5)通过方法名调用方法
````
package com.wjt.reflect;

import java.lang.reflect.*;

public class Reflect05 {

    public int add(int a, int b) {
        return a + b;
    }

    public static void main(String args[]) {
        try {
        	
        	Reflect05 reflect05 = new Reflect05();
        	Class<? extends Reflect05> cls = reflect05.getClass();
            Class partypes[] = new Class[2];//创建一个类对象数组
            partypes[0] = Integer.TYPE;//获取参数1
            partypes[1] = Integer.TYPE;//获取参数2
            Method meth = cls.getMethod("add", partypes);//获取类里面的方法
            Reflect05 methobj = reflect05;
            Object arglist[] = new Object[2];
            arglist[0] = new Integer(37);//为add方法传值
            arglist[1] = new Integer(47);
            Object retobj = meth.invoke(methobj, arglist);//把值保存到方法中
            Integer retval = (Integer) retobj;
            System.out.println(retval.intValue());
        } catch (Throwable e) {
            System.err.println(e);
        }
    }
}
````
&emsp;&emsp;一个程序想要去调用add方法，但是直到程序执行的时候才知道这个方法，所以在程序执行的时候，方法的名字才被指定。
&emsp;&emsp;getMethod用来在Class中寻找有两个Integer类型参数和方法名为add的方法。一旦这个方法被找到，并且赋值给Method对象后，就可以在合适的参数下调用。

### (6)创建新的对象

如果调用构造方法其实就是创造了一个对象，准确来说，创建一个新的对象包含了==内存分配和对象构建 #800c00==。
````
package com.wjt.reflect;

import java.lang.reflect.Constructor;

public class Reflect06 {
    public Reflect06() {
    }

    public Reflect06(int a, int b) {
        System.out.println("a = " + a + " b = " + b);
    }

    public static void main(String args[]) {
        try {
        	Reflect06 reflect06 = new Reflect06();
        	Class<? extends Reflect06> cls = reflect06.getClass();//获取这个对象
            Class partypes[] = new Class[2];//创建一个Class类型的数组存放参数类型
            partypes[0] = Integer.TYPE;
            partypes[1] = Integer.TYPE;//Integer中的一个字段,封装到partypes数组中去
            Constructor ct= cls.getConstructor(partypes);//获得具有两个int参数类型的构造方法
            Object arglist[] = new Object[2];
            arglist[0] = new Integer(37);//传值
            arglist[1] = new Integer(47);
            Object retobj = ct.newInstance(arglist);.//通过构造方法创建出来一个新的实例，所以会调用构造方法执行
        } catch (Throwable e) {
            System.err.println(e);
        }
    }
}
````
&emsp;&emsp;上面程序找到了一个有特定参数类型的构造方法，执行调用后，创建了对象的一个新的实例。这种方式创建对象的价值在于它
是完全动态的，==是在程序执行时寻找和执行构造方法，而不是在编译时 #f45736==。

### (7)修改属性值
````
package com.wjt.reflect;

import java.lang.reflect.Field;

public class Reflect07 {
    public double d;

    public static void main(String args[]) {
        try {
        	Reflect07 reflect07  = new Reflect07();
        	Class<? extends Reflect07> cls = reflect07.getClass();
        	Field fld = cls.getField("d");
        	System.out.println("before="+reflect07.d);
        	fld.setDouble(reflect07, 2.34);
        	System.out.println("after="+reflect07.d);
        } catch (Throwable e) {
            System.err.println(e);
        }
    }
}
````
&emsp;&emsp;最后的值被修改为2.34


### (8)使用数组
````
package com.wjt.reflect;

import java.lang.reflect.Array;

public class Reflect08 {
    public static void main(String args[]) {
        try {
        	Class cls = Class.forName("java.lang.String");
            Object arr = Array.newInstance(cls, 10);//创建一个String类型10个长度的数组
            Array.set(arr, 5, "this is a test");//向数组中第五个位置存入“this is a test”
            String s = (String) Array.get(arr, 5);//去除下标为5的元素
            System.out.println(s);
        } catch (Throwable e) {
            System.err.println(e);
        }
    }
}
````
上面程序创建了长度为10的String类型的数组，然后将数组中索引为5的值赋值为一个字符串。
````
package com.wjt.reflect;

import java.lang.reflect.Array;

public class Reflect0801 {
    public static void main(String args[]) {
        int dims[] = new int[]{5, 10, 15};
        Object arr = Array.newInstance(Integer.TYPE, dims);
        Object arrobj = Array.get(arr, 3);
        System.out.println(arrobj);
        Class cls = arrobj.getClass().getComponentType();
        System.out.println(cls);
        arrobj = Array.get(arrobj, 5);
        Array.setInt(arrobj, 10, 37);
        int arrcast[][][] = (int[][][]) arr;
        System.out.println(arrcast[3][5][10]);
    }
}
````
&emsp;&emsp;上面的程序创建了一个5x10x15的三维int类型数组。然后将位置[3][5][10]设置为37，一个多维数组实际上就是数组的数组，所以第一次Array.get后得到的是一个10x15的二维数组。


### (9)访问私有成员变量和方法

&emsp;&emsp;在访问private或protected成员变量和方法时，须通过setAccessible(boolean assess)方法取消Java语言检查，否则将抛出IllegalAccessException。如果JVM的安全管理器设置了相应的安全机制，那么调用该方法将抛出SecurityException


### (10)反射的实际应用
&emsp;&emsp;1.反射最重要的用途就是开发各种通用框架，比如在spring中，我们将所有的类Bean交给spring容器管理，无论是XML配置Bean还是注解配置，当我们从容器中获取Bean来依赖入时，容器会读取配置，而配置中给的就是类的信息，spring根据这些信息，需要创建那些Bean，spring就动态的创建这些类。还有	在struts2的struts.xml中配置action，也是通过反射调用的action。

&emsp;&emsp;2.还有在我们创建数据库链接时，这句代码Class tc = Class.forName("com.java.dbtest.TestConnection")就是告诉JVM去加载这个类，而加载的过程是在程序执行过程中动态加的。通过类的全类名让jvm在服务器中找到并加载这个类，而如果是使用别的数据库，那就要换一个类了，如果是传统写死的方法创建，就要修改原来类的代码，而对于反射，则只是传入的参数就变成另一个了而已，可以通过修改配置文件，而不是直接修改代码。

&emsp;&emsp;3.再比如我们有两个程序员，一个程序员在写程序的时候，需要使用第二个程序员所写的类，但第二个程序员并没完成他所写的类。那么第一个程序员的代码能否通过编译呢？这是不能通过编译的。利用Java反射的机制，就可以让第一个程序员在没有得到第二个程序员所写的类的时候，来完成自身代码的编译。只是如果这个类还没有，获取时会获取不到，但不会导致编译错误，更不会导致程序的崩溃。

&emsp;&emsp;还有当我们在使用 IDE（如 Eclipse\IDEA）时，当我们输入一个队长或者类并向调用它的属性和方法时，一按 (“.”)点号，编译器就会自动列出她的属性或方法，这里就会用到反射。
