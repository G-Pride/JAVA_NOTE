## CopyOnWrite容器

所谓的CopyOnWrite，顾名思义，他主要是一个写时复制的一个功能。

如果说我们操作的一个对象是ArrayList，假设不支持CopyOnWrite，当我发生读写并发的时候，或者说两个写操作并发的时候，对我们ArrayList的结构就是有破坏的，因此CopyOnWrite的作用是：当容器发生了CopyOnWrite的写操作，会根据当前对象开拓一个副本，并且写完副本之后，替换掉原来的内存结构，使得其可以快速的切换成写之后的一个状态。

如果说在写操作的过程当中发生了读操作，其实读的并不是写的副本，而是原始对象，相当于是一个读快照的一个概念，这样的话可以在容器发生写操作的时候不影响读，但是读的是一个旧的数据。

面试官经常会问了一个问题是：如果说我并发发生了两次写操作，它难道是开拓了两个对应的副本吗？其实CopyOnWrite容器本身不是这样去实现的。我们对应的一个写操作的副本是会加锁的，也就是说，如果说发生并发写，同一时间是只有一个写操作，需要等待另一个写操作完成之后，才能进入下一次的一个写的过程，因此CopyOnWrite本质上来说写操作是串行的，读操作是并行的。



作用是处理容器的线程安全，就像ConcurrentHashMap之于HashMap。

使用方法

```java
//线程安全的+无序+可重复 集合
List<Integer> list = new CopyOnWriteArrayList<>();

//线程安全的+无序+不重复 集合，CopyOnWriteArraySet的底层还是通过CopyOnWriteArrayList来实现的
Set<Integer> set = new CopyOnWriteArraySet<>();
```

