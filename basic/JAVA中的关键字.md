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

　　`方便在没有创建对象的情况下来进行调用（方法/变量）`

　　很显然，被static关键字修饰的方法或者变量不需要依赖于对象来进行访问，只要类被加载了，就可以通过类名去进行访问。

　　static可以用来修饰类的成员方法、类的成员变量，另外可以编写static代码块来优化程序性能。