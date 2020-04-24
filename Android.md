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

