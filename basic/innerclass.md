---
title: inner class
tags: basic
grammar_cjkRuby: true
---


&emsp;&emsp;Java里面static一般用来修饰成员变量或函数。但有一种特殊用法是用static修饰内部类，普通类是不允许声明为静态的，只有内部类才可以。被static修饰的内部类可以直接作为一个普通类来使用，而不需实例一个外部类（见如下代码）

 


Java代码  
public class OuterClass {  
    public static class InnerClass{  
        InnerClass(){  
            System.out.println("============= 我是一个内部类'InnerClass' =============");  
        }  
    }  
}  
 

Java代码  收藏代码
public class TestStaticClass {  
    public static void main(String[] args) {  
        // 不需要new一个OutClass  
        new OuterClass.InnerClass();  
    }  
}  
 

 

如果没有用static修饰InterClass，则只能按如下方式调用：

 

 

 

Java代码  收藏代码
package inner_class;  
  
public class OuterClass {  
    public class InnerClass{  
        InnerClass(){  
            System.out.println("============= 我是一个内部类'InnerClass' =============");  
        }  
    }  
}  
 

 

Java代码  收藏代码
public class TestStaticClass {  
    public static void main(String[] args) {  
        // OutClass需要先生成一个实例  
        OuterClass oc = new OuterClass();  
        oc.new InnerClass();  
    }  
}  