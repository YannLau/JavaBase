P444----P459

[toc]

# 异常的概念

1. num1 / num2 => 10 / 0
2. 当执行到 num1 / num2 因为 num2 = 0，程序就会出现（抛出）异常 ArithmeticException
3. 当抛出异常后，程序就退出，崩溃了，下面的代码就不在执行
4. 大家想想这样的程序好吗？不好，不应该出现了一个不算致命的问题，就导致整个系统崩溃
5. java 设计者，提供了一个叫 异常处理机制来解决该问题

如果程序员，认为一段代码可能出现异常/问题，可以使用`try-catch`异常处理机制来解决 , 从而保证程序的健壮性 .

> 方法操作:
>
> 将代码块选中->快捷键 command option T ->选择 try-catch

6. 如果进行异常处理，那么即使出现了异常，程序可以继续执行.

```java
try {
	int res = num1 / num2;
} catch (Exception e) {
	//e.printStackTrace();//打印原始异常信息
	System.out.println("出现异常的原因=" + e.getMessage())；//自定义输出异常信息
}
	System.out.println("程序继续运行…");
```

> 1. 基本概念
>
>    Java语言中，将程序执行中发生的不正常情况称为“异常”。（开发过程中的语法错误和逻辑错误不是异常）.
>
> 2. 执行过程中所发生的异常事件可分为两类
>    1. Error（错误）：Java虚拟机无法解决的严重问题。如：JVM系统内部错误、资源耗尽等严重情况。比如：StackOverflowError [栈溢出] 和 OOM（out of memory），Error 是严重错误 , 程序会崩溃。
>    2. Exception：其它因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。例如空指针访问，试图读取不存在的文件，网络连接中断等等，Exception 分为两大类：运行时异常[程序运行时发生的异常]    和   编译时异常[编程时,编译器检查出的异常]。

# 异常体系图 ! !

在类名上 shift option command U 显示下图

![image-20240207下午34928501](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午34928501.png)

![image-20240207下午34634419](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午34634419.png)

> 异常体系结构图小结
>
> 1. 异常分为两大类，运行时异常 和 编译时异常.
> 2. 运行时异常，编译器不要求强制处置的异常。一股是指编程时的逻辑错误，是程序员应该避免其出现的异常 . java.lang.RuntimeException类及它的子类都是运行时异常
> 3. 对于运行时异常 , 可以不作处理，因为这类异常很普遍，若全处理可能会对程序的可读性和运行效率产生影响
> 4. 编译时异常，是编译器要求必须处置的异常。

# 常见的异常

> 运行时异常

1. NullPointerException空指针异常

   当应用程序试图在需要对象的地方使用 null 时，抛出该异常，看案例演示。

2. ArithmeticException数学运算异常

   当出现异常的运算条件时，抛出此异常。例如，一个整数“除以零”时，抛出此类的一个实例

3. ArraylndexOutOfBoundsException数组下标越界异常

   用非法索引访问数组时抛出的异常。如果索引为负或大于等于数组大小，则该索引为非法索引

4. ClassCastException类型转换异常

   当试图将对象强制转换为不是实例的子类时，抛出该异常。例如，以下代码将生成一个 ClassCastException

5. NumberFormatException数字格式不正确异常

   当应用程序试图将字符串转换成一种数值类型，但该字符串不能转换为适当格式时，抛出该异常=>使用异常我们可以确保输入是满足条件数字.

> 编译异常 : 编译异常是指在编译期间，就必须处理的异常，否则代码不能通过编译。

> 常见的编译异常
> SQLException //操作数据库时，查询表可能发生异常
> IOException //操作文件时，发生的异常
> FileNotFoundException //当操作一个不存在的文件时，发生异常
> ClassNotFoundException //加载类，而该类不存在时，异常
> EOFException // 操作文件，到文件末尾，发生异常
> IllegalArguementException //参数异常

# 异常处理 ! !

异常处理就是当异常发生时,对异常处理的方式.

