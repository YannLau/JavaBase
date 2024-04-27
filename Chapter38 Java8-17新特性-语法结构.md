[toc]

# Java的REPL工具：jShell命令

- JDK9的新特性

Java终于拥有了像Python 和lcala 之类语言的REPL工具（交互式编程环境，read - evaluate - print - loop）：jshe11。以交互式的方式对语句和表达式进行求值。即写即得、快速运行。

利用jShell在没有创建类的情况下，在命令行里直接声明变量，计算表达式，执行语句。无需跟人解释"`public static void main(String[] args)`”这句"废话"。

1．命今行中通过指今jshell，调取jshell工具
2．常用指令：
/help：获取有关使用 jsheLl 工具的信息.
/help intro :jshell 工具的简介
/List ：列出当前 session 里所有有效的代码片段
/vars ：查看当前 session 下所有创建过的变量
/methods ：查看当前session 下所有创建过的方法
/imports ：列出导入的包
/history ：键入的内容的历史记录
/edit：使用外部代码編辑器来編写 Java 代码
/exit ：退出 jsheLl 工具

# 异常处理之try-catch资源关闭

在JDK7 之前，我们这样处理资源的关闭：

```java
@Test
public void test01(){
  FileWriter fw = null;
  BufferedWriter bw = null;
  try {
    fw = new FileWriter("d:/1.txt");
    bw = new BufferedWriter(fw);
    bw.write("hello");
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      if (bw != null) {
        bw.close());
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
    try {
      if (fw != null){
        fw.close();
      }
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

- JDK7新特性

在try的后面可以增加一个 () ，在括号中可以声明流对象并初始化。try中的代码执行完毕，会自动把流对象释放，就不用写finally了。

实现了 ==AutoCloseable类==的对象实例都可以放在 try()的括号里，自动关闭。

格式

```java
public void test020() {
  try (FileWriter fw = new FileWriter("test.txt");
       BufferedWriter bw = new BufferedWriter(fw);
      ) {
    bw.write("hello");
  } catch (IOException e) {
    e.printStackTrace();
  }
}

try（资源对象的声明和初始化）｛
  业务逻辑代码，可能会产生异常
  ｝catch（异常类型1 e）｛
  处理异常代码
  ｝catch（异常类型2 e）｛
  处理异常代码
  ｝
```

说明：

1、在try()中声明的资源，无论是否发生异常，无论是否处理异常，都会自动关闭资源对象，不用手动关闭了。
2、这些资源实现类必须实现AutoCloseable或Closeable接口，实现其中的close()方法。Closeable是AutoCloseable的子接口。Java7几乎把所有的“资源类”（包括文件IO的各种类、JDBC编程的Connection、Statement等接口....）都进行了改写，改写后资源类都实现了AutoCloseable或Closeable接口，并实现了close()方法。

- Java9 新特性

try的前面可以定义流对象，try后面的（中可以直接引用流对象的名称。在try代码执行完毕后，流对象也可以释放掉，也不用写finally了。

```java
A a = new A();
B b = new B();

try(a;b){
  可能产生的异常代码
    //不可以再对a和b进行修改了
    //a和b是事实上的final变量
} catch(异常类名 变量名){
  异常处理的逻辑
}
```

# 类型推断

- JDK10 新特性

局部变量的显示类型声明，常常被认为是不必须的，给一个好听的名字反而可以很清楚的表达出下面应该怎样继续。本新特性允许开发人员省略通常不必要的局部变量类型声明，以增强Java语言的体验性、可读性。

```java
public static void main(String[] args) {
  //1.局部变量的实例化  
  var list = new ArrayList<String>();
  var set = new LinkedHashSet<Integer>();
  //2.增强for循环中的索引
  for (var v : list) {
    System.out.println(v);
  }
  //3.传统for循环中
  for (var i = 0; i < 100; i++) {
    System.out.println(i);
  }
}
```

- 不适用场景
  • 声明一个成员变量
  。声明一个数组变量，并为数组静态初始化（省略new的情况下）
  • 方法的返回值类型
  • 方法的参数类型
  。 没有初始化的方法内的局部变量声明
  。 作为catch块中异常类型
  • Lambda表达式中函数式接口的类型
  。 方法引用中函数式接口的类型

# instanceof

JDK14中预览特性。
JDK15中第二次预览。
JDK16中转正特性。

```java
JDK14之前：
  // 先判断类型
  if (obj instanceof String) {
    // 然后转换
    String s = (String) obj;
    // 然后才能使用
  }

