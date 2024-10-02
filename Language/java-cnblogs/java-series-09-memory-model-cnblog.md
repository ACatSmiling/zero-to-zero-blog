> *`Author: ACatSmiling`*
>
> *`Since: 2024-07-24`*

## 概念

**`Java 内存模型`**：**Java Memory Model，简称 JMM，是 Java 语言中定义的一组规则和规范，用于解决多线程环境下的内存可见性和有序性问题。**JMM 确定了线程之间如何通过内存进行交互，并规定了变量的读取和写入操作的行为。

JMM 能干吗？

- 通过 JMM 来实现线程的工作内存和主内存之间的抽象关系。
- 屏蔽各个硬件平台和操作系统的内存访问差异，以实现让 Java 程序在各种平台下都能达到一致性的内存访问效果。

## 抽象模型

![image-20240724224217335](https://img2023.cnblogs.com/blog/3488201/202410/3488201-20241002230227125-927619358.png)

JMM 规定了所有的变量都存储在主内存中，每个线程都有自己的工作内存，线程自己的工作内存中保存了该线程使用到的主内存变量的副本拷贝，线程对变量的所有操作（读取、赋值等）都必须在线程自己的工作内存中进行，而不能够直接写入主内存中的变量，不同线程之间也无法直接访问对方工作内存中的变量，线程间变量值的传递均需要通过主内存来完成。

- `主内存（Main Memory）`：所有变量都存储在主内存中，主内存是所有线程共享的区域。
- `工作内存（Working Memory）`：每个线程都有自己的工作内存（类似于 CPU 缓存），线程在工作内存中对变量进行操作。工作内存中的变量是主内存中变量的拷贝，线程对变量的修改必须在某个时刻刷新回主内存。

## 三大特性

**`内存可见性`**：JMM 规定了一个线程对共享变量的修改何时对另一个线程可见，在没有适当的同步机制时，线程可能会看到旧的、不一致的数据。

**`原子性`**：JMM 确保基本的读写操作是原子的（不可分割的）。

**`有序性`**：JMM 规定了程序中指令的执行顺序，编译器和处理器可能会对指令进行重排序，但 JMM 规定了哪些重排序是可见的，哪些是不可见的，以确保某些操作不会被重排序而破坏程序的正确性。

## 可见性规则

为了确保多线程编程中的可见性和有序性，JMM 定义了一些关键的同步原语和规则：

1. `volatile 变量`：
   - 对 volatile 变量的读写操作具有可见性和有序性。
   - 当一个线程修改了 volatile 变量，新的值会立即被刷新到主内存中，其他线程读取时会直接从主内存中读取。
2. `synchronized 块`：
   - synchronized 块可以确保进入同步块的线程对共享变量的修改对其他线程可见。
   - 每个对象都有一个监视器锁（monitor lock），线程通过获取锁来实现同步。
3. `final 变量`：
   - final 变量在构造函数结束后不能被修改，且在构造函数中对 final 变量的写入对其他线程可见。

## happens-before 原则

**`happens-before 原则`**：**happens-before 关系定义了一个操作的结果对另一个操作可见的条件，可以用于确定多个操作之间的顺序性和可见性，进而确保了多线程程序中的内存一致性和正确性。**通过遵循这些规则，开发者可以确保在多线程环境中读写共享变量时不会出现意外的行为。

happens-before 原则内容如下：

1. `程序次序规则（Program Order Rule）`：**在一个线程内，按照程序的顺序，前面的操作 happens-before 后面的操作。**例如，在同一个线程中，a = 1; b = 2;，则 a = 1 happens-before b = 2。
2. `监视器锁规则（Monitor Lock Rule）`：**对一个锁的解锁操作 happens-before 其后的对这个锁的加锁操作。**例如，线程 A 对某个对象的解锁操作 happens-before 线程 B 对同一个对象的加锁操作。
3. `volatile 变量规则（Volatile Variable Rule）`：**对一个 volatile 变量的写操作 happens-before 后续对这个 volatile 变量的读操作。**例如，线程 A 对 volatile 变量 x 的写操作 x = 1 happens-before 线程 B 对 x 的读操作 int y = x。
4. `线程启动规则（Thread Start Rule）`：**在主线程中对线程对象的启动操作 happens-before 启动线程中的每一个操作。**例如，主线程调用 thread.start() happens-before 新线程中的任何操作。
5. `线程中断规则（Thread Interruption Rule）`：**对线程对象的中断操作 happens-before 被中断线程检测到中断事件的发生。**例如，主线程调用 thread.interrupt() happens-before 被中断线程检测到中断（通过 Thread.interrupted() 或 Thread.isInterrupted()）。
6. `线程终止规则（Thread Termination Rule）`：**一个线程中的所有操作 happens-before 另一个线程检测到这个线程已经终止或等待这个线程终止。**例如，线程 A 中的所有操作 happens-before 主线程检测到线程 A 已终止（通过 thread.join()）。
7. `对象构造规则（Object Construction Rule）`：**对象的构造函数的执行 happens-before 该对象的 finalize() 方法的开始。**例如，某个对象的构造函数执行完毕 happens-before 该对象的 finalize() 方法开始。
8. `传递性（Transitivity）`：**如果 A happens-before B，且 B happens-before C，那么 A happens-before C。**例如，如果 a = 1 happens-before b = 2，且 b = 2 happens-before c = 3，那么 a = 1 happens-before c = 3。

## 原文链接

https://github.com/ACatSmiling/zero-to-zero/blob/main/JavaLanguage/java-util-concurrent.md