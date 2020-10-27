# 第5章 Java线程

## 使用线程
- 使用实现 Runnable 接口的类
- 使用实现 Runnable 接口的 Thread 类
- 使用继承 实现 Runnable 接口的 Thread类的类

## 线程的状态
- 创建状态  
实例化一个线程对象后,线程就处于创建状态。处于创建状态的线程,系统不为其分配资源。
- 可运行状态  
当线程对象调用了run()方法后,线程就处于可运行状态。处于可运行状态的线程,系统为其分配了所需的系统资源。
- 不可运行状态  
线程成为不可运行状态可能是因为调用了sleep()方法、suspend()方法、wait()方法或输入输出/流中发生线程阻塞。
- 死亡状态  
线程执行完或是调用了stop()方法都会进入死亡状态。

### 线程主动放弃CPU可原因:
- 线程调用了yield()或sleep()方法主动放弃。
- 线程调用wait()方法。
- 由于当前线程进行I/O访问、外存读写、等待用户输入等操作,因此导致线程阻塞。

### 线程优先级
- Thread.MIN_PRIORITY 代表最低优先级 1
- Thread.NORM_PRIORITY 代表默认优先级 5
- Thread.MAX_PRIORITY 代表最高优先级 10

### 守护线程
线程默认都是非守护线程,非守护线程也称作用户线程。当所有用户线程都结束时,守护线程也立即结束。因为守护线程随时会结束,所以守护线程所做的工作应该是可以随时结束而不影响运行结果的工作。

## 线程同步
当多个线程共享同一个变量等资源时,需要确保资源在某一时刻只有一个线程占用,这个过程就是线程同步。

```
public class HelloWorld {
        public static void main(String[] args) {
                Ticket ticket = new Ticket();
                for (int i = 1; i <= 4; i++) {
                        Thread t = new Thread(ticket);
                        t.setName("窗口" + i);
                        if (i == 3) {
                                t.setPriority(10);
                        } else {
                                t.setPriority(1);
                        }
                        t.start();
                }
        }
}

/**
 * Ticket
 */
class Ticket implements Runnable {
        private static int ticket = 100;

        @Override
        public void run() {
                synchronized (this) {
                        while (Ticket.ticket > 0) {
                                System.out.println(Thread.currentThread().getName() + ": " + Ticket.ticket);
                                Ticket.ticket--;
                                try {
                                        wait(50);
                                } catch (InterruptedException e) {
                                        e.printStackTrace();
                                }
                        }
                }
        }
}
```

```
public class HelloWorld {
        public static void main(String[] args) {
                for (int i = 1; i <= 4; i++) {
                        Thread t = new Ticket();
                        t.setName("窗口" + i);
                        if (i == 3) {
                                t.setPriority(10);
                        } else {
                                t.setPriority(1);
                        }
                        t.start();
                }
        }
}

/**
 * Ticket
 */
class Ticket extends Thread {
        private static int ticket = 100;

        @Override
        public void run() {
                synchronized (this) {
                        while (Ticket.ticket > 0) {
                                System.out.println(Thread.currentThread().getName() + ": " + Ticket.ticket);
                                Ticket.ticket--;
                                try {
                                        wait(50);
                                } catch (InterruptedException e) {
                                        e.printStackTrace();
                                }
                        }
                }
        }
}
```

## 练习
### 填空题
1. 每个进程都有__的代码和数据空间或称为进程上下文,进程切换的开销__。而线程可以看作是轻量级的进程,同一类线程__代码和数据空间,每个线程有__的运行栈和程序计数器,线程切换与进程切换相比开销要__。
2. Java中的线程由三部分组成:__、__和 __。
3. 线程有4种状态:__、__、__和 __。
4. Java中线程有__个优先级由低到高分别用__~ __表示。
5. 线程默认都是非守护线程,非守护线程也称作__。

### 选择题
1. Thread.NORM_PRIORITY对应的级别数是__。  
A. 0  
B. 1  
C. 5  
D. 10
2. Thread thread=new Thread();如果要将thread设置为守护线程,那么应该如何编写代码。请选择__。  
A. thread.setDaemon(true)  
B. thread.setDaemon(1)  
C. thread.setDaemon(False)  
D. thread.setDaemon(0)
3. 下列哪种线程是JVM自动创建的__。  
A. 守护线程  
B. 主线程  
C. 非守护线程  
D. 用户线程
4. 实现线程体的方式除了继承Thread类外,还可以实现以下哪个接口__。  
A. Cloneable  
B. Runnable  
C. Iterable  
D. Serializable
5. Java中实现线程同步的关键字是__。  
A. static  
B. final  
C. synchronized  
D. protected

### 问答题
1. 简述线程和进程的关系。
2. 什么是守护线程?
3. 什么是主线程?主线程的特点是什么?
4. 简述实现线程体两种方式的使用范围。

### 编程练习
分别用两种不同的方式实现线程体。