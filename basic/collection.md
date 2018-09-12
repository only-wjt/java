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
2.HashSet不是同步的，如果多个线程同时访问一个HashSet，则必须通过代码来保证其同步。其实查看源代码发现`HashSet内部维护数据的采用的是HashMap`，根本原因是HashMap不是线程安全的类。导致了`HashSet的非线程安全`。
3.集合元素值可以是null。
除此之外，HashSet判断两个元素是否相等的标准也是其一大特点。HashSet集合判断两个元素相等的标准是两个对象通过equals()方法比较相等，并且两个对象的hashCode()方法返回值也相等。

写到这里，我们就要介绍下equals()和hashCode()方法了。

#### equals()和hashCode()
#### equals()
equals() 的作用是``用来判断两个对象是否相等。``

equals() 定义在JDK的Object.java中。`通过判断两个对象的地址是否相等`(即，是否是同一个对象)来区分它们是否相等。源码如下：

````
public boolean equals(Object obj) {
       return (this == obj);
   }
````
&emsp;&emsp;既然Object.java中定义了equals()方法，这就意味着所有的Java类都实现了equals()方法，所有的类都可以通过equals()去比较两个对象是否相等。 但是，使用默认的“equals()”方法，等价于“==”方法。我们也可以在Object的子类中重写此方法，自定义“equals()”方法，在其中定义自己的判断逻辑，如果满足则返回true，不满足则返回false。

下面我们自定义一个类 Person,并认为年龄，身高相等的两个Person对象，equals()方法比较结果相等。

````
public class Person {
    public int age;
    public int height;
    @Override
    public boolean equals(Object obj) {
        if (this == obj)
            return true;
        if (obj == null)
            return false;
        if (getClass() != obj.getClass())
            return false;
        Person other = (Person) obj;
        if (age != other.age)
            return false;
        if (height != other.height)
            return false;
        return true;
    }
}
````
````
public class EqualTest {
  public static void main(String[] args){
      Person p1 = new Person();
      Person p2 =new Person();
      System.out.println(p1.equals(p2));
  }

}
````
下面根据“类是否覆盖equals()方法”，将它分为2类。
&emps;&emps;(01) 若某个类没有覆盖equals()方法，当它的通过equals()比较两个对象时，实际上是比较两个对象是不是同一个对象。这时，等价于通过“==”去比较这两个对象，即两个对象的内存地址是否相同。
&emps;&emps;(02) 我们可以覆盖类的equals()方法，来让equals()通过其它方式比较两个对象是否相等。通常的做法是：若两个对象的内容相等，则equals()方法返回true；否则，返回fasle。

#### hashCode()
hashCode() 的作用是`获取哈希码`，也称为散列码；它`实际上是返回一个int整数`。这个哈希码的作用是确定该对象在哈希表中的索引位置。

&emps;&emps;hashCode() 定义在JDK的Object.java中，这就意味着Java中的任何类都包含有hashCode() 函数。虽然，每个Java类都包含hashCode() 函数。但是，仅仅当创建某个“类"的散列表时，该类的hashCode() 才有用。更通俗地说就是`创建包含该类的HashMap，Hashtable，HashSet集合时，hashCode() 才有用`。因为HashMap，Hashtable，HashSet就是散列表集合。
&emps;&emps;在散列表中，hashCode()作用是：确定该类的每一个对象在散列表中的位置；其它情况下类的hashCode() 没有作用。在散列表中hashCode() 的作用`是获取对象的散列码，进而确定该对象在散列表中的位置`。

hashCode()也分两种情况。一种是Object类中默认的方法，另一种是在子类中重写的方法。
&emps;&emps;(01) 若某个类没有覆盖hashCode()方法，当它的通过hashCode()比较两个对象时，实际上是比较两个对象是不是同一个对象。这时，等价于通过“==”去比较这两个对象，即两个对象的内存地址是否相同。
&emps;&emps;(02) 我们可以覆盖类的hashCode()方法，来让hashCode()通过其它方式比较两个对象是否相等。通常的做法是：若两个对象的内容相等，则hashCode()方法返回true；否则，返回fasle。

通过对以上两个方法的了解。我们可以接下来学习HashSet集合中如何判断两个元素是否相等？

#### HashSet中判断集合元素相等
两个对象比较 具体分为如下四个情况：
1.如果有两个元素通过equal()方法比较返回false，但它们的hashCode()方法返回不相等，HashSet将会把它们存储在不同的位置。

2.如果有两个元素通过equal()方法比较返回true，但它们的hashCode()方法返回不相等，HashSet将会把它们存储在不同的位置。

3.如果两个对象通过equals()方法比较不相等，hashCode()方法比较相等，HashSet将会把它们存储在相同的位置，在这个位置以链表式结构来保存多个对象。这是因为当向HashSet集合中存入一个元素时，HashSet会调用对象的hashCode()方法来得到对象的hashCode值，然后根据该hashCode值来决定该对象存储在HashSet中存储位置。

4.如果有两个元素通过equal()方法比较返回true，但它们的hashCode()方法返回true，HashSet将不予添加。

HashSet判断两个元素相等的标准：两个对象通过equals()方法比较相等，并且两个对象的hashCode()方法返回值也相等。

注意：HashSet是根据元素的hashCode值来快速定位的，如果HashSet中两个以上的元素具有相同的hashCode值，将会导致性能下降。所以如果重写类的equals()方法和hashCode()方法时，应尽量保证两个对象通过hashCode()方法返回值相等时，通过equals()方法比较返回true。





