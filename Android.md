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
* post的用法：当使用Handler.post(Runnable r)方法时，传入的参数是一个Runnable对象，Runnable只是一个有着run()方法的接口，便不是一个线程。Handler会通过getPostMessage(Runnable r) 去返回一个Message对象，而将Runnable存储在Message对象的callback属性。Message消息存储进消息队列，当Looper.loop()取出消息，交给handler.dispatchMessage(msg):
```java
public void dispatchMessage(Message msg) {
        if (msg.callback != null) {
            handleCallback(msg);
        } else {
            if (mCallback != null) {
                if (mCallback.handleMessage(msg)) {
                    return;
                }
            }
            handleMessage(msg);
        }
    }
```
从上面的代码可以看到，会通过handleCallback(msg)来处理msg.callback不为空，也就是通过handler.post发送的Runnable，在handleCallback(msg)方法中会通过message.callback.run()来执行Runnable的run()。
* 如何实现延迟消息：不管Handler怎么发送消息，最终都会调用到MessageQueue.enqueueMessage(Message msg, long when)来进行消息入队列，所谓的消息队列并不是队列结构，而是单链表结构。当当前队列为空或者需要入队的Message的延迟时间when比头节点的when小时，则将msg插入头节点，否则，按时间将消息进行排序。所谓消息延迟，并不是延迟去发送消息，而是延迟的去处理，不管是延迟消息还是普通消息，都会第一时间被放到消息队列里。</br>
消息处理在底层是通过epoll实现：Looper.loop()不断的调用MessageQueue.next()方法取出消息，在next()方法中，有一个很重要的底层方法nativePollOnce(ptr, nextPollTimeoutMillis)，nativePollOnce方法会一直阻塞，参数ptr就是底层消息队列NativeMessageQueue的指针，nextPollTimeoutMillis表示nativePollOnce的超时时间。当消息队列为空时，nextPollTimeoutMillis = -1，当取出的msg的when，即延时时间大于0时，nextPollTimeoutMillis = msg.when - now，其中now是当前时间，这两种情况都会使epoll进入休眠，释放CPU资源。当nextPollTimeoutMillis = 0时，epoll就会被唤醒，从管道读取事件放入事件集合，这是MessageQueue.next()返回一个Message对象交给Looper.loop()，交给Handler.dispatchMessage()去处理。