JDK14：（自动匹配模式）
  if(obj instanceof String str) {
    // 如果类型匹配 直接使用
    System.out.println(str.contains("Java"));
  } else {
    // 如果类型不匹配则不能直接使用
    System.out.println("非String类型");
  }

public boolean equals(Object o){
  return o instanceof Computer other && this.model. equals(other.model)
    && this.price == other.price;
}
```

# switch

JDK12中预览特性：switch表达式
JDK13中二次预览特性：switch表达式
JDK14中转正特性：switch表达式
JDK17的预览特性：switch的模式匹配

- 传统switch声明语句的弊端：

  - 匹配是自上而下的，如果忘记写break，后面的case语句不论匹配与否都会执行；
  - 所有的case语句共用一个块范围，==在不同的case语句定义的变量名不能重复==；
  - 不能在一个case里写多个执行结果一致的条件；
  - 整个switch不能作为表达式返回值；

  ```java
  public class dawd {
    @SuppressWarnings("all")
    public static void main(String[] args) {
      Week day = Week.FRIDAY;
      switch (day) {
        case MONDAY:
          int num = 10;
          System.out.println(1);
          break;
        case TUESDAY:
        case WEDNESDAY:
        case THURSDAY: //multiple cases can't be putted together
          int num =10; //Error variable num has been defined elsewhere!
          System.out.println(2);
          break;
        case FRIDAY:
          System.out.println(3);
          break;
        case SATURDAY:
        case SUNDAY:
          System.out.println(4);
          break;
        default:
  				System.out.println(0);
      }
    }
  }
  
  enum Week {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
  }
  ```

  

- JDK12中预览特性
  - Java 12将会对switch声明语句进行扩展，使用`case L ->` 来替代以前的 break；省去了 break 语句，避免了因少写 break 而出错。
  - 同时将多个 case 合并到一行，显得简洁、清晰，也更加优雅的表达逻辑分支。
  - 为了保持兼容性，case 条件语句中依然可以使用字符:，但是同一个 switch 结构里不能混用-> 和 : ，否则编译错误。

```java
public class dawd {
  @SuppressWarnings("all")
  public static void main(String[] args) {
    Week day = Week.FRIDAY;
    switch (day) {
      case MONDAY -> {
        int num = 10;
        System.out.println(1);
      }
      case TUESDAY, WEDNESDAY, THURSDAY -> {
        int num = 10;
      } //Correct! {} represents a new region.
      case FRIDAY -> System.out.println(3);
      case SATURDAY, SUNDAY -> System.out.println(4);
      default -> throw new IllegalStateException("Unexpected value: " + day);
    }
  }
}

