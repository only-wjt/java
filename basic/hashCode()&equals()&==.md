---
title: hashCode()&equals()&==
tags: basic
grammar_cjkRuby: true
---

## 简介
“==”对比对象实例的内存地址（也即对象实例的ID），来判断是否是同一对象实例；又可以说是判断对象实例是否物理相等；

equals：查看底层object的equals方法 

````
	public boolean equals(Object obj) { 
		return (this == obj);
	}
````

&emsp;&emsp;当对象所属的类没有重写根类Object的equals()方法时`调用==判断`即物理相等。当对象所属的类重写equals()方法（可能因为需要自己特有的“逻辑相等”概念).equals()判断的根据就因具体实现而异：
&emsp;&emsp;如String类重写equals()来判断字符串的值是否相等。
````
 public boolean equals(Object anObject) {
        if (this == anObject) {//先判断是不是物理相等此时相当于==
            return true;
        }
        if (anObject instanceof String) {//如果物理不相等，则继续判断String对象中的每一个字符是否相等
            String anotherString = (String) anObject;
            int n = value.length;
            if (n == anotherString.value.length) {//先判断两个String对象长度是否相等，相等继续判断，不相等，返回false
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {//循环遍历，比较两个String对象同一位置上的字符是否相等
                    if (v1[i] != v2[i])
                            return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
````
&emsp;&emsp;hashCode():java.lang.Object类的源码中方法是native的

````
public native int hashCode();
````

&emsp;&emsp;方法的计算依赖于对象实例的ID（内存地址），每个Object对象的hashCode都是唯一的，通过将该对象的内部地址转换成一个整数来实现的 ，toString方法也会用到这个hashCode。

````
public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }
````

&emsp;&emsp;如果重写了hashCode();

````
public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;
 
            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }
````

&emsp;&emsp;String底层是一个final的char数组

````
private final char value[];
````

&emsp;&emsp;String的hashCode计算方式就是遍历字符数组 计算其加权和


Integer类hashCode()返回的是它值本身
````
public int hashCode() {
        return value;
    }
````

**两个对象的equals()方法等同->两对象hashCode()必相同,hashCode()相同不能推导 equals()相等**
**个人理解是两个对象的逻辑相等必须要通过equals做比较**

&emsp;&emsp;hashCode是对象的散列值 ,不同的对象是可能会有相同的散列值的，所以两个对象的hashCode()方法相等，不代表 equals相等
&emsp;&emsp;`hashCode()就是一个提高效率的策略问题` ，因为hashCode是int型的，int型比较运算是相当快的。所以`比较两个对象是否相等，可以先比较其hashCode` 如果hashCode不相等说明肯定是两个不同的对象。但hashCode相等的情况下，并不一定equals也相等，就再执行equals方法真正来比较两者是否相等。
&emsp;&emsp;也就是说hashCode()方法效率比较高，一般做两个对象的比较时，先判断hashCode()是否相等。如果不相等，则两个对象必然不相等，如果hashCode()相等，再比较equals()方法，如果相等，则两个对象相等。


### 注意：

&emsp;&emsp;重写了equals方法则一定要重写hashCode，就像String类一样，equals方法不同于Object的equals，它的hashCode方法也不一样。
&emsp;&emsp;为什么：反证一下，如果不重写hashCode，那么两个字符串，不同的对象，但里面的内容是一样的，equals则返回true。但他们的hashCode却返回实例的ID（内存地址）。那么这两个对象的hashCode就不相等了！违反了上面所说的：equals方法是鉴定两个对象逻辑相等的唯一标准 。两个对象的equals()方法等同===>两对象hashCode()必相同。 hashCode()相同 不能推导 equals()相等。因此，重写了equals方法则一定要重写hashCode来保证上面的语句成立

## hashMap中的hashCode()和equals()

&emsp;&emsp;HashMap中维护了一个特别的数据结构，就是Entry

````
static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        Entry<K,V> next;
        int hash;
````
### 重写hashCode()和equal()
&emps;&emps;Object版本的equal只是简单地判断是不是同一个实例。但是有的时候，我们想要的的是逻辑上的相等。比如有一个学生类student，有一个属性studentID，只要studentID相等，不是同一个实例我们也认为是同一学生。当我们认为`判定equals的相等应该是逻辑上的相等而不是只是判断是不是内存中的同一个东西的时候`，就需要重写equals()。而涉及到HashMap的时候，重写了equals()，就需要重写hashCode()

基本原则 
1. 同一个对象（没有发生过修改）无论何时调用hashCode()得到的返回值必须一样。 
如果一个key对象在put的时候调用hashCode()决定了存放的位置，而在get的时候调用hashCode()得到了不一样的返回值，这个值映射到了一个和原来不一样的地方，那么肯定就找不到原来那个键值对了。 
2. hashCode()的返回值相等的对象不一定相等，通过hashCode()和equals()必须能唯一确定一个对象 
不相等的对象的hashCode()的结果可以相等。hashCode()在注意关注碰撞问题的时候，也要关注生成速度问题，完美hash不现实 
3. 一旦重写了equals()函数（重写equals的时候还要注意要满足自反性、对称性、传递性、一致性），就必须重写hashCode()函数。而且hashCode()的生成哈希值的依据应该是equals()中用来比较是否相等的字段 
如果两个由equals()规定相等的对象生成的hashCode不等，对于hashMap来说，他们很可能分别映射到不同位置，没有调用equals()比较是否相等的机会，两个实际上相等的对象可能被插入不同位置，出现错误。其他一些基于哈希方法的集合类可能也会有这个问题 
1. 给int变量result赋予某个非零整数值，例如17 
2. 位对象内每个有意义的域f(即每个可以做equals()操作的域)计算出一个int散列值c：