1. try-catch-finally
   程序员在代码中捕获发生的异常，自行处理
2) throws
将发生的异常抛出，交给调用者（方法）来处理，最顶级的处理者就是JVM

> try-catch-finally 处理机制示意图
>
> ```java
> try {
> 代码/可能有异常
> }catch(Exception e) {
> //捕获到异常
> //1.当异常发生时
> //2.系统将异常封装成Exception 对象e，传递给catch
> //3.得到异常后,程序员自己处理
> //4.注意,如果没有发生异常,catch 代码块不执行!
> }finally{
>  //不管try代码块是否有异常发生，始终要执行finally 
>  //通常将释放资源的代码,放在 finally 中执行.
> }
> ```

> throws 处理机制示意图
>
> 1. try-catch-finally 和 throws 二选一
> 2. 如果程序员，没有显示是处理异常，默认throws (to JVM)
>
> ![image-20240207下午45010072](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午45010072.png)

# 异常处理 之 try-catch

1. Java提供try和catch块来处理异常。try块用于包含可能出错的代码。catch块用于处理try块中发生的异常。可以根据需要在程序中有多个try..catch块。
2. 基本语法
   `try {`
     `//可疑代码`
     `//将异常生成对应的异常对象，传递给catch块`
   `｝catch（异常）｛`
     `//对异常的处理`
   `｝` //如果没有 finally , 语法也可以通过

> try-catch 方式处理异常--注意事项
>
> 1） 如果异常发生了，则异常发生后面的代码不会执行，直接进入到catch块.  类似于 break;
> 2）如果异常没有发生，则顺序执行try的代码块，不会进入到catch.
> 3） 如果希望不管是否发生异常，都执行某段代码（比如关闭连接，释放资源等）则使用如下代码-  finally｛｝
>
> 4） 可以有多个catch语句，捕获不同的异常（进行不同的业务处理），要求父类异常在后，子类异常在前，比如（Exception 在后，NullPointerException 在前），如果发生异常，只会匹配一个catch
>
> ```java
> try {
> 	Person person = new Person();
> 	person = null;
> 								System.out.println(person.getName());//NullPoirterExcepton
> 	int n1 = 10;
> 	int n2 = 0;
> 	int res = n1 / n2;//ArithmeticException
>   
> } catch(NullPointerException e){
>   
> 	System.out.println("空指针异常=" + e.getMessage());
> 
> } catch(AtithmeticException e){
> 	
>   System.out.println("算数异常=" + e.getMessage());
> 
> } catch(Exception e) {  //父类异常
>   	System.out.println(e.getMessage());
> } finally {
>   
>   //释放资源等操作...
> 
> }
> ```
>
> 5） 可以进行 `try-finally` 配合使用，这种用法相当于没有捕获异常，因此程序会直接崩掉。应用场景 : 就是执行一段代码，不管是否发生异常，都必须执行某个业务逻辑 !  finally语块 执行完成后 , 程序该崩就崩掉了 , 没崩就接着走 !
>
> finally 有多厉害 ? 就算前面的 语句中有 return XXX语句 , 也是先把 XXX 执行了, 然后先不 return 都要等着执行 finally 的句块后才能返回. 如果finally 中也有 return . 那就以 finally 中的为准 ! ! 
>
> ```java
> public static int test(){
>         int i = 1;
>         try {
>             String w = null;
>             w.charAt(2);
>         } catch (NullPointerException e) {
>             System.out.println("此时在 NullPointerException 的 catch 中 i = " + i);
>             return ++i;
>         } finally {
>             System.out.println("此时在 finally 中 i = " + i);
>             return ++i;
>         }
>     }
> ```
>
> 调用以上方法,会返回 3 !
>
> ```java
>  public static int test() {
>         int i = 1;
>         try {
>             String w = null;
>             w.charAt(2);
>         } catch (NullPointerException e) {
>             System.out.println("此时在 NullPointerException 的 catch 中 i = " + i);
>             return ++i;
>         } finally {
>             System.out.println("此时在 finally 中 i = " + i);
>             ++i;
>             System.out.println("最终的 i 应该是 i =" + i);
>         }
>         return 100;
>     }
> ```
>
> 调用以上方法,会返回 2 ! 说明 finally 中的语块对之前的 return 处的值不造成影响了 !!!!  因为在 return语句处 , return 动作不能马上执行, 而 finally 中有没有 return语句 , i=2 是被临时变量 temp 保存的.

