阅读

https://www.javadoop.com/post/spring-aop-source

## 面试题

#### 1.你说使用到了 AOP ，能谈谈它的实现原理嘛？

在调用getBean 时候,底层调用实例话对象时候，会调用 BeanPostProcessor,里面会先查询出该bean 匹配的 advisor ，然后把 bean 的基本元素，和匹配的advisor

进行封装，最后根据配置（例如proxy-target-class="true"），和是否有接口，判断到是使用JDK自身代理，还是 CJLB 进行代理

#### 2.两种代理方式的优缺点。

JDK 代理：就要求 目标类必须要有接口

CGLB代理：原理是继承，所以目标类和目标方法不能说final的

效率的话：1.6的时候 JDK 代理很慢，1.7估计优化啦，效率和cglb差不多，1.8之后效率大幅度提升

#### 3.Spring AOP 有哪些不同的通知类型

1.前置通知(Before Advice): 在连接点之前执行的Advice，不过除非它抛出异常，否则没有能力中断执行流。使用 @Before 注解使用这个Advice。

2.返回之后通知(After Retuning Advice): 在连接点正常结束之后执行的Advice。例如，如果一个方法没有抛出异常正常返回。通过 @AfterReturning 关注使用它。

3.抛出（异常）后执行通知(After Throwing Advice): 如果一个方法通过抛出异常来退出的话，这个Advice就会被执行。通用 @AfterThrowing 注解来使用。

4.后置通知(After Advice): 无论连接点是通过什么方式退出的(正常返回或者抛出异常)都会执行在结束后执行这些Advice。通过 @After 注解使用。

5.围绕通知(Around Advice): 围绕连接点执行的Advice，就你一个方法调用。这是最强大的Advice。通过 @Around 注解使用。

#### 4.描述下 AOP

面向切面编程，主要应用于 事务管理，权限，日志等方面

主要实现方式：

1.xml 注解实现

2.@AspectJ注解

#### 5.在Spring AOP中关注点和横切关注点有什么不同？

**关注点是我们想在应用的模块中实现的行为**。关注点可以被定义为：我们想实现以解决特定业务问题的方法。比如，在所有电子商务应用中，不同的关注点（或者模块）可能是库存管理、航运管理、用户管理等。

**横切关注点是贯穿整个应用程序的关注点**。像日志、安全和数据转换，它们在应用的每一个模块都是必须的，所以他们是一种横切关注点。

简单来说，关注点就是具体到某一个模块，横切关注点就是贯穿整个项目

#### 6.AOP有哪些可用的实现？

1.AspectJ

2.Spirng AOP

3.JBoos AOP

#### 7.Spring AOP 代理是什么？

代理是使用非常广泛的设计模式。简单来说，代理是一个看其他像另一个对象的对象，但它添加了一些特殊的功能。

Spring AOP是基于代理实现的。AOP 代理是一个由 AOP 框架创建的用于在运行时实现切面协议的对象。

Spring AOP默认为 AOP 代理使用标准的 JDK 动态代理。这使得任何接口（或者接口的集合）可以被代理。Spring AOP 也可以使用 CGLIB 代理。这对代理类而不是接口是必须的。

如果业务对象没有实现任何接口那么默认使用CGLIB。

\8. 连接点(Joint Point)和切入点(Point cut)是什么？

连接点 是具体执行的方法

切入点是 匹配连接点的表达式

#### 9.什么是织入(weaving)？

Spring AOP 框架仅支持有限的几个 AspectJ 切入点的类型，它允许将切面运用到在 IoC 容器中声明的 bean 上。如果你想使用额外的切入点类型或者将切面应用到在 Spring IoC 容器外部创建的类，那么你必须在你的 Spring 程序中使用 AspectJ 框架，并且使用它的织入特性。

简单理解就是在 spring 容器以外的元素，进行AOP

##### AspectJ  后编译时织入

AspectJ 编译时织入是通过一个叫做 ajc 特殊的 AspectJ 编译器完成的。它可以将切面织入到你的 Java 源码文件中，然后输出织入后的二进制 class 文件。它也可以将切面织入你的编译后的 class 文件或者 Jar 文件。这个过程叫做后编译时织入(post-compile-time weaving)