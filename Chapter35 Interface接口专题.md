[toc]

# 接口介绍

> 引入案例
>
> usb插槽就是现实中的接口。你可以把手机，相机，u盘都插在usb插槽上，而不用担心那个插槽是专门插哪个的，原因是做
> usb插槽的厂家和做各种设备的厂家都遵守了统一的规定包括尺寸，排线等等。
>
> ```java
> package com.hspedu.interface_;
> public interface UsbInterface { //接口
>   //规定接口的相关方法,老师规定的.即规范... public void start();
>   public void stop();
> }
> package com.hspedu.interface_;
> public class Camera implements UsbInterface{//实现接口,就是把接口方法实现
>   @Override
>   public void start() {
>     System.out.println("相机开始工作...");
>   }
>   @Override
>   public void stop() {
>     System.out.println("相机停止工作....");
>   }
> }
> package com.hspedu.interface_;
> //Phone 类 实现 UsbInterface
> //解读 1. 即 Phone 类需要实现 UsbInterface 接口 规定/声明的方法
> public class Phone implements UsbInterface {
>   @Override
>   public void start() {
>     System.out.println("手机开始工作...");
>   }
>   @Override
>   public void stop() {
>     System.out.println("手机停止工作.....");
>   }
> }
> package com.hspedu.interface_;
> public class Interface01 {
>   public static void main(String[] args) {
>     //创建手机，相机对象
>     //Camera 实现了 UsbInterface
>     Camera camera = new Camera();
>     //Phone 实现了 UsbInterface
>     Phone phone = new Phone();
>     //创建计算机
>     Computer computer = new Computer();
>     computer.work(phone);//把手机接入到计算机
>     System.out.println("===============");
>     computer.work(camera);//把相机接入到计算机
>   }
> }
> ```
>

> 接口就是给出一些没有实现的方法，封装到一起，到某个类要使用的时候，在根据具体情况把这些方法写出来。语法：
>
> `interface 接口名{`
>
> ​	`//属性`
>
> ​	`//方法`
>
> `}`
>
> `class 类名 implements 接口1,接口2,接口3{`
>
> ​	`//自己属性`
>
> ​	`//自己方法`
>
> ​	`//必须实现的接口抽象方法`
>
> `}`
>
> 小结：
>
> 1. 在Jdk7.0前 接口里的所有方法都没有方法体。即都是抽象的方法（一般不写 abstract 关键字）。在接口中，抽象方法，可以省略 abstract 关键字。
> 2. <u>***Jdk8.0后接口类可以有静态方法，默认方法（default 修饰——default 放在访问修饰符前面），也就是说接口中可以有方法的具体实现。***</u>

> 接口的深入讨论
>
> 对初学者讲.理解接口的概念不算太难，难的是不知道什么时候使用接口，下面我例举几个应用场景：
>
> 1. 说现在要制造战斗机，武装真升机.专家只需把飞机需要的功能/规格定下来即可，然后让别的人具体实现就可。
>
> 2. 说现在有一个项目经理.管理三个程序员，功能开发一个软件，为了控制和管理软件，项目经理可以定义一些接口，然后由程序员具体实现。
>
>   实际要求：3个程序员，编写三个类，分别完成对Mysql,Oracle,DB2数据库的连接。
>
> ```java
> package com.hspedu.interface_;
> public interface DBInterface { //项目经理
>   public void connect();//连接方法
>   public void close();//关闭连接
> }
> package com.hspedu.interface_;
> //A 程序
> public class MysqlDB implements DBInterface {
>   @Override
>   public void connect() {
>     System.out.println("连接 mysql");
>   }
>   @Override
>   public void close() {
>     System.out.println("关闭 mysql");
>   }
> }
> package com.hspedu.interface_;
> //B 程序员连接 Oracle
> public class OracleDB implements DBInterface{
>   @Override
>   public void connect() {
>     System.out.println("连接 oracle");
>   }
>   @Override
>   public void close() {
>     System.out.println("关闭 oracle");
>   }
> }
> package com.hspedu.interface_;
> public class Interface03 {
>   public static void main(String[] args) {
>     MysqlDB mysqlDB = new MysqlDB();
>     t(mysqlDB);
>     OracleDB oracleDB = new OracleDB();
>     t(oracleDB);
>   }
>   public static void t(DBInterface db) {
>     db.connect();
>     db.close();
>   }
> }
> ```

