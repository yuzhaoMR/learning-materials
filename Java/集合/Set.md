# Set

## 特点

元素不可重复

> Set 集合常用子类

- HashSet 集合
  - 无序，允许为 null，底层数据结构是 HashMap(散列表+红黑树)，非线程同步
- TreeSet 集合
  - 有序，不允许为 null，底层是 TreeMap(红黑树),非线程同步
- LinkedHashSet 集合
  - 迭代有序，允许为 null，底层是 HashMap+双向链表，非线程同步

## HashSet

> 为啥要用 HahSet?

假如我们现在想要在一大堆数据中查找 X 数据。LinkedList 的数据结构就不说了，查找效率低的可怕。ArrayList 哪，如果我们不知道 X 的位置序号，还是一样要全部遍历一次直到查到结果，效率一样可怕。HashSet 天生就是为了提高查找效率的。

我们知道 Map 是一个映射，有 key 有 value，既然 HashSet 底层用的是 HashMap，那么 value 在哪里呢？  
value 是一个 Object，所有的 value 都是它。

实际上就是封装了 HashMap，操作 HashSet 元素实际上就是操作 HashMap。这也是面向对象的一种体现，重用性高！

> 要点

- 实现 Set 接口
- 不保证迭代顺序
- 允许元素为 null
- 底层实际上是一个 HashMap 实例（非线程安全）
- 非同步
- 初始容量非常影响迭代性能

## TreeSet

> 要点

- 实现 NavigableSet 接口
- 可以实现排序功能
- 底层实际上是一个 TreeMap 实例
- 非同步

## LinkedHashSet

> 要点

- 迭代是有序的
- 允许为 null
- 底层实际上是一个 HashMap+双向链表实例(其实就是 LinkedHashMap,value 为一个 Object)...
- 非同步
- 性能比 HashSet 差一丢丢，因为要维护一个双向链表
- 初始容量与迭代无关，LinkedHashSet 迭代的是双向链表
- 遍历的是内部维护的双向链表，所以说初始容量对 LinkedHashSet 遍历是不受影响的
