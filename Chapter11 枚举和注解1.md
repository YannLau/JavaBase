P425——P443

[toc]

1）枚举对应英文（enumeration， 简写 enum）
2）枚举是一组常量的集合。
3）可以这里理解：枚举属于一种特殊的类，里面只包含一组有限的特定的对象。
4）只读，不需要修改。

> 枚举的两种实现方式
>
> 1. ==自定义类实现枚举==
> 2. ==使用 enum 关键字实现枚举==

# 自定义枚举

> 自定义枚举实现
>
> 1. 将构造器私有化,防止直接 new
> 2. 去掉 setter 方法,防止被修改
> 3. 在类内部直接创建固定的对象,作为 private 成员变量
> 4. 优化,加上 final 关键字.
> 5. 不需要提供setXxx 方法，因为枚举对象值通常为只读.
> 6. 对枚举对象/属性使用 final + static 共同修饰，实现底层优化.
> 7. 枚举对象名通常使用全部大写，常量的命名规范.
> 8. 枚举对象根据需要，也可以有多个属性

```java
/**
* @author 韩顺平
* @version 1.0
*/
public class Enumeration02 {
  public static void main(String[] args) {
    System.out.println(Season.AUTUMN);
    System.out.println(Season.SPRING);
  }
}
//演示字定义枚举实现
class Season {//类
  private String name;//枚举名
  private String desc;//描述
  //定义了四个对象, 固定. public static final Season SPRING = new Season("春天", "温暖");
  public static final Season WINTER = new Season("冬天", "寒冷");
  public static final Season AUTUMN = new Season("秋天", "凉爽");
  public static final Season SUMMER = new Season("夏天", "炎热");
  //1. 将构造器私有化,目的防止 直接 new
  //2. 去掉 setXxx 方法, 防止属性被修改
  //3. 在 Season 内部，直接创建固定的对象
  //4. 优化，可以加入 final 修饰符
  private Season(String name, String desc) {
    this.name = name;
    this.desc = desc;
  }
  public String getName() {
    return name;
  }
  public String getDesc() {
    return desc;
  }
  @Override
  public String toString() {
    return "Season{" +
      "name='" + name + '\'' +
      ", desc='" + desc + '\'' +
      '}';
  }
}
```

> 小结
>
> 1）构造器私有化
> 2） 本类内部创建一组对象［四个 春夏秋冬］
> 3） 对外暴露对象（通过为对象添加public final static修饰符）
> 4） 可以提供get方法，但是不要提供 set

# enum 枚举

使用 enum 来实现前面的枚举案例，看老师演示，主要体会和自定义类实现枚举不同的地方。Enumeration03.java 代码

```java
//演示使用 enum 关键字来实现枚举类
enum Season{
  //如果使用了 enum 来实现枚举类
  //1. 使用关键字 enum 替代 class
  //2. public static final Season SPRING = new Season("春天", "温暖") 直接使用 SPRING("春天", "温暖")
  //   解读 常量名(实参列表)
  //3. 如果有多个常量(对象)， 使用 , 号间隔即可
  //4. 如果使用 enum 来实现枚举，要求将定义常量对象，写在前面
  //5. 如果我们使用的是无参构造器，创建常量对象，则可以省略 ()

  //enum 关键字实现枚举要求把常量对象写在枚举类的最前面
  SPRING("春天","温暖"),
  SUMMER("夏天","炎热"),
  AUTUMN("秋天","凉爽"),
  WINTER("冬天","寒冷"),
  WHAT;  
  //常量名(实参列表)

  private String name;
  private String desc;

  private Season(){ //无参构造器

  }
  private Season(String name,String desc) {
    this.name = name;
    this.desc = desc;
  }
  public String getName(){
    return this.name;
  }
  public String toString(){
    ....
  }
}
```