# 接口的注意事项和细节

> 1）接口不能被实例化
> 2）接口中所有的方法是 public方法，==不写访问修饰符也是默认为 public==，接口中抽象方法，可以不用abstract 修饰 `void aaa();  等价于    public abstract void aaa();`不用加花括号！
> 3） 一个普通类实现接口，就必须将该接口的所有方法都实现。
> 4） 抽象类实现接口，可以不用实现接口的方法。
>
> ```java
> package com.hspedu.interface_;
> 
> public class InterfaceDetail01 {
>   public static void main(String[] args) {
>     //new IA()
>   }
> }
> //1.接口不能被实例化
> //2.接口中所有的方法是 public 方法, 接口中抽象方法，可以不用 abstract 修饰
> //3.一个普通类实现接口,就必须将该接口的所有方法都实现,可以使用 alt+enter 来解决
> //4.抽象类去实现接口时，可以先不实现接口的抽象方法
> interface IA {
>   void say();//修饰符 public protected 默认 private
>   void hi();
> }
> class Cat implements IA{
>   @Override
>   public void say() {
>   }
>   @Override
>   public void hi() {
>   }
> }
> abstract class Tiger implements IA {
> }
> ```
>
> 5） 一个类同时可以实现多个接口
> 6） 接口中的属性，只能是final的，而且默认就是 public static final 修饰符。比如：int a=1；实际上是 `public static final` int a=1;（必须初始化）
> 7） 接口中属性的访问形式：`接口名.属性名`
> 8） 一个接口不能继承其它的类，但是可以继承多个别的接口 `interface A extends B,C {}`
>
> 9)  接口的修饰符 只能是 public 和 默认，这点和类的修饰符是一样的。
>
> ```java
> package com.hspedu.interface_;
> public class InterfaceDetail02 {
>   public static void main(String[] args) {
>     //老韩证明 接口中的属性,是 public static final
>     System.out.println(IB.n1);//说明 n1 就是 static
>     //IB.n1 = 30; 说明 n1 是 final
>   }
> }
> interface IB {
>   //接口中的属性,只能是 final 的，而且是 public static final 修饰符
>   int n1 = 10; //等价 public static final int n1 = 10;
>   void hi();
> }
> interface IC {
>   void say();
> }
> //接口不能继承其它的类,但是可以继承多个别的接口
> interface ID extends IB,IC {
> }
> //接口的修饰符 只能是 public 和默认，这点和类的修饰符是一样的
> interface IE{}
> //一个类同时可以实现多个接口
> class Pig implements IB,IC {
>   @Override
>   public void hi() {
>   }
>   @Override
>   public void say() {
>   }
> }
> ```
>
> 

# 接口 VS 继承类

小结：当子类继承了父类，就自动地拥有父类的功能。
如果子类需要扩展功能，可以通过实现接口的方式了扩展。

可以理解 实现接口 是 对 java 单继承机制的一种补充。

> 接口和继承解决的问题不同
>
> 1. 继承的价值主要在于：解决代码的 复用性 和 可维护性。
>
> 2. 接口的价值主要在于：设计，设计好各种规范（方法），让其它类去实现这些方法。
>
> 接口比继承更加灵活
>
> 1. 接口比继承更加灵活，继承是满足 is-a的关系，而接口只需满足like-a的关系。
>
> 接口在一定程度上实现代码解耦【即 接口规范性 + 动态绑定】

# 接口的多态特性

1） 多态参数（前面案例体现）InterfacePolyParameter.java
在前面的Usb接口案例，`Usblnterface usb`，既可以接收手机对象，又可以接收相机对象，就体现了 接口 多态（接口引用可以指向实现了接口的类的对象）。

