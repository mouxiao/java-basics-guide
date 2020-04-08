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
....TODO
###### <span id="stackOverflowError">StackOverflowError</span>
....TODO