> enum 注意事项
>
> 1. 当我们使用 enum 关键字开发一个枚举类时，`默认会继承 Enum类 (一个 final 类)` , 可以用反编译工具 javap 证明.
>
> 2. 从父类的Enum类中会继承几个好用的方法：
>
>    `public final String name()`
>
>    `public static org.yannlau.Season valueOf(String name)`
>
>    `public static <T extends Enum<T>> T valueOf(Class<T> enumType,String name)`
>
>    `public static org.yannlau.Season[] values()`
>
>    `compareTo(E o)`
>
>    `ordinal()`：返回当前对象的位置号，默认从0开始
>    
>    ```java
>    package org.yannlau;
>    
>    import java.util.Arrays;
>    import java.util.List;
>    import java.util.stream.Collectors;
>    import java.util.stream.Stream;
>    
>    public enum Season {
>      SPRING,
>      SUMMER,
>      AUTUMN,
>      WINTER(4, "寒冷");
>    
>      private int value;
>      private String description;
>      private static final Season[] seasons = values();
>    
>      Season() {
>      }
>    
>      Season(int value, String description) {
>        this.value = value;
>        this.description = description;
>      }
>    
>      public int getValue() {
>        return value;
>      }
>    
>      public String getDescription() {
>        return description;
>      }
>    
>      public static void printAll() {
>        //测试values()的返回值
>        System.out.println("=========测试values()的返回值===========");
>        Arrays.stream(seasons).forEach(System.out::println);
>        System.out.println("&&&&&&&&&&&&&&");
>        Stream<Season> stream = Arrays.stream(seasons);
>        List<Season> collect = stream.collect(Collectors.toList());
>        System.out.println(collect);
>        System.out.println("=========测试name()的返回值========");
>        String name = SPRING.name();
>        System.out.println(name);
>        System.out.println("=========测试valueOf(String)的返回值========");
>        Season awd = Season.valueOf("AUTUMN");
>        System.out.println(awd);
>        System.out.println("========展示valueOf(Class<T> enumType,String name)的用法========");
>        Season summer = Enum.valueOf(Season.class, "SUMMER");
>        System.out.println(summer);
>        System.out.println("========展示compareTo(E o)的用法========");
>        int i = Season.SPRING.compareTo(Season.AUTUMN); //spring-order: 0  autumn-order:2   0 - 2 = -2
>        System.out.println("compareTo()的结果为 -> " + i);
>      }
>    }
>    
>    printAll()的输出结果：
>    
>      =========测试values()的返回值===========
>      SPRING
>      SUMMER
>      AUTUMN
>      WINTER
>      &&&&&&&&&&&&&&
>      [SPRING, SUMMER, AUTUMN, WINTER]
>      =========测试name()的返回值========
>      SPRING
>      =========测试valueOf(String)的返回值========
>      AUTUMN
>      ========展示valueOf(Class<T> enumType,String name)的用法========
>      SUMMER
>      ========展示compareTo(E o)的用法========
>      compareTo()的结果为 -> -2
>    ```
>
> 
>
> 3. 传统的 `public static final Season2 SPRING = new Season2（"春天”，"温暖"）`；简化成 `SPRING(“春天”,“温暖")`，这里必须知道，它调用的是哪个构造器.
>
> 4. 如果使用无参构造器 创建 枚举对象，则实参列表和小括号都可以省略 .  见上方代码 `WHAT`
>
> 5. `当有多个枚举对象时，使用，间隔，最后有一个分号结尾`
>
> 6. 枚举对象必须放在枚举类的行首.
>
> 7. 枚举类不能显式地继承类`'extends' not allowed on enum`，`但是可以实现接口`
>
> 8. `Modifier 'private' is redundant for enum constructors` 枚举类型的构造器的private关键字是多余的，可以省略
>

> 练习题
>
> ```java
> enum Gender{
>   BOY,GIRL; //这里其实就是调用Gender 类的无参构造器
> }
> 
> //外部打印 Gender.BOY 会直接打印字符串"BOY"
> String s = String.valueOf(Season.BOY);//可以将枚举类转成字符串复制给 String 类型.
> 
> enum Season{
>   SPRING,SUMMER
> }
> 
> //反编译结果
> public final class Season extends java.lang.Enum<Season> {
>   public static final Season SPRING;
>   public static final Season SUMMER;
>   public static Season[] values();
>   public static Season valueOf(java.lang.String);
>   static {};
> }
> ```
>
> 1） 上面语法是ok的
> 2） 有一个枚举类Gender，没有属性。
> 3） 有两个枚举对象 BOY,GIRL，使用的无参构造器创建.

常量值枚举类

`Gender boy1 = Gender.BOY;`

`Gender boy2 = Gender.BOY;`

