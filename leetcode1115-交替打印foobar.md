# leetcode1115-交替打印foobar

## 1.问题描述

```
我们提供一个类：

class FooBar {
  public void foo() {
    for (int i = 0; i < n; i++) {
      print("foo");
    }
  }

  public void bar() {
    for (int i = 0; i < n; i++) {
      print("bar");
    }
  }
}
两个不同的线程将会共用一个 FooBar 实例。其中一个线程将会调用 foo() 方法，另一个线程将会调用 bar() 方法。

请设计修改程序，以确保 "foobar" 被输出 n 次。

 

示例 1:

输入: n = 1
输出: "foobar"
解释: 这里有两个线程被异步启动。其中一个调用 foo() 方法, 另一个调用 bar() 方法，"foobar" 将被输出一次。
示例 2:

输入: n = 2
输出: "foobarfoobar"
解释: "foobar" 将被输出两次。

```

## 2.解题思路

可以查看本博客写的一篇名为：多线程交替顺序执行 的文章，传送门: [多线程交替顺序执行](https://juntech.top/posts/39dc4822.html)

1、condition和可重入锁配合，使用一个锁来控制一次只执行一个线程。
2、需要保证线程先输出foo方法，so,需要增加一个变量来控制执行顺序
3、根据两个线程就使用一个锁创建两个condition条件，根据fooRun来判断执行哪一个方法，并且通过condition来对当前线程等待还是继续执行。
4、用一个变量fooRun来控制当前应该执行的是哪一个方法，如果当前线程和fooRun想要执行的线程不一致，就把当前线程等待，并且释放当前的锁。

## 3.代码

```
class FooBar {
    private int n;
    private Lock lock = new ReentrantLock();
    private int flag = 1;  //初始运行开始标志
    private Condition condition1 = lock.newCondition();
    private Condition condition2 = lock.newCondition();
    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
       try {
           lock.lock();

           for (int i = 0; i < n; i++) {
               if(flag!=1){
                   condition1.await();
                   condition2.signal();
               }
               // printFoo.run() outputs "foo". Do not change or remove this line.
               printFoo.run();
               flag =2;
               condition2.signal();
           }
           
       }catch (Exception e) {
            throw  new RuntimeException("运行超时了...",e);
       }finally{
           lock.unlock();
       }

    }

    public void bar(Runnable printBar) throws InterruptedException {

        try {

            lock.lock();

            for (int i = 0; i < n; i++) {
                if(flag!=2){
                    condition2.await();
                    condition1.signal();
                }
                // printBar.run() outputs "bar". Do not change or remove this line.
                printBar.run();
                flag =1;
                condition1.signal();
            }
            
        }catch (Exception e) {
            throw  new RuntimeException("运行超时了...",e);
        }finally{
            lock.unlock();
        }
    }
}
```

`执行结果：`

通过

显示详情

执行用时 :21 ms, 在所有 java 提交中击败了97.21%的用户

内存消耗 :37.2 MB, 在所有 java 提交中击败了100.00%的用户

具体情况：

| 提交时间 | 提交结果 | 执行用时 | 内存消耗 | 语言 |
| -------- | -------- | -------- | -------- | ---- |
| 几秒前   | [通过]() | 21 ms    | 37.2 MB  | Java |