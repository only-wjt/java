---
title: data persistence 
tags:java,数据持久化
grammar_cjkRuby: true
---


1.什么是持久化？

狭义的理解: “持久化”仅仅指把域对象永久保存到数据库中；广义的理解,“持久化”包括和数据库相关的各种操作。

●     保存：把域对象永久保存到数据库。

●     更新：更新数据库中域对象的状态。

●     删除：从数据库中删除一个域对象。

●     加载：根据特定的OID，把一个域对象从数据库加载到内存。

●     查询：根据特定的查询条件，把符合查询条件的一个或多个域对象从数据库加载内在存中。

2．为什么要持久化？
持久化技术封装了数据访问细节，为大部分业务逻辑提供面向对象的API。

● 通过持久化技术可以减少访问数据库数据次数，增加应用程序执行速度；

● 代码重用性高，能够完成大部分数据库操作；

● 松散耦合，使持久化不依赖于底层数据库和上层业务逻辑实现，更换数据库时只需修改配置文件而不用修改代码。

据持久化对象的基本操作有：保存、更新、删除、查询等。


之前就听说过数据持久化操作这类的词，虽然使用过一点点相关的内容，比如Hibernate中的session这个事物管理器，但一直不理解数据持久化的含义和原理方面，所以在查找了一定的资料后，我总结出了以下内容。

什么是持久化呢？
　　持久化的含义就是把内存中的数据（比如内存中的对象——用对象来封装数据）保存到可永久保存的存储设备或关系型数据库中(常见关系型数据库有：mysql，oracle，sqlserver等)。那么持久层的定义也就很明显了，持久层就是在某个系统中专门实现数据持久化的一个逻辑层面，将数据使用者与数据实体相关联——对象数据映射。

什么是对象数据映射呢？
　　ORM-Object/Relational Mapper——“对象-关系型数据库映射组件”，即在面向对象开发语言（比如java）中的对象与关系型数据库之间建立映射。这就要求需要使用面向对象的开发语言和关系型数据库进行开发。

　　备注：建模领域中的 ORM 为Object/Role Modeling（对象角色建模）。另外这里是“O/R Mapper”而非“O/R Mapping”。相对来讲，O/R Mapping 描述的是一种设计思想或者实现机制，而 O/R Mapper指以O/R原理设计的持久化框架（Framework），包括 O/R机制还有 SQL自生成，事务处理，Cache管理等。

　　所以，综合以上的内容可以大致这样理解：数据持久化就是一种操作对象和关系型数据库之间联系的机制。用比较官方的语言来说，数据持久化就是将内存中的数据模型转换为存储模型,以及将存储模型转换为内存中的数据模型的统称。

　　在这块我再强行举个例子，比如使用过mvc框架的同学，通过对配置文件（如：servletcontext）等内容的更改，可以让我们方便的链接到不同的数据库，并且可以让我们方便的将model对象与数据库进行绑定或者说是映射，然后可以通过一些ORM组件，比如Hibernate，可以方便的对数据库进行增删改查等操作，不用写很多的sql语句，直接通过组件就可以自动生成。

使用数据持久化的好处：
持久化技术封装了数据访问细节，为大部分业务逻辑提供面向对象的API。
通过持久化技术可以减少访问数据库数据次数，增加应用程序执行速度;
代码重用性高，能够完成大部分数据库操作;
松散耦合，使持久化不依赖于底层数据库和上层业务逻辑实现，更换数据库时只需修改配置文件而不用修改代码
为什么要做持久化和ORM设计：
　　在目前的企业应用系统设计中，MVC，即 Model（模型）- View（视图）-Control（控制）为主要的系统架构模式。MVC 中的Model 包含了复杂的业务逻辑和数据逻辑，以及数据存取机制（如 JDBC的连接、SQL生成和Statement创建、还有ResultSet结果集的读取等）等。将这些复杂的业务逻辑和数据逻辑分离，以将系统的紧耦合关系转化为松耦合关系（即解耦合），是降低系统耦合度迫切要做的，也是持久化要做的工作。MVC 模式实现了架构上将表现层（即View）和数据处理层（即Model）分离的解耦合，而持久化的设计则实现了数据处理层内部的业务逻辑和数据逻辑分离的解耦合。而 ORM 作为持久化设计中的最重要也最复杂的技术，也是目前业界热点技术。

简单来说，按通常的系统设计，使用 JDBC 操作数据库，业务处理逻辑和数据存取逻辑是混杂在一起的。



## 总结：

&emsp;&emsp;要说数据持久层，首先要说持久化，所谓持久化就是把内存中的数据保存到可永久化的存储设备或者关系型数据库中（mysql oracle等），所以持久层就是某个系统中专门实现数据持久化的一个逻辑层面，将数据使用者与数据实体相关联--对象数据映射。在mybatis中把数据库的字段映射到持久层中，这样可以实现松散耦合，使业务层不直接访问数据库，这样可以大大减少了访问数据库的次数，增加应用程序执行速度。在更换数据库只需要更改配置文件即可。

