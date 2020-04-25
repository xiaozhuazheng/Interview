## 记录面试中遇到的Android问题！

### 1、intent机制，传递数据大小限制
真正限制大小的不是Intent，是Binder：</br>
https://mp.weixin.qq.com/s/6tAHfoBRx8ZhEZjpNXLUmA

### 2、ListView与RecycleView源码，RecycleView可以取代ListView吗？

### 3、如何加载一张尺寸很大的图？

### 4、Sqlite如何优化

### 5、HashMap
基本的概念以及逻辑就不累述了，主要是数组加链表的组合，JDK 1.8之后添加了当链表长度大于8之后采用红黑树的逻辑。
* key的hash值是如何计算的,(h = key.hashCode()) ^ (h >>> 16)，最后通过得到的hash值和桶的长度取模，确定应该所在桶的位置；
* loadFactor：负载因子，默认为0.75，在HashMap的构造函数中设定；
* length：HashMap当前哈希桶的长度；
* threshold：阈值，当HashMap的键值对总数超过该阈值时，threshold = length * loadFactor，则开启扩容机制，将桶的长度为当前的两倍，并将已存在的数据重新计算位置； 

### 6、UI线程的looper一直在无限循环，为什么不ANR？
* ANR机制：什么情况下会造成ANR，通常会回答Activity最长执行时间为5秒、BroadcastReceiver最长之间超过15秒、Service最长执行时间超过20秒等。当我们通过StartActivity去启动一个Activity时，系统会向主线程的MessageQueue即消息队列里添加一个时间为5秒的延迟消息，在5秒内如果完成了Activity的启动，那么就会移除这个消息，反之，主线的handler在5秒后就会从MessageQueue消息队列中取出该消息并处理，即弹出ANR弹窗，Service以及BroadcastReceiver也一样。因此，是否造成ANR的条件为是否影响相关事件的完成，比如我们在Activity的onCreat()方法里添加了耗时的操作影响了Activity的启动，就会造成ANR。
* 在主线程里添加耗时操作一定会造成ANR吗？ 其实不是，只要我们的操作不影响Activity的启动，Services启动，输入事件的响应等，都不会造成ANR。
* ActivityThread类中的main是Android的入口，ActivityThread并不是主线程，并没有实现Thread类，只是运行在由Zygote fork而创建的App进程也是我们的主线程里，在main()方法中，会获取主线程Looper并开启消息循环。当我们StartActivity去启动一个Activity时，是app进程与AMS进程间的通信，android中进程间的通信是通过Binder实现。当AMS收到启动Activity需求后，会通过在ActivityThread类中的Binder服务端ApplicationThread线程调用ActivityThread中的handler发送launchActivity的消息，ActivityThread然后通过handMessage方法去处理消息，创建Activity类。
* 对于一个线程来说，执行完操作后就结束线程，但对于主线程来说，需要随时处理消息，因此不能销毁线程，需要一直保存running状态，怎么办呢，那就是通过Looper的死循环。那么，主线程的死循环一直运行是不是特别消耗CPU资源呢？ 其实不然，这里就涉及到Linux pipe/epoll机制，简单说就是在主线程的MessageQueue没有消息时，便阻塞在loop的queue.next()中的nativePollOnce()方法里，此时主线程会释放CPU资源进入休眠状态，直到下个消息到达或者有事务发生，通过往pipe管道写端写入数据来唤醒主线程工作。这里采用的epoll机制，是一种IO多路复用机制，可以同时监控多个描述符，当某个描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作，本质同步I/O，即读写是阻塞的。 所以说，主线程大多数时候都是处于休眠状态，并不会消耗大量CPU资源。

### 7、关于Handler值得注意的地方？
