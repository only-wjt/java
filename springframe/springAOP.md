---
title: springAOP
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---


**动态代理见[动态代理](https://github.com/only-wjt/java/blob/master/basic/%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86.md)**
Spring AOP通过代理模式实现，目前支持两种代理：JDK动态代理、CGLIB代理来创建AOP代理，Spring建议优先使用JDK动态代理。

JDK动态代理：使用java.lang.reflect.Proxy动态代理实现，即提取目标对象的接口，然后对接口创建AOP代理。

CGLIB代理：CGLIB代理不仅能进行接口代理，也能进行类代理，CGLIB代理需要注意以下问题：

不能通知final方法，因为final方法不能被覆盖（CGLIB通过生成子类来创建代理）。

会产生两次构造器调用，第一次是目标类的构造器调用，第二次是CGLIB生成的代理类的构造器调用。如果需要CGLIB代理方法，请确保两次构造器调用不影响应用。

Spring AOP默认首先使用JDK动态代理来代理目标对象，如果目标对象没有实现任何接口将使用CGLIB代理，如果需要强制使用CGLIB代理，请使用如下方式指定：

对于Schema风格配置切面使用如下方式来指定使用CGLIB代理：

java代码：

1
2
<aop:config proxy-target-class="true"> 
</aop:config>
而如果使用@AspectJ风格使用如下方式来指定使用CGLIB代理：

java代码：

1
<aop:aspectj-autoproxy proxy-target-class="true"/>