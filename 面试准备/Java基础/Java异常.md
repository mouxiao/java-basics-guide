#### error 和 exception 区别是什么？
> 首先error和exception都继承 Throwable类 ，但是exception又分为运行时异常(RuntimeException)和非运行时异常。

##### Exception
- 运行时异常(RuntimeException):程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避
免这类异常的发生；比如下面这些都属于RuntimeException:
    - NullPointerException
    - ArrayIndexOutOfBoundsException
    - ArithmeticException(算术异常)
    - ClassNotFoundException
    - MissingResourceException(丢失资源)
- 非运行时异常:RuntimeException之外的异常我们统称为非运行时异常，类型上属于Exception类及其子类，从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过。一般需要通过try catch或者throws抛出处理。比如下面这些都属于非运行时异常：
    - IOException
    - SqlException

##### Error 
- error 一般不能称之为异常，一般error都脱离了程序的掌控，比如堆栈error，下面这些一般都输入error：
    - [OutOfMemoryError](#outOfMemoryError)
    - [StackOverflowError](#stackOverflowError)
    - UnknownError
    - ZipError
    - InternalError

#### 常见Exception和Error介绍说明

#####Error 

###### <span id="outOfMemoryError">OutOfMemoryError</span>
- 什么情况下会出现OutOfMemoryError？
    - 在[Java虚拟机栈]()这个内存区域中，如果Java虚拟机栈容量可以动态扩展，当栈扩展时无法申请到足够的内存会抛出OutOfMemoryError异常
    - [本地方法栈]()在栈扩展失败时，也会抛出OutOfMemoryError
    - 当[Java堆]()中存放的对象实例超过JVM设置的最大值时，会出现OutOfMemoryError,具体见[Java heap space](#javaHeapSpace)
    - 当[方法区]()(存储已被虚拟机加载的类型信息、常量、静态变量、即时编译器编译后的代码缓存等数据)无法满足新的内存分配需求时，将抛出OutOfMemoryError异常。
    - 当[常量池]()无法再申请到内存时会抛出OutOfMemoryError异常。
  
  > 一般OutOfMemoryError分为三类：PermGen space, Java heap space, unable to create new native thread。
- PermGen space
    - 一般出现这种问题是程序中使用了大量的jar或者class，使java虚拟机装载类的空间不够，解决这类问题一般有以下两种办法：
        - 增加java虚拟机中的xx:PermSize(持久代内存最小值)和XX:MaxPermSize(持久代内存最大值)参数的大小
        - 清理应用程序中多余的jar包，或者多个项目jar包共享
- <span id="javaHeapSpace"></span>Java heap space
    ```java
    public class HeapOOM {
    
        public static void main(String[] args) {
            List<HeapOOM> heaps = new ArrayList<>();
            while (true){
                heaps.add(new HeapOOM());
            }
        }
    }
    ``` 
    一般像这种如果不是[内存泄露]()的话，可以通过修改虚拟机中的堆内存大小(-Xmx与-Xms)解决，比如：
    - -Xmx3550m:设置JVM最大堆内存为3550M。
    - -Xms3550m:设置JVM初始堆内存为3550M，此值可以设置与-Xmx相同，可以避免每次垃圾回收完成后JVM重新分配内存。
- unable to create new native thread。               
    - 出现这种情况一般是java程序创建的线程数量超过了系统正常运行所运行的线程数量(MaxProcessMemory-JVMMemory-ReservedOsMemory)，解决思路一般可以从下面几点考虑：
        - 合理创建使用线程，对线程做到有效的资源回收。
        - 修改MaxProcessMemory(使用64为操作系统)，JVMMemory(减少JVMMemory的分配)，ReservedOsMemory(减少单个线程栈的大小)参数                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                   

###### <span id="stackOverflowError">StackOverflowError</span>
- 什么情况下会出现StackOverflowError？
    - 在[Java虚拟机栈]()这个内存区域中，如果线程请求的栈深度大于虚拟机所允许的深度，将抛出StackOverflowError异常
    - [本地方法栈]()在栈深度溢出时，也会抛出StackOverflowError