enum Week {
  MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

- JDK12中的写法：还可以使用变量接收switch表达式的结果。

```java
public class dawd {
    @SuppressWarnings("all")
    public static void main(String[] args) {
        Week day = Week.FRIDAY;
        int result = switch (day) {
            case MONDAY -> 1;
            case TUESDAY, WEDNESDAY, THURSDAY -> 2;
            case FRIDAY -> 3;
            case SATURDAY, SUNDAY -> 4;
            default -> throw new IllegalStateException("Unexpected value: " + day);
        };
    }
}
// -> 后面 必须为 throw 表达式 或者 块。
// 当只有要返回的结果时无需{} ,直接写值。
enum Week {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

- JDK13中的写法：引入了yield关键字，用于返回指定的数据，结束switch结构。

- 这意味着，switch表达式（返回值）应该使用yield.

- switch语句（不返回值）应该使用break。和return的区别在于return会直接跳出当前方法，而yield只会跳出当前switch块。

- 且return不能出现在 ->

  ==根据Java核心技术卷的描述， switch应该分为switch语句和switch表达式。switch语句是无返回值的，switch表达式是有返回值==

  switch语句中可以直接return；switch表达式不能出现return，只能用yield或者单句的返回值。 switch表达式字面上应该必须有一个返回值，但是如果抛异常的话就不需要了。

```java
public class dawd {
  @SuppressWarnings("all")
  public static void main(String[] args) throws Exception {
    Week day = Week.FRIDAY;
    int result = switch (day) {
      case MONDAY -> {
        yield 1;
      }
      case TUESDAY, WEDNESDAY, THURSDAY -> 2；
      case FRIDAY -> {
        System.out.println(12);
        throw new Exception("fuck error!");
      }
      case SATURDAY, SUNDAY -> {
        yield 4;
      }
      default -> {
        System.out.println("Error value!");
        throw new IllegalStateException("Unexpected value: " + day);
      }
    };
  }
}

// -> 后面 必须为 throw 表达式 或者 块。
// 当只有要返回的结果时无需{} ,直接写值。
enum Week {
  MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

```java
public class dawd {
    @SuppressWarnings("all")
    public static void main(String[] args) throws Exception {
        Week day = Week.FRIDAY;
        int result = switch (day) {
            case MONDAY:
                int a = 1;
                yield 1;
            case TUESDAY, WEDNESDAY, THURSDAY:
                int a = 2; //Error!
                yield 2;
            case FRIDAY: { // {} is unnecessary! But it is legal!
                System.out.println(12);
                throw new Exception("fuck error!");
            }
            case SATURDAY, SUNDAY:
                yield 4;
            default:
                System.out.println("Error value!");
                throw new IllegalStateException("Unexpected value: " + day);
        };
    }
}

// -> 后面 必须为 throw 表达式 或者 块。
// 当只有要返回的结果时无需{} ,直接写值。
enum Week {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}
```

总结 `-> {}` 可以隔离变量，导致可以重复定义。 `：` 不行

- JDK17 switch模式匹配

JDK17之前：

```java
    static String formatter(Object o) {
        String formatted = "unknown";
        if (o instanceof Integer i) {
            formatted = "int " + i;
        } else if (o instanceof Long l) {
            formatted = "long " + l;
        } else if (o instanceof Double d) {
            formatted = "double " + d;
        } else if (o instanceof String s) {
            formatted = "String " + s;
        }
        return formatted;
    }
```

JDK17中的switch模式匹配

```java
@SuppressWarnings("all")
static String formatterSwitchPattern(Object o) {
  String formatted = switch (o) {
    case Integer i:
      yield "int " + i;
    case Double d:
      yield "double " + d;
    case Long l:
      yield "long " + l;
    case Character c:
      yield "char " + c;
    case String str:
      yield "String " + str;
    default:
      throw new IllegalStateException("Unexpected value: " + o);
  };
  return formatted;
}
```

# 文本块

JDK13的预览特性
JDK14中二次预览特性
JDK15中功能转正

```java
//以前

String info = "<html>\n" +
  " <body>\n" +
  " <P>Hello，尚硅谷</p>\n" +
  " </body>\n" +
  "</htmly>";
System.out.println(info);

//现在
String info = """
  <html>
  <body>
  <P>Hello，尚硅谷</p>
  </body>
  </htmly>
  """;
  System.out.println(info);

//以前
String myJson = "{\n" +
  "   \"name\": \"Song Hongkang\", \n" +
  "   \"gender\"：\"男\",\n" +
  "   \"address\":\"www.atguigu.com\"\n" +
  "}";
System.out.println(myJson);

//现在
String myJson1 = """
  ｛
  "name": "Song Hongkang",
"gender"："男"，
  "address":"www.atguigu.com"
    ｝""";
    System.out.println(myJson1);
}

//JDK14新特性
//* \：取消换行操作
//* \S：表示一个空格

public void test() {
  String newQuery1 = """
    SELECT id, name, email \
    FROM customers
    WHERE id > 4 \
    ORDER BY email DESC
    """;
    System.out.println(newQuery1);
}

//输出结果
SELECT id, name, email FROM customers
  WHERE id > 4 ORDER BY email DESC
```

# record

JDK14中预览特性
JDK15中第二次预览特性
JDK16中转正特性

早在2019年2月份，Java 语言架构师 Brian Goetz，曾写文抱怨“Java太啰嗦”或有太多的“繁文缛节”。他提到：开发人引根要创建纯数据载体类 （plain data carriers）通常都必须编写大量低价值、重复的、容易出错的代码。

如：构造函数、getter/setter、equals、hashCode 以及 toString 等。

以至于很多人选择使用IDE的功能来自动生成这些代码。还有一些开发会选择使用一些第三方类库，如 Lombok (通过注解) 等来生成这些方法。

JDK14中预览特性：神说要用record，于是就有了。实现一个简单的数据载体类，为了避免编写：构造函数，访问器，equals，hashCode，toString等，Java 14推出record。

record 是一种全新的类型，它本质上是一个==final 类==，同时所有的属性都是 final 修饰，它会自动编译出public get、hashcode、equals、toString、构造器等结构，减少了代码编写量。具体来说：当你用record 声明一个类时，该类将自动拥有以下功能：

- 获取成员变量的简单方法，比如例题中的 name()和 partner()。注意区别于我们平常getter()的写法。
- 一个equals方法的实现，执行比较时会比较该类的所有成员属性。
- 重写 hashCode()方法。
- 一个可以打印该类所有成员属性的 toString 方法。
- 只有一个构造方法。
- 此外：
  • 还可以在record声明的类中定义静态字段、静态方法、构造器或实例方法。
  • 不能在record声明的类中定义实例字段；不能声明为abstract；不能声明显式的父类等. 因为已经有一个父类Record。

```java
import java.util.Objects;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName order
 * @ClassPath PACKAGE_NAME.order
 * @create 2024-04-25 10:35
 * @description
 */
public class order {
    //属性 private final修饰
    private final int orderId;
    private final String orderName;
    public order(int orderId, String orderName) {
        this.orderId = orderId;
        this.orderName = orderName;
    }

    public int orderId() {
        return orderId;
    }

    public String orderName() {
        return orderName;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        order order = (order) o;
        return orderId == order.orderId && Objects.equals(orderName, order.orderName);
    }

    @Override
    public int hashCode() {
        return Objects.hash(orderId, orderName);
    }

    @Override
    public String toString() {
        return "order{" +
                "orderId=" + orderId +
                ", orderName='" + orderName + '\'' +
                '}';
    }
}
```

等价于

```java
public record order_record(int orderId, String orderName) {
}
```

测试

```java
class daw{
  public static void main(String[] args) {
    System.out.println("OK!!");
    order_record order_record = new order_record(1001, "飞机");
    System.out.println(order_record.orderId());
    System.out.println(order_record.toString());
  }
}
```

最终到JDK16中转正。

记录不适合哪些场景

record的设计目标是提供一种将数据建模为数据的好方法。它也不是 JavaBeans 的直提替代品，因为record的方法不符合 JavaBeans 的 get 标准。另外 JavaBeans 通常是可变的，而记录是不可变的。尽管它们的用途有点像，但记录并不会以某种方式取代JavaBean。

# 密封类

JDK15的预览特性
JDK16二次预览特性
JDK17转正特性

背景：
在 Java 中如果想让一个类不能被继承和修改，这时我们应该使用 final 关键字对类进行修饰。不过这种要么可以继承，要么不能继承的机制不够灵活，有些时候我们可能想让某个类可以被某些类型继承，但是又不能随意继承，是做不到的。Java 15 尝试解决这个问题，引入了
sealed。被sealed 修饰的类可以指定子类。这样这个类就只能被指定的类继承。

- JDK15的预览特性：通过密封的类和接口来限制超类的使用，密封的类和接口限制其它可能继承或实现它们的其它类或接口。具体使用：

```java
public sealed class Person permits Student,Teacher,Worker{
}
//可以无视,去继承别的类
class Student extends Thread implements Comparable<Student> {
  @Override
  public int compareTo(Student o) {
    return 0;
  }
}
//可以无视,不去继承
class Teacher{
}
//如果继承,继承后的类应为修饰符sealed、'non-sealed' 或 final 三者之一
// sealed 又要指定 permits 了
// final 直接不玩了,不能被任何类继承了
// non-sealed 无任何继承限制了,可以被任何类继承了
non-sealed class Worker extends Person{
}
class WatchWorker extends Worker{ // That's OK
}
//Error 无继承权限
class Dancer extends Person{
}
```

