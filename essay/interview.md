---
title: interview 
tags:interview,essay
grammar_cjkRuby: true
---


## 反向代理是什么，有什么作用
### 使用代理的作用
#### 提高访问速度 
由于目标主机返回的数据会存放在代理服务器的硬盘中，因此下一次客户再访问相同的站点数据时，会直接从代理服务器的硬盘中读取，起到了缓存的作用，尤其对于热门网站能明显提高访问速度。

#### 防火墙作用 
由于所有的客户机请求都必须通过代理服务器访问远程站点，因此可以在代理服务器上设限，过滤掉某些不安全信息。同时正向代理中上网者可以隐藏自己的IP,免受攻击。

#### 突破访问限制 
互联网上有许多开发的代理服务器，客户机在访问受限时，可通过不受限的代理服务器访问目标站点，通俗说，我们使用的翻墙浏览器就是利用了代理服务器，可以直接访问外网。

 ### 正向代理
 &emsp;&emsp;我们可以先从正向代理入手，正向代理一般用来作为对访问不到的资源的代理，我们知道从谁那得到的资源，但是资源处不知道具体是谁得到。

![Diagram](https://www.github.com/only-wjt/images/raw/master/小书匠/1547447924906.png)

&emsp;&emsp;就想A向B借钱，但是B没有钱，于是B就向C借钱，然后把借来的钱给A，A并不知道这是B从C那借来的钱。

&emsp;&emsp; 正向代理（forward proxy） ，一个位于客户端和原始服务器之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并制定目标（原始服务器），然后代理向原始服务器转发请求并将获得的内容返回给客户端，客户端才能使用正向代理。我们平时说的代理就是指正向代理。

### 反向代理

&emsp;&emsp; 反向代理（Reverse Proxy），以`代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，`并将从服务器上得到的结果返回给internet上请求的客户端，此时代理服务器对外表现为一个反向代理服务器。 
&emsp;&emsp; 理解起来有些抽象，可以这么说：A向B借钱，B没有拿自己的钱，而是悄悄地向C借钱，拿到钱之后再交给A,A以为是B的钱，他并不知道C的存在。 

![Diagram](https://www.github.com/only-wjt/images/raw/master/小书匠/1547447924926.png)

### 正向代理和反向代理的区别； 

 - 位置不同 
 正向代理，架设在客户机和目标主机之间； 
 反向代理，架设在服务器端；
 - 代理对象不同 
正向代理，代理客户端，服务端不知道实际发起请求的客户端； 
反向代理，代理服务端，客户端不知道实际提供服务的服务端； 

![Diagram](https://www.github.com/only-wjt/images/raw/master/小书匠/1547447925120.png)

&emsp;&emsp; 备注：正向代理–HTTP代理为多个人提供翻墙服务；反向代理–百度外卖为多个商户提供平台给某个用户提供外卖服务。
- 用途不同 
正向代理，为在防火墙内的局域网客户端提供访问Internet的途径； 
反向代理，将防火墙后面的服务器提供给Internet访问；
- 安全性不同 
正向代理允许客户端通过它访问任意网站并且隐藏客户端自身，因此必须采取安全措施以确保仅为授权的客户端提供服务； 
反向代理对外都是透明的，访问者并不知道自己访问的是哪一个代理。

 - 正向代理的应用
    1. 访问原来无法访问的资源 
    2. 用作缓存，加速访问速度 
    3. 对客户端访问授权，上网进行认证 
    4. 代理可以记录用户访问记录（上网行为管理），对外隐藏用户信息

 - 反向代理的应用
    1. 保护内网安全 
    2. 负载均衡 
    3. 缓存，减少服务器的压力 
       Nginx作为最近较火的反向代理服务器，安装在目的主机端，主要用于转发客户机请求，后台有多个http服务器提供服务，nginx的功能就是把请求转发给后台的服务器，决定哪台目标主机来处理当前请求。

## 总结
&emsp;&emsp;正向代理是从客户端的角度出发，服务于特定用户（比如说一个局域网内的客户）以访问非特定的服务；反向代理正好与此相反，从服务端的角度出发，服务于非特定用户（通常是所有用户），已访问特定的服务。 

## nginx的作用，以及实现负载均衡[更深层次的理解](https://blog.csdn.net/duxingxia356/article/details/49820013)

&emsp;&emsp; Nginx是一个轻量级、高性能、稳定性高、并发性好的HTTP和反向代理服务器，应用很广泛。

### 主要作用

#### 反向代理          
&emsp;&emsp; 是用来代理服务器的，代理我们要访问的目标服务器。代理服务器接受请求，然后将请求转发给内部网络的服务器(集群化)，并将从服务器上得到的结果返回给客户端，此时代理服务器对外就表现为一个服务器。

#### 负载均衡

&emsp;&emsp; 多在高并发情况下需要使用。其原理就是将数据流量分摊到多个服务器执行，减轻每台服务器的压力，多台服务器(集群)共同完成工作任务，从而提高了数据的吞吐量。
&emsp;&emsp; Nginx可使用的负载均衡策略有：轮询（默认）、权重、ip_hash、url_hash(第三方)、fair(第三方)

&emsp;&emsp; 负载均衡也是Nginx常用的一个功能，负载均衡其意思就是分摊到多个操作单元上进行执行，例如Web服务器、FTP服务器、企业关键应用服务器和其它关键任务服务器等，从而共同完成工作任务。简单而言就是当有2台或以上服务器时，根据规则随机的将请求分发到指定的服务器上处理，负载均衡配置一般都需要同时配置反向代理，通过反向代理跳转到负载均衡。而Nginx目前支持自带3种负载均衡策略，还有2种常用的第三方策略。

- RR 
按照轮询（默认）方式进行负载，每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。虽然这种方式简便、成本低廉。但缺点是：可靠性低和负载分配不均衡。

- 权重 
指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。

````
upstream test{
     server localhost:8080 weight=9;
     server localhost:8081 weight=1;
}
````

此时8080和8081分别占90%和10%。

- ip_hash 
上面的2种方式都有一个问题，那就是下一个请求来的时候请求可能分发到另外一个服务器，当我们的程序不是无状态的时候（采用了session保存数据），这时候就有一个很大的很问题了，比如把登录信息保存到了session中，那么跳转到另外一台服务器的时候就需要重新登录了，所以很多时候我们需要一个客户只访问一个服务器，那么就需要用iphash了，iphash的每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

````
upstream test {
    ip_hash;
    server localhost:8080;
    server localhost:8081;
}
````
- fair(第三方) 
按后端服务器的响应时间来分配请求，响应时间短的优先分配。

````
upstream backend { 
    fair; 
    server localhost:8080;
    server localhost:8081;
}
````

- url_hash(第三方) 
按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。 在upstream中加入hash语句，server语句中不能写入weight等其他的参数，hash_method是使用的hash算法。

````
upstream backend { 
    hash $request_uri; 
    hash_method crc32; 
    server localhost:8080;
    server localhost:8081;
} 
````

#### 动静分离
&emsp;&emsp; Nginx提供的动静分离是指把动态请求和静态请求分离开，合适的服务器处理相应的请求，使整个服务器系统的性能、效率更高。
&emsp;&emsp; Nginx可以根据配置对不同的请求做不同转发，这是动态分离的基础。静态请求对应的静态资源可以直接放在Nginx上做缓冲，更好的做法是放在相应的缓冲服务器上。动态请求由相应的后端服务器处理。

## mysql主从复制的作用和原理[详细原理](https://blog.csdn.net/darkangel1228/article/details/80003967)

### 一、什么是主从复制?
&emsp;&emsp;主从复制，是用来建立一个和主数据库完全一样的数据库环境，称为从数据库；主数据库一般是准实时的业务数据库。

### 二、主从复制的作用（好处，或者说为什么要做主从）重点!
&emsp;&emsp;1、做数据的热备，作为后备数据库，主数据库服务器故障后，可切换到从数据库继续工作，避免数据丢失。
&emsp;&emsp;2、架构的扩展。业务量越来越大，I/O访问频率过高，单机无法满足，此时做多库的存储，降低磁盘I/O访问的频率，提高单个机器的I/O性能。
&emsp;&emsp;3、读写分离，使数据库能支撑更大的并发。在报表中尤其重要。由于部分报表sql语句非常的慢，导致锁表，影响前台服务。如果前台使用master，报表使用slave，那么报表sql将不会造成前台锁，保证了前台速度。

### 三、主从复制的原理（重中之重，面试必问）：
&emsp;&emsp;1.数据库有个bin-log二进制文件，记录了所有sql语句。
&emsp;&emsp;2.我们的目标就是把主数据库的bin-log文件的sql语句复制过来。
&emsp;&emsp;3.让其在从数据的relay-log重做日志文件中再执行一次这些sql语句即可。
&emsp;&emsp;4.下面的主从配置就是围绕这个原理配置
&emsp;&emsp;5.具体需要三个线程来操作：
&emsp;&emsp;&emsp;&emsp;1.binlog输出线程:每当有从库连接到主库的时候，主库都会创建一个线程然后发送binlog内容到从库。在从库里，当复制开始的时候，从库就会创建两个线程进行处理：
&emsp;&emsp;&emsp;&emsp;2.从库I/O线程:当START SLAVE语句在从库开始执行之后，从库创建一个I/O线程，该线程连接到主库并请求主库发送binlog里面的更新记录到从库上。从库I/O线程读取主库的binlog输出线程发送的更新并拷贝这些更新到本地文件，其中包括relay log文件。
&emsp;&emsp;&emsp;&emsp;3.从库的SQL线程:从库创建一个SQL线程，这个线程读取从库I/O线程写到relay log的更新事件并执行。

![主从复制](https://www.github.com/only-wjt/images/raw/master/小书匠/1547447925972.png)

![主从复制辅助图](https://www.github.com/only-wjt/images/raw/master/小书匠/1547447926552.png)

&emsp;&emsp;可以知道，对于每一个主从复制的连接，都有三个线程。拥有多个从库的主库为每一个连接到主库的从库创建一个binlog输出线程，每一个从库都有它自己的I/O线程和SQL线程。
&emsp;&emsp;步骤一：主库db的更新事件(update、insert、delete)被写到binlog
&emsp;&emsp;步骤二：从库发起连接，连接到主库
&emsp;&emsp;步骤三：此时主库创建一个binlog dump thread线程，把binlog的内容发送到从库
&emsp;&emsp;步骤四：从库启动之后，创建一个I/O线程，读取主库传过来的binlog内容并写入到relay log
&emsp;&emsp;步骤五：还会创建一个SQL线程，从relay log里面读取内容，从Exec_Master_Log_Pos位置开始执行读取到的更新事件，将更新内容写入到slave的db.

#### 从数据库的读的延迟问题了解吗？如何解决？做主从后主服务器挂了怎么办？
- 主库宕机后，数据可能丢失
- 从库只有一个sql Thread，主库写压力大，复制很可能延时

- 半同步复制—解决数据丢失的问题
- 并行复制—-解决从库复制延迟的问题

## 线程中的死锁是怎么产生的，应该怎么避免？


&emsp;&emsp;多个线程同时被阻塞,它们中的一个或者全部都在等待某个资源被释放.由于线程被无限期地阻塞,因此程序不能正常运行.形象的说就是:一个宝藏需要两把钥匙来打开,同时间正好来了两个人,他们一人一把钥匙,但是双方都再等着对方能交出钥匙来打开宝藏,谁都没释放自己的那把钥匙.就这样这俩人一直僵持下去,直到开发人员发现这个局面.

### 死锁的产生

&emsp;&emsp;死锁的产生大部分都是在你不知情的时候.我们通过一个例子来看下什么是死锁.

&emsp;&emsp;1.synchronized嵌套.

&emsp;&emsp;synchronized关键字可以保证多线程再访问到synchronized修饰的方法的时候保证了同步性.就是线程A访问到这个方法的时候线程B同时也来访问这个方法,这时线程B将进行阻塞,等待线程A执行完才可以去访问.这里就要用到synchronized所持有的同步锁.具体来看代码:

````
//首先我们先定义两个final的对象锁.可以看做是共有的资源.
final Object lockA = new Object();
final Object lockB = new Object();
//生产者A
class  ProductThreadA implements Runnable{
    @Override
    public void run() {
//这里一定要让线程睡一会儿来模拟处理数据 ,要不然的话死锁的现象不会那么的明显.这里就是同步语句块里面,首先获得对象锁lockA,然后执行一些代码,随后我们需要对象锁lockB去执行另外一些代码.
        synchronized (lockA){
        //这里一个log日志
            Log.e("CHAO","ThreadA lock  lockA");
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (lockB){
             //这里一个log日志
                Log.e("CHAO","ThreadA lock  lockB");
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
        }
    }
}
//生产者B
class  ProductThreadB implements Runnable{
//我们生产的顺序真好好生产者A相反,我们首先需要对象锁lockB,然后需要对象锁lockA.
    @Override
    public void run() {
        synchronized (lockB){
         //这里一个log日志
            Log.e("CHAO","ThreadB lock  lockB");
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (lockA){
             //这里一个log日志
                Log.e("CHAO","ThreadB lock  lockA");
                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
        }
    }
}
//这里运行线程
ProductThreadA productThreadA = new ProductThreadA();
ProductThreadB productThreadB = new ProductThreadB();

    Thread threadA = new Thread(productThreadA);
    Thread threadB = new Thread(productThreadB);
    threadA.start();
    threadB.start();
`````
&emsp;&emsp;分析一下,当threadA开始执行run方法的时候,它会先持有对象锁localA,然后睡眠2秒,这时候threadB也开始执行run方法,它持有的是localB对象锁.当threadA运行到第二个同步方法的时候,发现localB的对象锁不能使用(threadB未释放localB锁),threadA就停在这里等待localB锁.随后threadB也执行到第二个同步方法,去访问localA对象锁的时候发现localA还没有被释放(threadA未释放localA锁),threadB也停在这里等待localA锁释放.就这样两个线程都没办法继续执行下去,进入死锁的状态. 看下运行结果:

````
10-20 14:54:39.940 18162-18178/? E/CHAO: ThreadA lock  lockA
10-20 14:54:39.940 18162-18179/? E/CHAO: ThreadB lock  lockB
````

&emsp;&emsp;通俗的讲就是因为，线程1执行线程2时，发现线程2的锁没有释放，而线程2的锁释放条件是拿到线程1的对象锁，这样的话就形成了死锁。

### 避免死锁的方法

#### 加锁顺序
&emsp;&emsp;当多个线程需要相同的一些锁，但是按照不同的顺序加锁，死锁就很容易发生。

&emsp;&emsp;如果能确保所有的线程都是按照相同的顺序获得锁，那么死锁就不会发生。看下面这个例子：

````
Thread 1:
  lock A 
  lock B

Thread 2:
   wait for A
   lock C (when A locked)

Thread 3:
   wait for A
   wait for B
   wait for C
````
 

&emsp;&emsp;如果一个线程（比如线程3）需要一些锁，那么它必须按照确定的顺序获取锁。它只有获得了从顺序上排在前面的锁之后，才能获取后面的锁。
&emsp;&emsp;例如，线程2和线程3只有在获取了锁A之后才能尝试获取锁C(译者注：获取锁A是获取锁C的必要条件)。因为线程1已经拥有了锁A，所以线程2和3需要一直等到锁A被释放。然后在它们尝试对B或C加锁之前，必须成功地对A加了锁。
&emsp;&emsp;按照顺序加锁是一种有效的死锁预防机制。但是，这种方式需要你事先知道所有可能会用到的锁(译者注：并对这些锁做适当的排序)，但总有些时候是无法预知的。

#### 加锁时限
&emsp;&emsp;另外一个可以避免死锁的方法是在尝试获取锁的时候加一个超时时间，这也就意味着在尝试获取锁的过程中若超过了这个时限该线程则放弃对该锁请求。若一个线程没有在给定的时限内成功获得所有需要的锁，则会进行回退并释放所有已经获得的锁，然后等待一段随机的时间再重试。这段随机的等待时间让其它线程有机会尝试获取相同的这些锁，并且让该应用在没有获得锁的时候可以继续运行(译者注：加锁超时后可以先继续运行干点其它事情，再回头来重复之前加锁的逻辑)。

&emsp;&emsp;以下是一个例子，展示了两个线程以不同的顺序尝试获取相同的两个锁，在发生超时后回退并重试的场景：

````
Thread 1 locks A
Thread 2 locks B

Thread 1 attempts to lock B but is blocked
Thread 2 attempts to lock A but is blocked

Thread 1's lock attempt on B times out
Thread 1 backs up and releases A as well
Thread 1 waits randomly (e.g. 257 millis) before retrying.

Thread 2's lock attempt on A times out
Thread 2 backs up and releases B as well
Thread 2 waits randomly (e.g. 43 millis) before retrying.
```` 

&emsp;&emsp;在上面的例子中，线程2比线程1早200毫秒进行重试加锁，因此它可以先成功地获取到两个锁。这时，线程1尝试获取锁A并且处于等待状态。当线程2结束时，线程1也可以顺利的获得这两个锁（除非线程2或者其它线程在线程1成功获得两个锁之前又获得其中的一些锁）。

&emsp;&emsp;需要注意的是，由于存在锁的超时，所以我们不能认为这种场景就一定是出现了死锁。也可能是因为获得了锁的线程（导致其它线程超时）需要很长的时间去完成它的任务。

&emsp;&emsp;此外，如果有非常多的线程同一时间去竞争同一批资源，就算有超时和回退机制，还是可能会导致这些线程重复地尝试但却始终得不到锁。如果只有两个线程，并且重试的超时时间设定为0到500毫秒之间，这种现象可能不会发生，但是如果是10个或20个线程情况就不同了。因为这些线程等待相等的重试时间的概率就高的多（或者非常接近以至于会出现问题）。
(译者注：超时和重试机制是为了避免在同一时间出现的竞争，但是当线程很多时，其中两个或多个线程的超时时间一样或者接近的可能性就会很大，因此就算出现竞争而导致超时后，由于超时时间一样，它们又会同时开始重试，导致新一轮的竞争，带来了新的问题。)

&emsp;&emsp;这种机制存在一个问题，在Java中不能对synchronized同步块设置超时时间。你需要创建一个自定义锁，或使用Java5中java.util.concurrent包下的工具。写一个自定义锁类不复杂，但超出了本文的内容。后续的Java并发系列会涵盖自定义锁的内容。

#### 死锁检测
&emsp;&emsp;死锁检测是一个更好的死锁预防机制，它主要是针对那些不可能实现按序加锁并且锁超时也不可行的场景。

&emsp;&emsp;每当一个线程获得了锁，会在线程和锁相关的数据结构中（map、graph等等）将其记下。除此之外，每当有线程请求锁，也需要记录在这个数据结构中。

&emsp;&emsp;当一个线程请求锁失败时，这个线程可以遍历锁的关系图看看是否有死锁发生。例如，线程A请求锁7，但是锁7这个时候被线程B持有，这时线程A就可以检查一下线程B是否已经请求了线程A当前所持有的锁。如果线程B确实有这样的请求，那么就是发生了死锁（线程A拥有锁1，请求锁7；线程B拥有锁7，请求锁1）。

&emsp;&emsp;当然，死锁一般要比两个线程互相持有对方的锁这种情况要复杂的多。线程A等待线程B，线程B等待线程C，线程C等待线程D，线程D又在等待线程A。线程A为了检测死锁，它需要递进地检测所有被B请求的锁。从线程B所请求的锁开始，线程A找到了线程C，然后又找到了线程D，发现线程D请求的锁被线程A自己持有着。这是它就知道发生了死锁。

&emsp;&emsp;下面是一幅关于四个线程（A,B,C和D）之间锁占有和请求的关系图。像这样的数据结构就可以被用来检测死锁。

![死锁检测图解](https://www.github.com/only-wjt/images/raw/master/小书匠/1547447926399.png)

&emsp;&emsp;那么当检测出死锁时，这些线程该做些什么呢？

&emsp;&emsp;一个可行的做法是释放所有锁，回退，并且等待一段随机的时间后重试。这个和简单的加锁超时类似，不一样的是只有死锁已经发生了才回退，而不会是因为加锁的请求超时了。虽然有回退和等待，但是如果有大量的线程竞争同一批锁，它们还是会重复地死锁（编者注：原因同超时类似，不能从根本上减轻竞争）。

&emsp;&emsp;一个更好的方案是给这些线程设置优先级，让一个（或几个）线程回退，剩下的线程就像没发生死锁一样继续保持着它们需要的锁。如果赋予这些线程的优先级是固定不变的，同一批线程总是会拥有更高的优先级。为避免这个问题，可以在死锁发生的时候设置随机的优先级。

## 怎么用redis实现多个操作的原子性

&emsp;&emsp;通过Redis的事务来实现.Redis中的事务(transaction)是一组命令的集合。事务同命令一样都是Redis最小的执行单位，一个事务中的命令要么都执行，要么都不执行。Redis事务的实现需要用到 MULTI 和 EXEC 两个命令，事务开始的时候先向Redis服务器发送 MULTI 命令，然后依次发送需要在本次事务中处理的命令，最后再发送 EXEC 命令表示事务命令结束。

## 创建对象的方式

````
使用new关键字							   } → 调用了构造函数
使用Class类的newInstance方法			     } → 调用了构造函数
使用Constructor类的newInstance方法	     } → 调用了构造函数
使用clone方法								 } → 没有调用构造函数
使用反序列化								   } → 没有调用构造函数
````

## HahsMap

我们知道，数据结构的物理存储结构只有两种：顺序存储结构和链式存储结构（像栈，队列，树，图等是从逻辑结构去抽象的，映射到内存中，也这两种物理组织形式），而在上面我们提到过，在数组中根据下标查找某个元素，一次定位就可以达到，哈希表利用了这种特性，哈希表的主干就是数组。

HashMap的主干是一个Entry数组。Entry是HashMap的基本组成单元，每一个Entry包含一个key-value键值对。

````
//HashMap的主干数组，可以看到就是一个Entry数组，初始值为空数组{}，主干数组的长度一定是2的次幂，至于为什么这么做，后面会有详细分析。
transient Entry<K,V>[] table = (Entry<K,V>[]) EMPTY_TABLE;
````

 Entry是HashMap中的一个静态内部类。代码如下

````
static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        Entry<K,V> next;//存储指向下一个Entry的引用，单链表结构
        int hash;//对key的hashcode值进行hash运算后得到的值，存储在Entry，避免重复计算

        /**
         * Creates new entry.
         */
        Entry(int h, K k, V v, Entry<K,V> n) {
            value = v;
            next = n;
            key = k;
            hash = h;
        } 
````

![HashMap的图解](https://www.github.com/only-wjt/images/raw/master/小书匠/1547447929398.png)

简单来说，HashMap由数组+链表组成的，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的，如果定位到的数组位置不含链表（当前entry的next指向null）,那么对于查找，添加等操作很快，仅需一次寻址即可；如果定位到的数组包含链表，对于添加操作，其时间复杂度为O(n)，首先遍历链表，存在即覆盖，否则新增；对于查找操作来讲，仍需遍历链表，然后通过key对象的equals方法逐一比对查找。所以，性能考虑，HashMap中的链表出现越少，性能才会越好。


HashMap ：先说HashMap，HashMap是线程不安全的，在并发环境下，可能会形成环状链表（扩容时可能造成，具体原因自行百度google或查看源码分析），导致get操作时，cpu空转，所以，在并发环境中使用HashMap是非常危险的。

　　HashTable ： HashTable和HashMap的实现原理几乎一样，差别无非是1.HashTable不允许key和value为null；2.HashTable是线程安全的。但是HashTable线程安全的策略实现代价却太大了，简单粗暴，get/put所有相关操作都是synchronized的，这相当于给整个哈希表加了一把大锁，多线程访问时候，只要有一个线程访问或操作该对象，那其他线程只能阻塞，相当于将所有的操作串行化，在竞争激烈的并发场景中性能就会非常差。
  
  HashTable性能差主要是由于所有操作需要竞争同一把锁，而如果容器中有多把锁，每一把锁锁一段数据，这样在多线程访问时不同段的数据时，就不会存在锁竞争了，这样便可以有效地提高并发效率。这就是ConcurrentHashMap所采用的"分段锁"思想。
  
  ConcurrentHashMap采用了非常精妙的"分段锁"策略，ConcurrentHashMap的主干是个Segment数组。

 final Segment<K,V>[] segments;
　　Segment继承了ReentrantLock，所以它就是一种可重入锁（ReentrantLock)。在ConcurrentHashMap，一个Segment就是一个子哈希表，Segment里维护了一个HashEntry数组，并发环境下，对于不同Segment的数据进行操作是不用考虑锁竞争的。（就按默认的ConcurrentLeve为16来讲，理论上就允许16个线程并发执行，有木有很酷）

　　所以，对于同一个Segment的操作才需考虑线程同步，不同的Segment则无需考虑。

Segment类似于HashMap，一个Segment维护着一个HashEntry数组

 transient volatile HashEntry<K,V>[] table;
HashEntry是目前我们提到的最小的逻辑处理单元了。一个ConcurrentHashMap维护一个Segment数组，一个Segment维护一个HashEntry数组。

复制代码
 static final class HashEntry<K,V> {
        final int hash;
        final K key;
        volatile V value;
        volatile HashEntry<K,V> next;
        //其他省略
}    
复制代码
我们说Segment类似哈希表，那么一些属性就跟我们之前提到的HashMap差不离，比如负载因子loadFactor，比如阈值threshold等等，看下Segment的构造方法

Segment(float lf, int threshold, HashEntry<K,V>[] tab) {
            this.loadFactor = lf;//负载因子
            this.threshold = threshold;//阈值
            this.table = tab;//主干数组即HashEntry数组
        }
我们来看下ConcurrentHashMap的构造方法

复制代码
 1  public ConcurrentHashMap(int initialCapacity,
 2                                float loadFactor, int concurrencyLevel) {
 3           if (!(loadFactor > 0) || initialCapacity < 0 || concurrencyLevel <= 0)
 4               throw new IllegalArgumentException();
 5           //MAX_SEGMENTS 为1<<16=65536，也就是最大并发数为65536
 6           if (concurrencyLevel > MAX_SEGMENTS)
 7               concurrencyLevel = MAX_SEGMENTS;
 8           //2的sshif次方等于ssize，例:ssize=16,sshift=4;ssize=32,sshif=5
 9          int sshift = 0;
10          //ssize 为segments数组长度，根据concurrentLevel计算得出
11          int ssize = 1;
12          while (ssize < concurrencyLevel) {
13              ++sshift;
14              ssize <<= 1;
15          }
16          //segmentShift和segmentMask这两个变量在定位segment时会用到，后面会详细讲
17          this.segmentShift = 32 - sshift;
18          this.segmentMask = ssize - 1;
19          if (initialCapacity > MAXIMUM_CAPACITY)
20              initialCapacity = MAXIMUM_CAPACITY;
21          //计算cap的大小，即Segment中HashEntry的数组长度，cap也一定为2的n次方.
22          int c = initialCapacity / ssize;
23          if (c * ssize < initialCapacity)
24              ++c;
25          int cap = MIN_SEGMENT_TABLE_CAPACITY;
26          while (cap < c)
27              cap <<= 1;
28          //创建segments数组并初始化第一个Segment，其余的Segment延迟初始化
29          Segment<K,V> s0 =
30              new Segment<K,V>(loadFactor, (int)(cap * loadFactor),
31                               (HashEntry<K,V>[])new HashEntry[cap]);
32          Segment<K,V>[] ss = (Segment<K,V>[])new Segment[ssize];
33          UNSAFE.putOrderedObject(ss, SBASE, s0); 
34          this.segments = ss;
35      }
复制代码
　　初始化方法有三个参数，如果用户不指定则会使用默认值，initialCapacity为16，loadFactor为0.75（负载因子，扩容时需要参考），concurrentLevel为16。

　　从上面的代码可以看出来,Segment数组的大小ssize是由concurrentLevel来决定的，但是却不一定等于concurrentLevel，ssize一定是大于或等于concurrentLevel的最小的2的次幂。比如：默认情况下concurrentLevel是16，则ssize为16；若concurrentLevel为14，ssize为16；若concurrentLevel为17，则ssize为32。为什么Segment的数组大小一定是2的次幂？其实主要是便于通过按位与的散列算法来定位Segment的index。至于更详细的原因，有兴趣的话可以参考我的另一篇文章《HashMap实现原理及源码分析》，其中对于数组长度为什么一定要是2的次幂有较为详细的分析。

## Jsp的四大作用域与九大对象

- Request(Javax.servlet.ServletRequest)它包含了有关浏览器请求的信息.通过该对象可以获得请求中的头信息、Cookie和请求参数。

- Response(Javax.servlet.ServletResponse)作为JSP页面处理结果返回给用户的响应存储在该对象中。并提供了设置响应内容、响应头以及重定向的方法(如cookies,头信息等)

- Out(Javax.servlet.jsp.JspWriter)用于将内容写入JSP页面实例的输出流中,提供了几个方法使你能用于向浏览器回送输出结果。

- pageContext(Javax.servlet.jsp.PageContext)描述了当前JSP页面的运行环境。可以返回JSP页面的其他隐式对象及其属性的访问,另外,它还实现将控制权从当前页面传输至其他页面的方法。

- Session(javax.servlet.http.HttpSession)会话对象存储有关此会话的信息,也可以将属性赋给一个会话,每个属性都有名称和值。会话对象主要用于存储和检索属性值。

- Application(javax.servle.ServletContext)存储了运行JSP页面的servlet以及在同一应用程序中的任何Web组件的上下文信息。

- Page(Java.lang.Object)表示当前JSP页面的servlet实例

- Config(javax.servlet.ServletConfig)该对象用于存取servlet实例的初始化参数。

- Exception(Javax.lang.Throwable)在某个页面抛出异常时,将转发至JSP错误页面,提供此对象是为了在JSP中处理错误。只有在错误页面中才可使用

四大与对象：request、session、pacontext、application

## jsp和servlet的区别
jsp和servlet的区别和联系：
1.jsp经编译后就变成了Servlet.
(JSP的本质就是Servlet，JVM只能识别java的类，不能识别JSP的代码,Web容器将JSP的代码编译成JVM能够识别的java类)
2.jsp更擅长表现于页面显示,servlet更擅长于逻辑控制.
3.Servlet中没有内置对象，Jsp中的内置对象都是必须通过HttpServletRequest对象，HttpServletResponse对象以及HttpServlet对象得到.
Jsp是Servlet的一种简化，使用Jsp只需要完成程序员需要输出到客户端的内容，Jsp中的Java脚本如何镶嵌到一个类中，由Jsp容器完成。
而Servlet则是个完整的Java类，这个类的Service方法用于生成对客户端的响应。


联系：  
JSP是Servlet技术的扩展，本质上就是Servlet的简易方式。JSP编译后是“类servlet”。
Servlet和JSP最主要的不同点在于：
Servlet的应用逻辑是在Java文件中，并且完全从表示层中的HTML里分离开来。
而JSP的情况是Java和HTML可以组合成一个扩展名为.jsp的文件。
JSP侧重于视图，Servlet主要用于控制逻辑
Servlet更多的是类似于一个Controller，用来做控制。



理解以下三点即可：


1、不同之处在哪？

Servlet在Java代码中通过HttpServletResponse对象动态输出HTML内容
JSP在静态HTML内容中嵌入Java代码，Java代码被动态执行后生成HTML内容
2、各自的特点

Servlet能够很好地组织业务逻辑代码，但是在Java源文件中通过字符串拼接的方式生成动态HTML内容会导致代码维护困难、可读性差
JSP虽然规避了Servlet在生成HTML内容方面的劣势，但是在HTML中混入大量、复杂的业务逻辑同样也是不可取的

## JSTL标签

导入标签库<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

### foreach标签
根据循环条件遍历集合中的元素

var 设定变量名用于存储从集合取得的元素（必须无默认值）

items指定要遍历的集合（必须无默认值）

begin、end用于指定遍历的起始位置和终止位置‘

step指定循环的步长

varStatus通过index、count、first、last几个状态值，描述begin和end子集中的元素状态

<c:forEach var="fruit" items="${fruits }" begin="2" end="4">
        <c:out value="${fruit }"></c:out><br>
    </c:forEach>
	
### out标签

 <c:out value="欢迎您"></c:out>
    <%String str = "user"; request.setAttribute("name", str);%>
    <c:out value="${empty name}"></c:out>
    <!-- 当变量不存在时，通过default输出值 -->
    <c:out value="${defalut }" default="error"></c:out><br>
    <!-- escapeXML设置成false，转义生效 -->
    <c:out value="&ltout标签&gt" escapeXml="false"></c:out>
	
### set标签：

存值到scope中

<c:set value="today" var="day" scope="session"></c:set>
<c:set var="age" scope="application">12</c:set>

### if标签

test属性用于存放判断的条件，一般使用EL表达式来编写

var指定名称用来存放判断的结果类型为true或false

scope用来存放var属性存放的范围

<form action="index.jsp" method="post">
    <!-- 用户输入的数据存入到${param.score}变量中去 -->
        <input type="text" name="score" value="${param.score }"/>
        <input type="submit" value="submit"/>
    </form>
    <c:if test="${param.score>=90 }" var="result">
        <c:out value="优秀"></c:out>
    </c:if>
    <c:out value="${result }"></c:out>
	
## redis集群

## redis单线程原因

## try catch的执行顺序
try{//正常执行的代码}catch (Exception e){//出错后执行的代码}finally{//无论正常执行还是出错,之后都会执行的代码}//跟上面try catch无关的代码正常执行的代码如果出现异常,就不会执行出现异常语句后面的所有正常代码。异常可能会被捕获掉,比如上面catch声明的是捕获Exception,那么所有Exception包括子类都会被捕获,但如Error或者是Throwable但又不是Exception(Exception继承Throwable)就不会被捕获。如果异常被捕获,就会执行catch里面的代码.如果异常没有被捕获,就会往外抛出,相当于这整个方法出现了异常。finally中的代码只要执行进了try catch永远都会被执行.执行完finally中的代码,如果异常被捕获就会执行外面跟这个try catch无关的代码.否则就会继续往外抛出异常。
return无论在哪里,只要执行到就会返回,但唯一一点不同的是如果return在try或者catch中,即使返回了,最终finally中的代码都会被执行.这种情况最常用的是打开了某些资源后必须关闭,比如打开了一个OutputStream,那就应该在finally中关闭,这样无论有没有出现异常,都会被关闭。

---------------------

本文来自 L_D_Y_K 的CSDN 博客 ，全文地址请点击：https://blog.csdn.net/L_D_Y_K/article/details/78166356?utm_source=copy 