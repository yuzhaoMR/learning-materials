# ThreadLocal

ThreadLocal 提供了线程的局部变量，每个线程都可以通过 set()和 get()来对这个局部变量进行操作，但不会和其他线程的局部变量进行冲突，实现了线程的数据隔离。

简要言之：往 ThreadLocal 中填充的变量属于当前线程，该变量对其他线程而言是隔离的。

## 为什么要学习 ThreadLocal？

> 管理 Connection

需要数据库连接池的理由也很简单，频繁创建和关闭 Connection 是一件非常耗费资源的操作，因此需要创建数据库连接池～那么，数据库连接池的连接怎么管理呢？

我们交由 ThreadLocal 来进行管理。为什么交给它来管理呢？？ThreadLocal 能够实现当前线程的操作都是用同一个 Connection，保证了事务！

> 避免一些参数传递

避免一些参数的传递的理解可以参考一下 Cookie 和 Session：

- 每当我访问一个⻚面的时候，浏览器都会帮我们从硬盘中找到对应的 Cookie 发送过去。
- 浏览器是十分聪明的，不会发送别的网站的 Cookie 过去，只带当前网站发布过来的 Cookie 过去

浏览器就相当于我们的 ThreadLocal，它仅仅会发送我们当前浏览器存在的 Cookie(ThreadLocal 的局部变量)，不同的浏览器对 Cookie 是隔离的(Chrome,Opera,IE 的 Cookie 是隔离的【在 Chrome 登陆了，在 IE 你也得重新登陆】)，同样地：线程之间 ThreadLocal 变量也是隔离的

## ThreadLocal 实现的原理

Thread 为每个线程维护了 ThreadLocalMap 这么一个 Map，而 ThreadLocalMap 的 key 是 LocalThread 对象本身，value 则是要存储的对象

- 每个 Thread 维护着一个 ThreadLocalMap 的引用
- ThreadLocalMap 是 ThreadLocal 的内部类，用 Entry 来进行存储
- 调用 ThreadLocal 的 set()方法时，实际上就是往 ThreadLocalMap 设置值，key 是 ThreadLocal 对象，值是传递进来的对象
- 调用 ThreadLocal 的 get()方法时，实际上就是往 ThreadLocalMap 获取值，key 是 ThreadLocal 对象
- ThreadLocal 本身并不存储值，它只是作为一个 key 来让线程从 ThreadLocalMap 获取 value。

## 避免内存泄露

由于 ThreadLocalMap 的生命周期跟 Thread 一样⻓，如果没有手动删除对应 key 就会导致内存泄漏，而不是因为弱引用。

想要避免内存泄露就要手动 remove()掉！

## 总结

ThreadLocal 设计的目的就是为了能够在当前线程中有属于自己的变量，并不是为了解决并发或者共享变量的问题
