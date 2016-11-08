Lab4:Deadlock
====

* 代码运行结果展示
* 死锁产生的必要条件
* 代码分析

## 代码运行结果展示
  * 第一次运行的结果
  
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForDeadlock/count=11111.png)
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForDeadlock/死锁count=11111.png)
  * 其他时间再次运行的结果
  
    ![connection](https://raw.githubusercontent.com/LyingChild/ES2016_14353423/master/Image/ForDeadlock/不同时间的结果.JPG)

## 死锁产生的必要条件
  * 互斥条件：一个资源实例只能同时被一个进程使用。
  * 请求和保持条件：进程请求已被其他进程占用的资源时会被阻塞，但此时该进程不会释放自己已经获得的资源。
  * 不可剥夺条件：进程已获得的资源，在未完成使用之前不可被剥夺，只能在使用完之后自己释放。
  * 循环等待条件：进程发生死锁，则必然存在一个进程--资源之间的环形请求链，即若干进程形成一种头尾相接的进程等待另一个进程占有资源的等待环路。

## 代码分析
  * Class A && Class B
```Java
class A {
	synchronized void methodA(B b) {
		b.last();
	}
	synchronized void last() {
		System.out.println("Inside A.last()");
	}
}
class B {
	synchronized void methodB(A a) {
		a.last();
	}
	synchronized void last() {
		System.out.println("Inside B.last()");
	}
}
```

        * 可以知道，两个Class中的方法都用`synchronized`修饰,其主要用于进程的同步：
          * 阻塞当前被修饰的代码: 如果被修饰的代码块正在被一个线程使用的同时另一线程也要调用，则后调用的线程被阻塞。
          * 阻塞其他被synchronized修饰的块或方法：object中的一个synchronized修饰的代码块或方法被访问时，object中其他synchronized修饰的块或方法的访问被阻塞。
* Deadlock
```Java
public class Deadlock implements Runnable{
	A a = new A();
	B b = new B();
	
	Deadlock() {
		Thread t = new Thread(this);
		int count = 11111;
		t.start();
		while(count-->0);
		a.methodA(b);
	}
	
	@Override
	public void run() {
		b.methodB(a);
	}

	public static void main(String args[]) {
		new Deadlock();
	}
}
```
         *  t执行t.start()的时候，会创建新的线程并在新的线程t中执行run()方法。
         *  Deadlock中使用while(count-->0)循环来做短暂的延迟，目的是使a.methodA(b)和t线程的b.methodB(a)同时被执行。
  * 死锁产生的原因
    * 程序从main()进入，然后new了一个Deadlock对象，Deadlock对象初始化过程中定义了子线程t，然后start了t线程
    * t线程成功创建并执行run()方法，调用了`b.methodB(a);`,t线程创建过程会有少量的时间开销t1
    * 主线程执行完t.start()后就继续向下，进入while循环空转了一段时间(t2)后执行`a.methodA(b);`
    * 当t1和t2的时间极为相近时，可能出现：a拿到了methodA(),阻塞了自身的a.last(),请求b.last(), b拿到了methodB(),阻塞了自身的b.last(),请求a.last(), 也即a请求b占有的资源，同时b也请求a占有的资源，所以形成了死锁。
