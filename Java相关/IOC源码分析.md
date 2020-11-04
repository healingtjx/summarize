IOC主要有两个流程，一个是注入，一个是实例化

大致单步跟了下Spring IOC的初始化过程，整个脉络很庞大，初始化的过程主要就是读取XML资源，并解析，最终注册到Bean Factory中：

![img](../assets/IOC源码分析/032179be-070b-11e5-9ecf-d7befc804e9d.png)

加载时需要读取、解析、注册bean，这个过程具体的调用栈如下所示： 

![img](../assets/IOC源码分析/8a488060-06e6-11e5-9ad9-4ddd3375984f.png)

解析xml对应到bean

![img](../assets/IOC源码分析/eae02bc6-06e6-11e5-941a-d1f59e3b363f.png)

注入依赖

![img](../assets/IOC源码分析/cec01bcc-092f-11e5-81ad-88c285f33845.png)

**相关面试题**

https://blog.csdn.net/a745233700/article/details/80959716