[toc]

# 区分 RuntimeException  和  非RuntimeException(受查异常)

## 运时异常/非检查异常（unckecked exception）

◼ ==Error 和 RuntimeException 及其子类==

◼ Java编译器不检查，当程序中可能出现这类异常时，即使没有用 try...catch语句捕获它，也没有用throws字句声明抛出它，还是会编译通过。 

◼ 原因通常是由于执行了错误的操作。一旦出现错误，建议让程序终止。修正代码，而不是异常处理器处理 

◼ 除0错误ArithmeticException 

◼ 错误的强制类型转换错误ClassCastException 

◼ 数组索引越界ArrayIndexOutOfBoundsException 

◼ 使用了空对象NullPointerException

## 受检查异常（checked exception）

除了Error 和 RuntimeException外，其他异常都属于受检查 异常，一般是由程序的运行环境导致 

◼SQLException 

◼IOException, 

◼ClassNotFoundException 

◼必须用try...catch捕获处理，或者用throws语句声明抛出， 否则编译不会通过。 

◼对于受检查异常可以由程序处理，如果抛出异常的方法本身不处理或者不能处理它，那么方法的调用者就必须去处理该异常 ，否则调用会出错，编译也无法通过。

> 说明：两种异常都可通过程序来捕获处理。但对于运行异常不建议 用try...catch...捕获处理，应通过调试尽量避免。

# 异常对象两个重要的方法

exception.getMessage() 获取异常简单的描述信息

exception.printStackTrace() 打印异常追踪的堆栈信息

# 异常处理 

◼ 系统自动处理 

◼ 使用try~catch~finally语句 

◼ 使用throw语句直接抛出异常（抛出异常） 或使用throws语句间接抛出异常（声明异常 ）。 

​	 finally为了完成执行的代码而设计的，主要是为了程序的健壮性和完整性，无论有没有 异常发生都执行代码（调用System.exit(0) 退出JVM除外）。

​	 finally块通常用来做资源释放操作：关闭文件，关闭数据库连接等等。良好的编程习惯 ：在try块中打开资源，在finally块中清理释放这些资源。

## 语法结构

```java
异常冒泡与链化
Java终结式异常处理模式（ termination model of exception handling ）：执行流恢复到处理了异常的catch块后接着执行

try~catch~finally语句
try{
  可能产生异常的代码段； 
  }catch（异常类名1 对象名1）{
  处理语句组1；
}catch（异常类名2 对象名2）{
  处理语句组2； 
}
……
  finally
  {
    最终处理语句；
  }

Java8：
catch(ArithmeticException | FileNotFoundException | NullpointerException e) { … }

```

## throws语句

如果一个方法可能导致异常但不处理它，此时 可在方法声明中包含 throws 子句，发生了异常，由调用者处理。 throws列举方法可能引发的所有异常。 例： 

`private static void throwsTest(参数表) throws 逗 号间隔的异常表 { 方法体 }`

```java
public class ThrowDemo{
  public static void main(String[] args){
    try{
      double d = 100 / 0.0;
      System.out.println(“浮点数除零: ”+d);
      if(String.valueOf(d).equals(“Infinity”)) 
        throw new ArithmeticException(“除零异常”);
    }
    catch(Arithmetic e){
      System.out.println(e);
    }
  }
}
//double类型的除法的时候，如果除数是0的时候不会抛出异常的，而是会返回Infinity，不被认为是错误。

```

### Throws抛出异常的规则

1. 对于Error、RuntimeException及其子类，可以不使用throws关键字来声明要抛出的异常，编译仍能顺利通过，但在运行时会被系统抛出。
2. 如果一个方法可能出现可查异常，必须用try-catch语句捕获，或用throws子句声明抛出，否则编译错误。
3. 该方法的调用者必须处理被抛出的异常或者重新抛出 （当方法的调用者不处理该异常时，应继续抛出）
4. 调用方法必须遵循任何可查异常的处理和声明规则。 方法覆盖时，不能声明与覆盖方法不同的异常，声明的任何异常必须是被覆盖方法所声明异常的同类或子类。

## try,catch,finally的各种组合用法

◼ try+catch  运行try块，有异常抛出则转到catch块去处理，然后执 行catch块后面的语句 

◼ try+catch+finally  运行try块，有异常抛出则转到catch块执行完毕后，执 行finally块的代码，再执行finally块后面的代码。 如果没有异常抛出，执行完try块后，转去执行finally 块的代码。然后执行finally块后面的语句 

◼ try+finally  运行try块，如果有异常抛出，转向执行finally块的代 码，而finally块后面的代码不会被执行！（因为没有处 理异常，所以遇到异常后，执行完finally后，方法就已 抛出异常的方式退出）。 

注意：这种方式由于没有捕获异常，所以要在方法后面 声明抛出异常。 try,catch,finally的各种组合用法

# 创建自己的异常

◼ 内置异常不可能始终足以捕获所有错误， 因此需要用户自定义的异常类 

◼ ==用户自定义的异常类应为 Exception 类（或 者Exception 类的子类）的子类==

◼ ==创建的任何用户自定义的异常类都可以获得Throwable类定义的方法==

# Java7： try-with-resources

◼ Java中如InputStream、OutputStream、 java.sql.Connection等资源需要手动关闭。 

◼ Java7之前通常使用try-finally方式关闭资源。Java7推出了try-with-resources声明来替代之前的方式。 

 try-with-resources 声明要求其中定义的变量实现 AutoCloseable 接口，这样系统可以自动调用它们的close方法，从而替代了 finally 中关闭资源的功能。 

`public interface AutoCloseable { void close() throws Exception; }`

```java
InputStream in;
OutputStream out;
try {
  out = new FileOutputStream(dst);
  byte[] buff = new byte[1024];
  int n;
  while ((n = in.read(buff)) >= 0) {
    out.write(buff, 0, n);
  }
} catch (IOException e) {
  e.printStackTrace();
} finally {
  in.close();
  out.close();
}

try (InputStream in = new FileInputStream(src);
     OutputStream out = new FileOutputStream(dst)) {
  byte[] buff = new byte[1024];
  int n;
  while ((n = in.read(buff)) >= 0) {
    out.write(buff, 0, n);
  }
} catch (IOException e) {
  e.printStackTrace();
}


public class Exception2 {
  public static void main(String[] args) throws ArithmeticException{
    System.out.println("beginning...");
    try{
      method1();
    } finally {
      System.out.println("finally…");
    }
    System.out.println("last method in main....");
  }
  public static void method1() throws ArithmeticException{
    int a = 10;
    int b = 0;
    System.out.println("in method 1...");
    System.out.println(a/b);
    System.out.println("...");
    System.out.println("…");
    System.out.println(“last line in method1...");
                       }
                       }
                       }
```
