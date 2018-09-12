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
