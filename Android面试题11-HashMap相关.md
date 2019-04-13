1. HashMap的关键成员变量
2. Java 1.8 中 HashMap 的不同

# HashMap的关键成员变量

**initialCapacity**：初始容量。指的是 HashMap 集合初始化的时候自身的容量。可以在构造方法中指定；如果不指定的话，总容量默认值是 16 。需要注意的是初始容量必须是 2 的幂次方。
**size**：当前 HashMap 中已经存储着的键值对数量，即 HashMap.size() 。
**loadFactor**：加载因子。所谓的加载因子就是 HashMap (当前的容量/总容量) 到达一定值的时候，HashMap 会实施扩容。加载因子也可以通过构造方法中指定，默认的值是 0.75 。举个例子，假设有一个 HashMap 的初始容量为 16 ，那么扩容的阀值就是 0.75 * 16 = 12 。也就是说，在你打算存入第 13 个值的时候，HashMap 会先执行扩容。
**threshold**：扩容阀值。即 扩容阀值 = HashMap 总容量 * 加载因子。当前 HashMap 的容量大于或等于扩容阀值的时候就会去执行扩容。扩容的容量为当前 HashMap 总容量的两倍。比如，当前 HashMap 的总容量为 16 ，那么扩容之后为 32 。
**table**：Entry 数组。我们都知道 HashMap 内部存储 key/value 是通过 Entry 这个介质来实现的。而 table 就是 Entry 数组。
在 Java 1.7 中，HashMap 的实现方法是数组 + 链表的形式。上面的 table 就是数组，而数组中的每个元素，都是链表的第一个结点。

# Java 1.8 中 HashMap 的不同

在 Java 1.8 中，如果链表的长度超过了 8 ，那么链表将转化为红黑树；
发生 hash 碰撞时，Java 1.7 会在链表头部插入，而 Java 1.8 会在链表尾部插入；
在 Java 1.8 中，Entry 被 Node 代替（换了一个马甲）