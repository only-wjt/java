---
title: mybatis的四种映射方式
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---


[四种映射方式](https://blog.csdn.net/lmy86263/article/details/53150091)

resultMap

as

注解

驼峰命名法


2.	MyBatis 核心配置文件中有哪些常用配置

连接数据库的配置单独放在一个properties文件中

为实体类定义别名，简化sql映射xml文件中的引用

mappers（映射器）

 parameterType和resultType

parameterType：指定输入参数类型，mybatis通过ognl从输入对象中获取参数值拼接在sql中。

resultType：指定输出结果类型，mybatis将sql查询结果的一行记录数据映射为resultType指定类型的对象。


3.	MyBatis 映射文件中常用元素定义有哪些?

[常用元素](https://blog.csdn.net/weixin_36279318/article/details/79993180)


3)	代理对象的方法执行时，是会调用DefaultSqlSession对象的对应方法吗？(是,例如selectOne,update,insert,….)
4)	DefaultSqlSession 对象底层操作数据库时，会调用Executor对应的相关方法吗？（会）
5)	Executor 对象是由谁创建的？（Configuration）
6)	Configuration对象是由谁创建的？(SqlSessionFactoryBuilder)
7)	Executor 底层执行时是要访问JDBCAPI吗？（是）