> try-catch-finally 执行顺序小结
>
> 1）如果没有出现异常，则执行 try 块中所有语句，不执行catch 块中语句，如果有 finally ，最后还需要执行finally里面的语句
> 2） 如果出现异常，则try块中异常发生后，try块 剩下的语句不再执行。将执行catch块中的语句，如果有finally，最后还需要执行finally里面的语句！

# throws 异常处理

> 基本介绍
>
> 1） 如果一个方法（中的语句执行时）可能生成某种异常，但是并不能确定如何处理这种异常，则此方法应显示地声明抛出异常，表明该方法将不对这些异常进行处理，而<u>***由该方法的调用者负责处理***</u>。
>
> 2） 在方法声明中用throws语句可以声明抛出异常的列表，throws后面的异常类型可以是方法中产生的异常类型，也可以是它的父类异常类型。

> 案例介绍
>
> 1. 这里的异常是一个FileNotFoundException 编译异常
>
> 2. 使用前面讲过的 try-catch-finally
> 3. 使用throws，抛出异常，让调用f2方法的调用者（方法）处理
> 4. throws 后面的异常类型可以是方法中产生的异常类型，也可以是它的父类异常如 Exception
> 5. throws 关键字后也可以是 异常列表，即可以抛出多个异常

> throws 注意事项和使用细节
>
> 1） 对于编译异常，程序中必须处理，比如 try-catch 或者 throws
> 2） 对于运行时异常，程序中如果没有处理，默认就是throws的方式处理
> 3） 子类重写父类的方法时，对抛出异常的规定：子类重写的方法，所抛出的异常类型要么和父类抛出的异常一致，要么为父类抛出的异常的类型的子类型
> 4） 在throws 过程中，如果有方法 try-catch，就相当于处理异常，就可以不必throws

一定要区分好 运行时异常 和 编译异常 !

```java
public static void f4() {
//1. 在f4()中调用方法f5()是OK的
//2. 原因是f5()抛出的是运行异常
//3.而java中，并不要求程序员显示处理，因为有默认处理机制
	f5();
}
public static void f5() throws ArithmeticException {
｝
```

# 自定义异常

> 自定义异常的步骤
>
> 1）定义类：自定义异常类名（程序员自己写）继承 Exception 或RuntimeException
> 2）如果继承 Exception，属于编译异常
> 3）如果继承RuntimeException，属于运行异常（一般来说，继承RuntimeException ）

```java
public class CustomException {
	public static void main (String[] args) {
		int age = 180;
		//要求范围在 18 - 120 之间，否则抛出一个自定义异常
		if(!(age >= 18 && age <= 120)){
			//这里我们可以通过构造器，设置信息
			throw new AgeException("年龄需要在 18~120之间");
		}
		System.out.println("你的年龄范围正确。");
  }
}
//自定义一个异常类
class AgeException extends RuntimeException {
	public AgeException（String message）{//构造器
		super(message);
}
  
  //1.一般情况下，我们自定义异常是继承 RuntimeException
  //2. 即把自定义异常做成 运行时异常，好处时，我们可以使用默认的处理机制
```

# throw 和 throws 的对比

一览表

|        |           意义           |    位置    | 后面跟的东西 |
| :----: | :----------------------: | :--------: | :----------: |
| throws |    异常处理的一种方式    | 声明方法出 |   异常类型   |
| throw  | 手动生成异常对象的关键字 |  方法体中  |   异常对象   |



