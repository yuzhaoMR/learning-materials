# synchronized 和 lock

> Java 多线程加锁机制

- Synchronized
- 显式 Lock

## Synchronized

synchronized 是 Java 的一个关键字，它能够将代码块(方法)锁起来

> 用法示例

    public synchronized void test() {
    }

> synchronized 是一种互斥锁一次

只能允许一个线程进入被锁住的代码块

> synchronized 是一种内置锁/监视器锁

Java 中每个对象都有一个内置锁(监视器,也可以理解成锁标记)，而 synchronized 就是使用对象的内置锁(监视器)来将代码块(方法)锁定的！ (锁的是对象，但我们同步的是方法/代码块)

> Synchronized 用处是什么？

- synchronized 保证了线程的原子性。(被保护的代码块是一次被执行的，没有任何线程会同时访问)
- synchronized 还保证了可⻅性。(当执行完 synchronized 之后，修改后的变量对其他的线程是可⻅的)

Java 中的 synchronized，通过使用内置锁，来实现对变量的同步操作，进而实现了对变量操作的原子性和其他线程对变量的可⻅性，从而确保了并发情况下的线程安全。

> Synchronized 原理

- 同步代码块：monitorenter 和 monitorexit 指令实现的
- 同步方法: 方法修饰符上的 ACC_SYNCHRONIZED 实现。

synchronized 底层是是通过 monitor 对象，对象有自己的对象头，存储了很多信息，其中一个信息标示是被哪个线程持有。

> 如何使用

- 修饰普通方法
  - 当前对象(内置锁)
- 修饰代码块
  - 当前对象(内置锁)
  - 其他的对象(随便一个对象都有一个内置锁)，解耦锁与当前对象的关系
- 修饰静态方法
  - 类锁(静态方法属于类方法，它属于这个类，获取到的锁是属于类的锁(类的字节码文件对象)）

Tip：

- 获取了类锁的线程和获取了对象锁的线程是不冲突的！
- 锁的持有者是“线程”，而不是“调用”。

## Lock 显示锁

- Lock 方式来获取锁支持中断、超时不获取、是非阻塞的
- 提高了语义化，哪里加锁，哪里解锁都得写出来
- Lock 显式锁可以给我们带来很好的灵活性，但同时我们必须手动释放锁
- 支持 Condition 条件对象
- 允许多个读线程同时访问共享资源

## synchronized 锁和 Lock 锁使用哪个

- synchronized 好用，简单，性能不差
- 没有使用到 Lock 显式锁的特性就不要使用 Lock 锁了，要手动释放锁才行(如果忘了释放，这就是一个隐患)

所以说，我们绝大部分时候还是会使用 Synchronized 锁，用到了 Lock 锁提及的特性，带来的灵活性才会考虑使用 Lock 显式锁
