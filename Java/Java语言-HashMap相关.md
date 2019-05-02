# HashMap相关

1. 什么时候会使用HashMap？它有什么特点？
2. HashMap的工作原理
3. get和put的原理, equals()和hashCode()的都有什么作用？
4. hash的实现及原因？
5. 如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？

#  什么时候会使用HashMap？它有什么特点？

HashMap是基于实现Map接口的散列表，这个实现提供了所有可选的map操作，并且允许null的键值对。它是非同步的，不保证有序，也不保证顺序不随事件变化。(HashTable是同步的，并且不接受null的键值对)。

假设元素的散列值合理的分布在桶中，HashMap可以提供常数级别复杂度的get()和put()操作。

HashMap的性能收到两个参数的影响，初始容量(initialCapacity)和负载因子(Load factor)。容量(Capacity)就是buckets的数目，负载因子(Load factor)就是buckets填满程度的最大比例。当bucket填充的数目（即hashmap中元素的个数）大于capacity*load factor时就需要调整buckets的数目为当前的2倍。默认负载因子是0.75。

# HashMap的工作原理

通过hash的方法，通过put和get存储和获取对象。存储对象时，我们将K/V传给put方法时，它调用hashCode计算hash从而得到bucket位置，进一步存储，HashMap会根据当前bucket的占用情况自动调整容量(超过Load Facotr则resize为原来的2倍)。获取对象时，我们将K传给get，它调用hashCode计算hash从而得到bucket位置，并进一步调用equals()方法确定键值对。如果发生碰撞的时候，Hashmap通过链表将产生碰撞冲突的元素组织起来，在Java 8中，如果一个bucket中碰撞冲突的元素超过某个限制(默认是8)，则使用红黑树来替换链表，从而提高速度。

# get和put的原理,equals()和hashCode()的都有什么作用？

通过对key的hashCode()进行hashing，并计算下标( n-1 & hash)，从而获得buckets的位置。如果产生碰撞，则利用key.equals()方法去链表或树中去查找对应的节点

# hash的实现及原因

在Java 1.8的实现中，是通过hashCode()的高16位异或低16位实现的：(h = k.hashCode()) ^ (h >>> 16)，主要是从速度、功效、质量来考虑的，这么做可以在bucket的n比较小的时候，也能保证考虑到高低bit都参与到hash的计算中，同时不会有太大的开销。

# 如果HashMap的大小超过了负载因子(load factor)定义的容量，怎么办？

如果超过了负载因子(默认0.75)，则会重新resize一个原来长度两倍的HashMap，并且重新调用hash方法。

# Java 1.8 中 HashMap 的不同

在 Java 1.8 中，如果链表的长度超过了 8 ，那么链表将转化为红黑树；
发生 hash 碰撞时，Java 1.7 会在链表头部插入，而 Java 1.8 会在链表尾部插入；
在 Java 1.8 中，Entry 被 Node 代替（换了一个马甲）