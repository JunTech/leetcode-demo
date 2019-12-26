# leetcode1114-按序打印

## 1.问题描述

```json
我们提供了一个类：
public class Foo {

  public void one() { print("one"); }

  public void two() { print("two"); }

  public void three() { print("three"); }

}

三个不同的线程将会共用一个 Foo 实例。

线程 A 将会调用 one() 方法

线程 B 将会调用 two() 方法

线程 C 将会调用 three() 方法

请设计修改程序，以确保 two() 方法在 one() 方法之后被执行，three() 方法在 two() 方法之后被执行。

 

示例 1:

输入: [1,2,3]

输出: "onetwothree"

解释: 

有三个线程会被异步启动。

输入 [1,2,3] 表示线程 A 将会调用 one() 方法，线程 B 将会调用 two() 方法，线程 C 将会调用 three() 方法。

正确的输出是 "onetwothree"。

示例 2:

输入: [1,3,2]

输出: "onetwothree"

解释: 

输入 [1,3,2] 表示线程 A 将会调用 one() 方法，线程 B 将会调用 three() 方法，线程 C 将会调用 two() 方法。

正确的输出是 "onetwothree"。

```

## 2.解题思路

### 2.1加标识运行线程

加一个标识flag初始化为1，1运行完了之后flag+1,然后判断flag值进行线程的运行

代码：

```
class Foo {
    private volatile int flag=1;
    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
        
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        flag=2;
    }

    public void second(Runnable printSecond) throws InterruptedException {
        
        // printSecond.run() outputs "second". Do not change or remove this line.
        while(flag!=2);
        printSecond.run();
        flag=3;
    }

    public void third(Runnable printThird) throws InterruptedException {
        
        // printThird.run() outputs "third". Do not change or remove this line.   
        while(flag!=3);
        printThird.run();
    }
}
```

在这里说一下volatile:

```
volatile 的特性
保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。（实现可见性）
禁止进行指令重排序。（实现有序性）
volatile 只能保证对单次读/写的原子性。i++ 这种操作不能保证原子性。

```

### 2.2 使用CountDownLatch

​	countDownLatch这个类使一个线程等待其他线程各自执行完毕后再执行。是通过一个计数器来实现的，计数器的初始值是线程的数量。每当一个线程执行完毕后，计数器的值就-1，当计数器的值为0时，表示所有线程都执行完毕，然后在闭锁上等待的线程就可以恢复工作了。

​	1:首先考虑使用一个线程安全的线程标记，选择了countDownLatch计数器。
	2:根据题目描述就是在线程1,2,3不论谁先执行，都要使执行结果为first - second - third
	3:分析countDownLatch计数器，他的作用就是初始化一个计数器数值指定，只有在该计数器的值为0时才继续执行。

--------------------------------------------------------------------------------------------------------------------------------------------------------

​	1、我们先创建两个计数器 c2 是second的，c3是third 初始化都为1.
	2、我们可以使用它的.countDown（解释：使当前计数器减一）方法在first执行后将c2的计数器置为0，在c2执行 后通过.countDown方法将c3的计数器置为0。
	3、同时在应对如果 second、third先执行，在他们之中.await（解释：如果当前计数器不为0，那么挂起，等待 计数器为0后继续执行）自己的计数器。
	4：问题解决。

理解例子

```
countdownlatch 是一个同步类工具，不涉及锁定，当count的值为零时当前线程继续运行，不涉及同步，只涉及线程通信的时候，使用它较为合适

public class testLatch {

    public static void main(String[] args) {
        CountDownLatch begin = new CountDownLatch(1);
        CountDownLatch end = new CountDownLatch(2);

        for(int i=0; i<2; i++){
            Thread thread = new Thread(new Player(begin,end),String.valueOf(i));
            thread.start();
        }

        try{
            System.out.println("the race begin");
            begin.countDown();
            end.await();//await() 方法具有阻塞作用，也就是说主线程在这里暂停
            System.out.println("the race end");
        }catch(Exception e){
            e.printStackTrace();
        }

    }
}



class Player implements Runnable{

    private CountDownLatch begin;

    private CountDownLatch end;

    Player(CountDownLatch begin,CountDownLatch end){
        this.begin = begin;
        this.end = end;
    }

    public void run() {

        try {
            
            System.out.println(Thread.currentThread().getName() + " start !");;
            begin.await();//因为此时已经为0了，所以不阻塞
            System.out.println(Thread.currentThread().getName() + " arrived !");

            end.countDown();//countDown() 并不是直接唤醒线程,当end.getCount()为0时线程会自动唤醒

        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}



主线程main被end.await();阻塞，两个副线程继续往下运行，因为benin已经为0，所以不阻塞，操作两次end的downcount之后，主线程继续往下执行。如果不理解可以将线程数改成3试一下

注：end.countDown() 可以在多个线程中调用 计算调用次数是所有线程调用次数的总和

```

代码：

```
class Foo {
    private CountDownLatch c2 = new CountDownLatch(1);
    private CountDownLatch c3 = new CountDownLatch(1);
    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {

        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        c2.countDown();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        c2.await();
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        c3.countDown();
    }

    public void third(Runnable printThird) throws InterruptedException {
        c3.await();
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
    }
}

```

运行结果：

```
执行用时 :
12 ms
, 在所有 java 提交中击败了
65.99%
的用户
内存消耗 :
35.6 MB
, 在所有 java 提交中击败了
100.00%
的用户
```

| 提交时间 | 提交结果 | 执行用时 | 内存消耗 | 语言 |
| -------- | -------- | -------- | -------- | ---- |
| 几秒前   | [通过]() | 12 ms    | 35.6 MB  | Java |