> 接口类型的变量 可以指向 实现了该接口的类的对象

2） 多态数组 InterfacePolyArr.java
演示一个案例：给Usb数组中，存放 Phone 和 相机对象， Phone类还有一个特有的方法call()。请遍历Usb数组，如果是Phone对象，除了调用Usb 接口定义的方法外，还需要调用 Phone 特有方法 call。

```java
package com.hspedu.interface_;
public class InterfacePolyParameter {
  public static void main(String[] args) {
    //接口的多态体现
    //接口类型的变量 if01 可以指向 实现了 IF 接口类的对象实例
    IF if01 = new Monster();
    if01 = new Car();
    //继承体现的多态
    //父类类型的变量 a 可以指向 继承 AAA 的子类的对象实例
    AAA a = new BBB();
    a = new CCC();
  }
}
interface IF {}
class Monster implements IF{}
class Car implements IF{}
class AAA {
}
class BBB extends AAA {}
class CCC extends AAA {}


package com.hspedu.interface_;
public class InterfacePolyArr {
  public static void main(String[] args) {
    //多态数组 -> 接口类型数组
    Usb[] usbs = new Usb[2];
    usbs[0] = new Phone_();
    usbs[1] = new Camera_();
    /*
给 Usb 数组中，存放 Phone 和 相机对象，Phone 类还有一个特有的方法 call（），
请遍历 Usb 数组，如果是 Phone 对象，除了调用 Usb 接口定义的方法外，
还需要调用 Phone 特有方法 call
*/
    for(int i = 0; i < usbs.length; i++) {
      usbs[i].work();//动态绑定.. //和前面一样，我们仍然需要进行类型的向下转型
      if(usbs[i] instanceof Phone_) {//判断他的运行类型是 Phone_
        ((Phone_) usbs[i]).call();
      }
    }
  }
}
interface Usb{
  void work();
}
class Phone_ implements Usb {
  public void call() {
    System.out.println("手机可以打电话...");
  }
  @Override
  public void work() {
    System.out.println("手机工作中...");
  }
}
class Camera_ implements Usb {
  @Override
  public void work() {
    System.out.println("相机工作中...");
  }
}
```

3）接口存在多态传递现象.InterfacePolyPass.java

> 1. 接口类型的变量可以指向，实现了该接口的类的对象实例
> 2. 如果A接口继承了另一个B 接口。而类 C 实现了 A 接口，就相当于类 C也实现了 B 接口，那么，B 接口引用变量可以用来指向 类C 的实例对象。这就是接口的多态传递现象！

```java
package com.hspedu.interface_;
/**
* 演示多态传递现象
*/
public class InterfacePolyPass {
  public static void main(String[] args) {
    //接口类型的变量可以指向，实现了该接口的类的对象实例
    IG ig = new Teacher();
    //如果 IG 继承了 IH 接口，而 Teacher 类实现了 IG 接口
    //那么，实际上就相当于 Teacher 类也实现了 IH 接口. //这就是所谓的 接口多态传递现象. IH ih = new Teacher();
  }
}
interface IH {
  void hi();
}
interface IG extends IH{ }
class Teacher implements IG {
  @Override
  public void hi() {
  }
}
```

> 当一个对象访问一个父类和实现的接口都有的变量时，用 super.变量名指定父类变量，用 接口名.变量名指代接口变量，避免混淆，否则编译器会报错！

# Java核心技术卷补充：接口可定义内容

1. 首先接口本身的访问修饰符只能是 public 或者 默认

2. 接口的抽象方法不写public是因为默认是public abstract的，但是Implements的时候要写public。

3. 记录和枚举类不能extends其他类，因为他们隐式地继承了Record类和Enum类。但是可以实现接口。

4. ==CharSequence接口==包含了管理所有字符序列的类的公共方法。String和StringBuilder以及另外一些“string-like”类都实现了这个接口。有一个共同的接口会鼓励程序员编写使用CharSequence接口的方法。哪些方法可以处理String、StringBuilder 和 其他的 类字符串 类的实例。 如果你要处理字符串，而那些操作已经能满足你的任务要求则可以接受CharSequence实例，而不是字符串。

