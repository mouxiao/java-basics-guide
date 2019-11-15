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
```java
public class Test {
  
  
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
    
    public void test4(){
      System.out.println("4444444444");
    }
  
  }

```
> 执行javap命令查看class文件synchronized信息
> javap -v Test.class

```text
{
  public com.m.x.picture.security.test.Test();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 8: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   Lcom/m/x/picture/security/test/Test;

  public synchronized void test1();
    descriptor: ()V
    flags: ACC_PUBLIC, ACC_SYNCHRONIZED
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #3                  // String 111111111111
         5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 11: 0
        line 12: 8
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       9     0  this   Lcom/m/x/picture/security/test/Test;

  public void test2();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=3, args_size=1
         0: aload_0
         1: dup
         2: astore_1
         3: monitorenter
         4: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         7: ldc           #5                  // String 222222222222
         9: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        12: aload_1
        13: monitorexit
        14: goto          22
        17: astore_2
        18: aload_1
        19: monitorexit
        20: aload_2
        21: athrow
        22: return
      Exception table:
         from    to  target type
             4    14    17   any
            17    20    17   any
      LineNumberTable:
        line 16: 0
        line 17: 4
        line 18: 12
        line 19: 22
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      23     0  this   Lcom/m/x/picture/security/test/Test;
      StackMapTable: number_of_entries = 2
        frame_type = 255 /* full_frame */
          offset_delta = 17
          locals = [ class com/m/x/picture/security/test/Test, class java/lang/Object ]
          stack = [ class java/lang/Throwable ]
        frame_type = 250 /* chop */
          offset_delta = 4

  public static synchronized void test3();
    descriptor: ()V
    flags: ACC_PUBLIC, ACC_STATIC, ACC_SYNCHRONIZED
    Code:
      stack=2, locals=0, args_size=0
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #6                  // String 333333333
         5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 22: 0
        line 23: 8

  public void test4();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #7                  // String 4444444444
         5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 26: 0
        line 27: 8
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       9     0  this   Lcom/m/x/picture/security/test/Test;
}
```

> 根据javap查看class信息可以看出，以synchronized修饰的同步方法，是方法修饰符上依靠ACC_SYNCHRONIZED标识实现的，
而以synchronized修饰的同步代码块则是以monitorenter和monirotexit来实现的。所以：

- 同步方法： 
> synchronized方法会被翻译成普通的方法调用和返回指令如:invokevirtual、areturn指令，
在VM字节码层面并没有任何特别的指令来实现被synchronized修饰的方法，而是在Class文件的方法表中
将该方法的access_flags字段中的synchronized标志位置1，表示该方法是同步方法并使用调用该方法的
对象或该方法所属的Class在JVM的内部对象表示Klass做为锁对象。
- 同步代码块：
> monitorenter指令插入到同步代码块的开始位置，monitorexit指令插入到同步代码块的结束位置，
JVM需要保证每一个monitorenter都有一个monitorexit与之相对应。任何对象都有一个monitor与之相关联，
当且一个monitor被持有之后，他将处于锁定状态。线程执行到monitorenter指令时，将会尝试获取对象所
对应的monitor所有权，即尝试获取对象的锁
    

#### synchronized锁范围

- 普通同步方法，锁是当前实例对象
- 静态同步方法，锁是当前类的class对象
- 同步方法块，锁是括号里面的对象
