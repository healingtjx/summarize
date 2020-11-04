## AQS

- CANCELLED：值为1，在同步队列中等待的线程等待超时或被中断，需要从同步队列中取消该Node的结点，其结点的waitStatus为CANCELLED，即结束状态，进入该状态后的结点将不会再变化。
- SIGNAL：值为-1，被标识为该等待唤醒状态的后继结点，当其前继结点的线程释放了同步锁或被取消，将会通知该后继结点的线程执行。说白了，就是处于唤醒状态，只要前继结点释放锁，就会通知标识为SIGNAL状态的后继结点的线程执行。
- CONDITION：值为-2，与Condition相关，该标识的结点处于等待队列中，结点的线程等待在Condition上，当其他线程调用了Condition的signal()方法后，CONDITION状态的结点将从等待队列转移到同步队列中，等待获取同步锁。
- PROPAGATE：值为-3，与共享模式相关，在共享模式中，该状态标识结点的线程处于可运行状态。
- 0状态：值为0，代表初始化状态。

AQS在判断状态时，通过用waitStatus>0表示取消状态，而waitStatus<0表示有效状

#### **1.acquire流程解释**

1.调用tryAcquire尝试获取锁

2.若没有成功就调用addWaiter就如等待队列的尾部,并且标记为独占模式

3.acquireQueued

3.1首先拿到自己的前驱对象，判断是否未head

3.2若前驱锁head ，则尝试获取锁，获取锁后将head设置为自己。

3.3若前驱不是head，或者尝试获取锁失败了，就将自己的线程状态设置为waiting状态（调用shouldParkAfterFailedAcquire）

4.1shouldParkAfterFailedAcquire方法，首先获取前驱对象，判断前驱对象的状态

4.2如果前驱对象是SIGNAL 状态则 返回true 调用parkAndCheckInterrupt让线程waiting

4.3如果前驱对象不是正常状态，则循环找到前驱中状态正常的现场，并且排到它后面。

4.4如果前驱状态是正常状态，但是不是 SINGNAL状态，就 利用CAS 将前驱设置成SINGAL状态

4.5**parkAndCheckInterrupt** 休眠自己 ，需要unpack唤醒

#### **2.release流程解析**

1.调用tryRelease判断是否可以释放锁

2.获取头节点判断是否未空和判断它是否为刚初始化到状态

3.如果都不是，则调用调用unparkSuccessor 唤醒下一个节点

4.1unparkSuccessor 首先cas调用置零当前线程所在的结点状态，允许失败。

4.2从队尾部开始寻找状态是SIGNAL  的然后将其唤醒

#### **3.acquireShared**

##### **1.ReentrantLook**

与非公平的区别就是在于hasQueuedPredecessors()这个方法，它会先判断阻塞队列中是否已经有节点了(是否已线程在等待获取锁)，有的话，就不尝试获取了，直接排队，因为我们是公平的。

##### **2.Semaphore**

（1）Semaphore，也叫信号量，通常用于控制同一时刻对共享资源的访问上，也就是限流场景；

（2）Semaphore的内部实现是基于AQS的共享锁来实现的；

（3）Semaphore初始化的时候需要指定许可的次数，许可的次数是存储在state中；

（4）获取一个许可时，则state值减1；

（5）释放一个许可时，则state值加1；

（6）可以动态减少n个许可；

（7）可以动态增加n个许可吗？

如何动态增加n个许可？

答：调用release(int permits)即可。我们知道释放许可的时候state的值会相应增加，再回头看看释放许可的源码，发现与ReentrantLock的释放锁还是有点区别的，Semaphore释放许可的时候并不会检查当前线程有没有获取过许可，所以可以调用释放许可的方法动态增加一些许可。

##### **3.CountDownLatch**

##### 4.ReentrantReadWriteLook

https://www.cnblogs.com/tong-yuan/p/ReentrantReadWriteLock.html

##### 5.SyschronousQueue

##### 6.FutureTask