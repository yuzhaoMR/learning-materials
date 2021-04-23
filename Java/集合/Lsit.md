# List

## 特点

有序(存储顺序和取出顺序一致),可重复

> List 集合常用的子类有三个：

- ArrayList:底层数据结构是数组。线程不安全
- LinkedList:底层数据结构是双向链表（双向链表方便实现往前遍历）。线程不安全
- Vector:底层数据结构是数组。线程安全

## ArrayList

ArrayList 底层其实就是一个数组，ArrayList 中有扩容这么一个概念，正因为它扩容，所以它能够实现“动态”增长

- ArrayList 是基于动态数组实现的，在增删时候，需要数组的拷贝复制。
- ArrayList 的默认初始化容量是 10，每次扩容时候增加原先容量的一半，也就是变为原来的 1.5 倍
- 删除元素时不会减少容量，若希望减少容量则调用 trimToSize()
- 它不是线程安全的。它能存放 null 值。
- 在增删时候，需要数组的拷贝复制(navite 方法由 C/C++实现)，频繁操作有性能问题

> 解决 ArrayList 线程不安全的问题

- Collections.synchronizedList（一般使用这个）
- 为 list.add()方法加锁
- CopyOnWriteArrayList（写少读多的场景：推荐）
- 使用 ThreadLocal（每 new 一个新的线程，变量也会 new 一次，一定程度上会造成性能[内存]损耗）

## Vector 与 ArrayList 区别

Vector 底层也是数组，与 ArrayList 最大的区别就是：同步(线程安全)

如果想要 ArrayList 实现同步，可以使用 Collections 的方法：  
List list = Collections.synchronizedList(new ArrayList(...))

- ArrayList 在底层数组不够用时在原来的基础上扩展 0.5 倍，Vector 是扩展 1 倍。
- Vector 所有方法都是同步，有性能损失。
- Vector 初始 length 是 10 超过 length 时 以 100%比率增长，相比于 ArrayList 更多消耗内存。

> Tip

总的来说：查询多用 ArrayList，增删多用 LinkedList
