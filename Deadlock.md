Lab4:Deadlock
====

* ## 代码运行结果展示
* ## 死锁产生的必要条件
* ## 代码分析

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
```
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
          可以知道，两个Class中的方法都有修饰词synchronized