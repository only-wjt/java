---
title: interview 
tags:interview,essay
grammar_cjkRuby: true
---


## 反向代理是什么，有什么作用

&emsp;&emsp;简单的说从一个局域网出来到服务端为正向，从客户端要进入一个局域网为反向

&emsp;&emsp;我们可以先从正向代理入手，正向代理一般用来作为对访问不到的资源的代理，我们知道从谁那得到的资源，但是资源处不知道具体是谁得到。

![Diagram](./attachments/1539759743844.drawio.html)

&emsp;&emsp;就想A向B借钱，但是B没有钱，于是B就向C借钱，然后把借来的钱给A，A并不知道这是B从C那借来的钱。

### 概念

&emsp;&emsp;反向代理（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个服务器。


## nginx的作用，以及实现负载均衡


## try catch的执行顺序
try{//正常执行的代码}catch (Exception e){//出错后执行的代码}finally{//无论正常执行还是出错,之后都会执行的代码}//跟上面try catch无关的代码正常执行的代码如果出现异常,就不会执行出现异常语句后面的所有正常代码。异常可能会被捕获掉,比如上面catch声明的是捕获Exception,那么所有Exception包括子类都会被捕获,但如Error或者是Throwable但又不是Exception(Exception继承Throwable)就不会被捕获。如果异常被捕获,就会执行catch里面的代码.如果异常没有被捕获,就会往外抛出,相当于这整个方法出现了异常。finally中的代码只要执行进了try catch永远都会被执行.执行完finally中的代码,如果异常被捕获就会执行外面跟这个try catch无关的代码.否则就会继续往外抛出异常。
return无论在哪里,只要执行到就会返回,但唯一一点不同的是如果return在try或者catch中,即使返回了,最终finally中的代码都会被执行.这种情况最常用的是打开了某些资源后必须关闭,比如打开了一个OutputStream,那就应该在finally中关闭,这样无论有没有出现异常,都会被关闭。

---------------------

本文来自 L_D_Y_K 的CSDN 博客 ，全文地址请点击：https://blog.csdn.net/L_D_Y_K/article/details/78166356?utm_source=copy 