5. 在==Java8中允许在接口中增加静态方法==。在标准库中，你会看到成对出现的接口和实用工具类，如 Collection和Collections，Path和Paths。但是Java11中，Path接口实现了Paths工具类中等价的静态方法。这样一来，Paths类就不再是必要的了。

6. 在Java9，接口中的方法可以使private方法。private方法可以是静态方法或实例方法，由于私有方法只能在接口本身的方法中使用，所以他们的用途很有限，只是作为接口中其他方法（default）的辅助方法。

7. 如果设计你自己的接口，其中只有一个抽象方法可以用`@FunctionalInterface`来标记这个接口这样做有两个优点，如果你无意中增加了另一个抽象方法，编译器会给出一个错误消息，另外Javadoc页中会指出你的接口是一个函数式接口.

   

   # Java核心技术卷补充：默认方法细节

   1. 默认方法可以调用其他方法。
   2. default 和 abstract 是 非法的修饰符组合。
   3. 默认方法的一个重要用法是==接口演化 interface evolution==。以collection接口为例，这个接口作为Java的一部分，已经有很多年了。

> 假设很久以前你提供了一个这样的类。
>
> `public class Bag implements Collection`
>
> 后来，，在Java8中，又为这个接口增加了一个stream方法。
>
> 假设Stream方法不是一个默认方法，，那么Bag类将不能编译（因为要实现所有抽象方法，所以必须实现stream）， 因为他没有实现这个新方法为接口增加一个非默认方法，不能保证 ==源代码兼容source compatible==。不过，假设不重新编译这个类而只是使用原先的一个包含这个类的jar文件，这个类仍能正常加载，尽管没有这个新方法程序仍然可以正常构造Bag实例，不会有意外发生。为接口增加方法可以做到二进制兼容。不过如果一个程序在一个Bag实例上调用Stram方法，就会出现一个AbstractMethodError。
>
> 将Collection中新增的stream方法实现为一个默认default方法就可以解决这两个问题。Bag类（无需修改）又能正常编译了，另外如果没有重新编译而直接加载这个类，并在一个Bag实例上调用Stram方法则会调用Collection.stream默认方法。

### 解决默认方法冲突

如果先在一个接口中将一个方法定义为默认方法，然后又在超类或另一个接口中定义了同样的方法，会发生什么情况？

1. 超类优先。==如果超类提供了一个具体方法，同名而且有相同参数类型的默认方法会被忽略。==一个类扩展了一个超类，同时实现了一个接口，并从超类和接口继承了相同的方法，例如假设person是一个类，student定义为`class student extent person implements named{}`。在这种情况下只会考虑超类方法，接口的所有默认方法都会被忽略。在我们的例子中，student从person继承了getName方法，named接口是否为getName提供了默认实现并不会带来什么区别，这正是 类优先原则。 
2. 因此，绝对不能创建一个默认方法重新定义Object类中的某个方法。如，不能为toString或equals定义默认方法，由于类优先原则，这样的方法绝对无法超越Object.toString或Objects.equals.
3. 以上规则也正是为什么接口Comparator中的 `boolean equals(Object obj)`，方法无需实现的根本原因所在。Java核心技术卷12P256 介绍了 java API中的一些接口会重新声明Object方法来附加javadoc注释，Comparator API 就是这样一个例子。所以说这才是他在Comparator中再次声明一个equals方法的目的（附加javadoc注释）。
4. 接口冲突。 如果一个接口提供了一个默认方法，另一个接口提供了一个同名而且参数类型相同的方法（不论是否是默认方法），<u>***必须覆盖这个方法来解决冲突***</u>。 两个接口 如何冲突并不重要，如果至少有一个接口提供了一个实现编译器就会报告错误，必须由程序员解决这个二义性。如果两个方法都是抽象的没有实现，那么本来就必须要覆盖掉，等于不会产生二义性。
