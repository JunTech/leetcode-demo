# leetcode[1116. 打印零与奇偶数](https://leetcode-cn.com/problems/print-zero-even-odd/)

`类型：多线程`

## 1.问题描述

```
假设有这么一个类：

class ZeroEvenOdd {
  public ZeroEvenOdd(int n) { ... }      // 构造函数
  public void zero(printNumber) { ... }  // 仅打印出 0
  public void even(printNumber) { ... }  // 仅打印出 偶数
  public void odd(printNumber) { ... }   // 仅打印出 奇数
}
相同的一个 ZeroEvenOdd 类实例将会传递给三个不同的线程：

线程 A 将调用 zero()，它只输出 0 。
线程 B 将调用 even()，它只输出偶数。
线程 C 将调用 odd()，它只输出奇数。
每个线程都有一个 printNumber 方法来输出一个整数。请修改给出的代码以输出整数序列 010203040506... ，其中序列的长度必须为 2n。

 

示例 1：

输入：n = 2
输出："0102"
说明：三条线程异步执行，其中一个调用 zero()，另一个线程调用 even()，最后一个线程调用odd()。正确的输出为 "0102"。
示例 2：

输入：n = 5
输出："0102030405"

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/print-zero-even-odd
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

## 2.解题思路

### 2.1使用lock+conditiom

```
class ZeroEvenOdd {
    private int n;
    private volatile int flag = 0;
    private Lock lock = new ReentrantLock();
    Condition condition0 = lock.newCondition();
    Condition condition1 = lock.newCondition();
    Condition condition2 = lock.newCondition();


    public ZeroEvenOdd(int n) {
        this.n = n;
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void zero(IntConsumer printNumber) throws InterruptedException {

        try {
            lock.lock();
            for (int i = 1; i < n; i++) {
                while (flag !=0){
                    condition0.await();
                }
                printNumber.accept(0);
                if((i & 1)==0){
                    flag =2;
                    condition1.signalAll();
                }else {
                    flag = 1;
                    condition2.signalAll();
                }
            }
        }finally {
            lock.unlock();
        }
    }

    public void even(IntConsumer printNumber) throws InterruptedException {
        try {
            lock.lock();
            for (int i = 2; i <= n; i += 2) {
                while (flag != 2) {
                    condition1.await();
                }
                printNumber.accept(i);
                flag = 0;
                condition0.signalAll();
            }
        } finally {
            lock.unlock();
        }
        

    }

    public void odd(IntConsumer printNumber) throws InterruptedException {
        try {
            lock.lock();
            for (int i = 1; i <= n; i += 2) {
                while (flag != 1) {
                    condition2.await();
                }
                printNumber.accept(i);
                flag = 0;
                condition0.signalAll();
            }
        } finally {
            lock.unlock();
        }
    }
}
```

这种方法会超出时间限制

### 2.2 只增加一个参数,使用wait(),notifyAll()实现

```
class ZeroEvenOdd {
	private int n;
	private int num = 0;

	public ZeroEvenOdd(int n) {
		this.n = n;
	}

	// printNumber.accept(x) outputs "x", where x is an integer.
	public void zero(IntConsumer printNumber) throws InterruptedException {
		for (int i = 0; i < n; i++) {
			synchronized (this) {
				while (num % 2 != 0) {
					this.wait();
				}
				printNumber.accept(0);
				num++;
				this.notifyAll();
			}
		}

	}

	public void even(IntConsumer printNumber) throws InterruptedException {
		for (int j = 2; j <= n; j = j + 2) {
			synchronized (this) {
				while (num % 2 == 0 || num % 4 != 3) {
					this.wait();
				}
				printNumber.accept(j);
				num++;
				this.notifyAll();
			}
		}

	}

	public void odd(IntConsumer printNumber) throws InterruptedException {
		for (int j = 1; j <= n; j = j + 2) {
			synchronized (this) {
				while (num % 2 == 0 || num % 4 != 1) {
					this.wait();
				}
				printNumber.accept(j);
				num++;
				this.notifyAll();
			}
		}
	}
}

```

### 2.3 使用信号量

```
mzero=1: 控制0线程的信号量;
modd=0: 控制奇数线程的信号量;
meven=0: 控制偶数线程的信号量;

zero(){
    for(i in [0,...,N-1]){
        wait(mzero);
        print(0);
        if(i & 1) post(meven);
        else post(modd);
    }
}

even(){
    for(i in[2,4,...,n]){
        wait(meven);
        print(i);
        post(mzero);
    }
}

odd(){
    for(i in[1,3,...,n]){
        wait(modd);
        print(i);
        post(mzero);
    }
}


```

### 2.4使用CountDownLatch实现

```
class ZeroEvenOdd {
    private int n;
    
    public ZeroEvenOdd(int n) {
        this.n = n;
    }

    Semaphore z = new Semaphore(1);
    Semaphore e = new Semaphore(0);
    Semaphore o = new Semaphore(0);
	
    public void zero(IntConsumer printNumber) throws InterruptedException {
        for(int i=0; i<n;i++) {
        	z.acquire();
        	printNumber.accept(0);
        	if((i&1)==0) {
        		o.release();
        	}else {
        		e.release();
        	}
        }
    }

    public void even(IntConsumer printNumber) throws InterruptedException {
        for(int i=2; i<=n; i+=2) {
        	e.acquire();
        	printNumber.accept(i);
        	z.release();
        }
    }

    public void odd(IntConsumer printNumber) throws InterruptedException {
        for(int i=1; i<=n; i+=2) {
        	o.acquire();
        	printNumber.accept(i);
        	z.release();
        }
    }
}


```

