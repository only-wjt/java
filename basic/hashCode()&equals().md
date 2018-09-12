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
