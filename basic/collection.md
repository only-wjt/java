---
title: collection
tags: collection,java,basic
grammar_cjkRuby: true
---
转载至[简书作者Ruheng](https://www.jianshu.com/p/589d58033841)
### 简介
&emsp;&emsp;本篇文章主要对java集合的框架进行介绍，使大家对java集合的整体框架有个了解。具体介绍了Collection接口，Map接口以及Collection接口的三个子接口Set，List，Queue。
### Java集合类简介：
&emsp;&emsp;java集合主要有Set、List、Queue、和Map四种体系

&emsp;&emsp;Set代表无序、不可重复的集合；

&emsp;&emsp;List代表有序、重复的集合；

&emsp;&emsp;Map则代表具有映射关系的集合，

&emsp;&emsp;Java 5 又增加了Queue体系集合，代表一种队列集合实现。

&emsp;&emsp;Java集合就像一种容器，可以把多个对象（实际上是对象的引用，但习惯上都称对象）“丢进”该容器中。从Java 5 增加了泛型以后，Java集合可以记住容器中对象的数据类型，使得编码更加简洁、健壮。
### Java集合和数组的区别：
&emsp;&emsp;1.数组长度在初始化时指定，意味着`只能保存定长的数据`。而集合可以保存数量不确定的数据。同时可以保存具有映射关系的数据（即关联数组，键值对 key-value）。

&emsp;&emsp;2.数组元素即可以是基本类型的值，也可以是对象。`集合里只能保存对象` （实际上只是保存对象的引用变量），基本数据类型的变量要转换成对应的包装类才能放入集合类中。
### Java集合类之间的继承关系:
&emsp;&emsp;Java的集合类主要由两个接口派生而出：Collection和Map,Collection和Map是Java集合框架的根接口。`Map实现类用于保存具有映射关系的数据`。Map保存的每项数据都是key-value对，也就是由key和value两个值组成。`Map里的key是不可重复的`，key用户标识集合里的每项数据。如下图：

![collection大家庭](./images/collection_assortment.png)

![map大家庭](./images/map_sort.png)

&emsp;&emsp;以上我们经常用到的集合类型有ArrayList,HashSet,LinkedList,TreeSet,HashMap,TreeMap

### Collection接口中的方法：

![collection_method](./images/collection_method.png)

&emsp;&emsp;常用的方法有iterator(),添加元素，删除元素，返回Collection集合的个数以及清空集合等

### iterator()遍历集合

&emsp;&emsp;Iterator接口经常被称作迭代器，它是Collection接口的父接口。但Iterator主要用于遍历集合中的元素。其中有两个方法hasNext()以及next(),hasNext()代表如果下一个还有元素，则返回true，next()返回迭代的下一个元素。
````
public class IteratorExample {
    public static void main(String[] args){
        //创建集合，添加元素  
        Collection<Day> days = new ArrayList<Day>();
        for(int i =0;i<10;i++){
            Day day = new Day(i,i*60,i*3600);
            days.add(day);
        }
        //获取days集合的迭代器
        Iterator<Day> iterator = days.iterator();
        while(iterator.hasNext()){//判断是否有下一个元素
            Day next = iterator.next();//取出该元素
            //逐个遍历，取得元素后进行后续操作
            .....
        }
    }
}
````
&emsp;&emsp;注意：当使用Iterator对集合元素进行迭代时，`Iterator并不是把集合元素本身传给了迭代变量，而是把集合元素的值传给了迭代变量`（`就如同参数传递是值传递，基本数据类型传递的是值，引用类型传递的仅仅是对象的引用变量`），所以修改迭代变量的值对集合元素本身没有任何影响。 
&emsp;&emsp;下面代码正好显示了这一点：
````
public class IteratorExample {
    public static void main(String[] args){
        List<String> list =Arrays.asList("java语言","C语言","C++语言");
        Iterator<String> iterator = list.iterator();
        while(iterator.hasNext()){
            String next = iterator.next();//集合元素的值传给了迭代变量，仅仅传递了对象引用。保存的仅仅是指向对象内存空间的地址
            next ="修改后的";
            System.out.println(next);
            
        }
        System.out.println(list);
    }

}

````
### 集合Set：
### 简介
&emsp;&emsp;Set集合与Collection集合基本相同，没有提供任何额外的方法。实际上Set就是Collection，只是行为略有不同（``Set不允许包含重复元素``）。Set集合不允许包含相同的元素，如果试图把两个相同的元素加入同一个Set集合中，则添加操作失败，add()方法返回false，且新元素不会被加入。

#### 一、hashSet

#### HashSet简介
&emsp;&emsp;HashSet是Set接口的典型实现，实现了Set接口中的所有方法，并没有添加额外的方法，``大多数时候使用Set集合时就是使用这个实现类``。HashSet按Hash算法来存储集合中的元素。因此``具有很好的存取和查找性能``。

#### HashSet特点

1.`不能保证元素的排列顺序`，顺序可能与添加顺序不同，顺序也有可能发生变化。
2.HashSet不是同步的，如果多个线程同时访问一个HashSet，则必须通过代码来保证其同步。其实查看源代码发现HashSet内部维护数据的采用的是HashMap，根本原因是HashMap不是线程安全的类。导致了HashSet的非线程安全。
3.集合元素值可以是null。
除此之外，HashSet判断两个元素是否相等的标准也是其一大特点。HashSet集合判断两个元素相等的标准是两个对象通过equals()方法比较相等，并且两个对象的hashCode()方法返回值也相等。

写到这里，我们就要介绍下equals()和hashCode()方法了。