boy1 == boy2  // true!  `因为枚举类型的对象是静态对象!`

# Enum 类中的方法

说明：使用关键字enum时，会隐式继承Enum类，这样我们就可以使用Enum类相关的方法。

`valueOf`传递枚举类型的 Class 对象和枚举常量名称给静态方法valueOf，会得到与参数匹配的枚举常量

`toString`得到当前枚举常量的名称。你可以通过重写这个方法来使得到的结果更易读 .

`equals`在枚举类型中可以直接使用“==”来比较两个枚举常量是否相等。Enum提供的这个 equals()方法，也是直接使用“==”实现的。它的存在是为了在 Set、 List 和 Map 中使用。注意，equals() 是不可变的。

`hashCode` Enum 实现了 hashCode()来和 equals()保持一致。它也是不可变的。
`getDeclaringClass` 得到枚举常量所属枚举类型的 Class对象。可以用它来判断两个枚举常量是否属于同一个枚举类型。
`name` 得到当前枚举常量的名称。建议优先使用toString()。
`ordinal` 得到当前枚举常量的次序  从零开始编号的。

`compareTo` 枚举类型实现了 Comparable 接口，这样可以比较两个枚举常量的大小（按照声明的顺序排列）

`clone` 枚举类型不能被 Clone。为了防止子类实现克隆方法，Enum 实现了一个仅抛出 CloneNotSupportedException 异常的不变Clone0。

> 1. toString:Enum类已经重写过了，返回的是当前对象
>     名.子类可以重写该方法，用于返回对象的属性信息
> 2. name：返回当前对象名（常量名），子类中不能重写
> 3. ordinal：返回当前对象的位置号，默认从0开始
> 4. values：返回当前枚举类中所有的常量 , 隐藏的 , **<u>*在反编译中可以看到*</u>** .  `枚举类名.values();`返回一个枚举类数组.
> 5. valueOf：*<u>**将字符串转换成枚举对象，要求字符串必须**</u>*
>     为已有的常量名，否则报异常！
> 6. compareTo：比较两个枚举常量，比较的就是数组中的位置号！`return self.ordinal - other.ordinal;`

```java
for (Season value : Season.values()) {  //增强for 循环打印 values 枚举数组
    System.out.println(value);
}
```

> enum 小结
>
> 1）使用enum关键字后，就不能再继承其它类了，因为enum会隐式继承Enum，而Java是单继承机制。
> 2）枚举类和普通类一样，可以实现接口，如下形式。enum 类名 implements 接口1,接口2 { }

# 注解 Annotation

1. 注解（Annotation）也被称为元数据（Metadata），用于修饰解释包、类、方法、属性、构造器、局部变量等数据信息。
2. 和注释一样，注解不影响程序逻辑，但注解可以被编译或运行，相当于嵌入在代码中的补充信息。
3. 在JavaSE中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在JavaEE中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替java EE旧日版中所遗留的繁冗代码和XML配置等。

> 基本的 Annotation 介绍
>
> 使用 Annotation 时要在其前面增加 @符号，并把该 Annotation 当成一个修饰符使用。用于修饰它支持的程序元素
> >三个基本的 Annotation：
> >1）@Override：限定某个方法，是重写父类方法，该注解只能用于方法
> >2） @Deprecated：用于表示某个程序元素（类，方法等）已过时
> >3） @SuppressWarnings：抑制编译器警告

# @Override

> @Override
>
> 1. @Override 注解放在fly方法上，表示子类的fly方法时重写了父类的fly
> 2. 这里如果没有写@Override 还是重写了父类fly
> 3. 如果你写了@Override注解，编译器就会去检查该方法是否真的重写了父类的方法，如果的确重写了，则编译通过，如果没有构成重写，则编译错误
> 4. ＞补充说明：
>    @interface 的说明
>    @interface 不是interface接口，是 注解类 是jdk1.5之后加入的.

> @override 使用说明
>
> 1. @Override 表示指定重写父类的方法（从编译层面验证），如果父类没有fly方法，则会报错
>
> 2. 如果不写@Override 注解，而父类仍有 `public void fly(){}`，仍然构成重写
> 3. @override 只能修饰方法，不能修饰其它类，包，属性等等
> 4. 查看@Override注解源码为 @Target(ElementType.METHOD) , 说明只能修饰
>     方法
> 5. @Target 是修饰注解的注解，称为元注解

