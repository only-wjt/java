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

2、  parameterType和resultType

parameterType：指定输入参数类型，mybatis通过ognl从输入对象中获取参数值拼接在sql中。

resultType：指定输出结果类型，mybatis将sql查询结果的一行记录数据映射为resultType指定类型的对象。