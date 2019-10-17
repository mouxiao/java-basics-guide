# Synchronized

## Synchronized ?
> 首先我们在使用之前,需要了解synchronized是个什么意思,以及在什么时候使用! synchronized 是java语言中的一个关键字,
翻译过来就是同步的意思,下面就具体看看synchronized的使用和原理。

##### 为什么要用synchronized?
> 在并发编程中存在线程安全问题，主要原因有：1.存在共享数据 2.多线程共同操作共享数据。关键字synchronized可以保证
在同一时刻，只有一个线程可以执行某个方法或某个代码块，同时synchronized可以保证一个线程的变化可见（可见性），
即可以代替volatile。synchronized使用的前置条件是java中每个对象都可以作为锁。

##### synchronized原理
> synchronized可以保证方法或者代码块在运行时，同一时刻只有一个方法可以进入到临界区，同时它还可以保证共享变量的内存可见性

##### 怎么使用synchronized ?
> 在java中使用synchronized有三种方式 
````
  public class SyncTest {
  
  
    public synchronized void test1(){
      System.out.println("111111111111");
    }
  
  
    public void test2(){
      synchronized (this){
        System.out.println("222222222222");
      }
    }
  
    public static synchronized void test3(){
      System.out.println("333333333");
    }
  
  }
  ````
- 普通同步方法，锁是当前实例对象
- 静态同步方法，锁是当前类的class对象
- 同步方法块，锁是括号里面的对象