# @Deprecated

@Deprecated 修饰某个元素，表示该元素已经过时 , 即不再推荐使用，但是仍然可以使用 .

> @Deprecated 的说明
>
> 1. 用于表示某个程序元素（类，方法等）已过时
> 2. 可以修饰方法，类，字段，包，参数 等等
> 3. @Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD,
>     PACKAGE, PARAMETER, TYPE))
> 4. @Deprecated 的作用可以做到新旧版本的兼容和过渡

# @SuppressWarnings

1. 当我们不希望看到这些警告的时候，可以使用 SuppressWarnings注解来抑制警告信息
2. 在`{" "}`中，可以写入你希望抑制（不显示）警告信息 , 如`@SuppressWarnings({"boxing","cast"})`
3. 可以指定的警告类型有
    all : 抑制所有警告 etc
4. 关于SuppressWarnings 作用范围是和你放置的位置相关
5. SuppressWarnings 源码查看
  1. 放置的位置就是 TYPE,FIELD,METHOD,PARAMETER, CONSTRUCTOR,LOCAL_VARIABLE
  2. 该注解类有数组

    `String[] values()` 设置一个数组比如 `{"rawtypes", "unchecked", "unused"}`

> ＞说明各种值
> 1） unchecked 是忽略没有检查的警告
> 2） rawtypes 是忽略没有指定泛型的警告（传参时没有指定泛型的警告错误）
> 3） unused 是忽略没有使用某个变量的警告错误
> 4） @SuppressWarnings 可以修饰的程序元素为，查看@Target
> 5） 生成@SupperssWarnings 时，不用背，直接点击左侧的黄色提示，就可以选择（注意可以指定生成的位置）

# 元注解 Annotation 了解

1. 元注解的基本介绍
   1. JDK 的元 Annotation 用于修饰其他 Annotation
   2. 元注解：本身作用不大，讲这个原因希望同学们，看源码时，可以知道他是干什么的
2. 元注解的种类（使用不多，了解，不用深入研究）
   1. Retention //指定注解的作用范围，三种 `SOURCE , CLASS , RUNTIME`
   2. Target // 指定注解可以在哪些地方使用
   3. Documented //指定该注解是否会在javadoc体现
   4. Inherited //子类会继承父类注解
      

> @Retention 注解
>
> ＞说明
> 只能用于修饰一个 Annotation 定义，用于指定该 Annotation 可以保留多长时间，@Rentention 包含一个 RetentionPolicy 类型的成员变量value ，使用 @Rentention时必须为该 value 成员变量指定值：
>
> > @Retention的三种值
> >
> > 1） RetentionPolicy.SOURCE：编译器使用后，直接丢弃这种策略的注解
> >
> > 2） RetentionPolicy.CLASS： 编译器将把注释记录在class 文件中. 当运行 Java 程序时，JVM 不会保留注解(即不读取使用)。这是默认值
> >
> > 3） RetentionPolicy.RUNTIME：编译器将把注解记录在 class 文件中.当运行Java 程序时，JVM 会保留注解(会加载到 JVM 中). 程序可以通过反射获取该注解

![image-20240207下午10028152](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午10028152.png)

> @Target 元注解
>
> ＞基本说明
> 用于修饰 Annotation 定义，用于指定被修饰的 Annotation 能用于修饰哪些程序元素
>
> @Target 也包含一个名为 value 的成员变量。
>
> `ElementType[] values();` 存储<u>***类型***</u>的枚举常量,表示一个注解可以作用的类型.

> @Documented注解
>
> > 基本说明
> > @Documented： 用于指定被该元 Annotation 修饰的 Annotation 类将被 javadoc 工具提取成文档，即在生成文档时，可以看到该注释。
> > <u>***说明：定义为Documented的注解必须设置Retention值为RUNTIME.</u>***

> @Inherited注解
>
> 被它修饰的 Annotation 将具有继承性.
>
> 如果某个类使用了被 @Inherited 修饰的 Annotation，则其子类将自动具有该注解.
>
> 说明：实际应用中，使用较少，了解即可。

元注解：本身作用不大，讲这个原因希望同学们，看源码时，可以知道他是干什么.

