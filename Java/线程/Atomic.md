# Atomic

原子变量类在 java.util.concurrent.atomic 包下，总体来看有这么多个：

- 基本类型：
  - AtomicBoolean：布尔型
  - AtomicInteger：整型
  - AtomicLong：⻓整型
- 数组：
  - AtomicIntegerArray：数组里的整型
  - AtomicLongArray：数组里的⻓整型
  - AtomicReferenceArray：数组里的引用类型
- 引用类型：
  - AtomicReference：引用类型
  - AtomicStampedReference：带有版本号的引用类型
  - AtomicMarkableReference：带有标记位的引用类型
- 对象的属性：
  - AtomicIntegerFieldUpdater：对象的属性是整型
  - AtomicLongFieldUpdater：对象的属性是⻓整型
  - AtomicReferenceFieldUpdater：对象的属性是引用类型
- JDK8 新增 DoubleAccumulator、LongAccumulator、DoubleAdder、LongAdder
  - 是对 AtomicLong 等类的改进。比如 LongAccumulator 与 LongAdder 在高并发环境下比 AtomicLong 更高效。

Atomic 包里的类基本都是使用 Unsafe 实现的包装类。

从原理上概述就是：Atomic 包的类的实现绝大调用 Unsafe 的方法（navtice 方法，他们使用 JNI 的方式访问 C++实现库），而 Unsafe 底层实际上是调用 C 代码，C 代码调用汇编，最后生成出一条 CPU 指令 cmpxchg，完成操作。这也就为啥 CAS 是原子性的，因为它是一条 CPU 指令，不会被打断。

> ABA 问题

- 现在我有一个变量 count=10，现在有三个线程，分别为 A、B、C
- 线程 A 和线程 C 同时读到 count 变量，所以线程 A 和线程 C 的内存值和预期值都为 10
- 此时线程 A 使用 CAS 将 count 值修改成 100 修改完后，就在这时，线程 B 进来了，读取得到 count 的值为 100(内存值和预期值都是 100)，将 count 值修改成 10
- 线程 C 拿到执行权，发现内存值是 10，预期值也是 10，将 count 值修改成 11

> 解决 ABA 问题

要解决 ABA 的问题，我们可以使用 JDK 给我们提供的 AtomicStampedReference 和 AtomicMarkableReference 类。

简单来说就是在给为这个对象提供了一个版本，并且这个版本如果被修改了，是自动更新的。

原理大概就是：维护了一个 Pair 对象，Pair 对象存储我们的对象引用和一个 stamp 值。每次 CAS 比较的是两个 Pair 对象

因为多了一个版本号比较，所以就不会存在 ABA 的问题了。

> LongAdder 性能比 AtomicLong 要好

- 使用 AtomicLong 时，在高并发下大量线程会同时去竞争更新同一个原子变量，但是由于同时只有一个线程的 CAS 会成功，所以其他线程会不断尝试自旋尝试 CAS 操作，这会浪费不少的 CPU 资源。
- 而 LongAdder 可以概括成这样：内部核心数据 value 分离成一个数组(Cell)，每个线程访问时,通过哈希等算法映射到其中一个数字进行计数，而最终的计数结果，则为这个数组的求和累加。
  - 简单来说就是将一个值分散成多个值，在并发的时候就可以分散压力，性能有所提高。
