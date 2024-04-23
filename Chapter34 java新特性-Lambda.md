[toc]

这部分来自【尚硅谷Java入门视频教程，宋红康java基础视频】https://www.bilibili.com/video/BV1Kb411W75N?p=687&vd_source=b77bfd0640ba59cb7c1bb8b3ad795165

Lambda 是一个匿名函数，我们可以把 Lambda 表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。使用它可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。

> 前置问题
>
> 为什么java的Comparator接口中的equals方法在实现该接口时可以不需要实现?

# 总体演示

```java
public void test1() {

  //经典的匿名内部类写法
  Runnable r1 = new Runnable() {
    @Override
    public void run() {
      System.out.println("我爱北京天安门");
    }
  };
  r1.run();

  System.out.println("--------");

  //因为只有一个方法需要进行重写，所以无任何歧义，又因为没有参数，并且只有单行语句 == 实现了匿名内部类
  //直接不加{}
  Runnable r2 = () -> System.out.println("我爱北京故宫");
  r2.run();

  System.out.println("---------");
}

public void test2() {
  //经典的内部类写法
  Comparator<Integer> com1 = new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
      return Integer.compare(o1, o2);
    }
  };
  int compare1 = com1.compare(12, 21);
  System.out.println(compare1);

  System.out.println("---------");
  //因为有两个参数，所以写上两个形参，为什么可以不带类型
  //他根据<>菱形表达式确定了是Integer
  Comparator<Integer> com2 = (o1, o2) -> Integer.compare(o1, o2);
  int compare2 = com2.compare(32, 21);
  System.out.println(compare2);

  System.out.println("---------");

  
  // int compare(T o1, T o2);
  // 当T确定为Integer后
  // 唯一需要Override的compare函数的
  // 返回值和参数类型全部和Integer的compare方法一直，
  // 可以直接方法引用
  Comparator<Integer> com3 = Integer::compare;
  int compare3 = com3.compare(32, 21);
  System.out.println(compare3);
}
```

# Lambda表达式

 * 1.举例 `(o1,o2) -> Integer.compare(o1,o2);`
 * 2.格式:
 * ->: lambda操作符 或 箭头操作符
 * ->的左边 Lambda形参列表(其实就是接口中的抽象方法的形参列表)
 * ->的右边 Lambda体 (其实就是重写的抽象方法的方法体)
 * 3.Lambda表达式的使用 (分为六种情况介绍)
 * 4.Lambda表达式的本质: ==作为接口的一个实现类的实例对象(且接口只有一个方法需要重写)==或者说是一个匿名函数。
 * 5.总结:
 * ->左边  Lambda形参列表的参数类型可以省略;如果Lambda形参列表只有一个参数,其一对()也可以省略
 * ->右边  Lambda体 应该使用一对大括号进行包裹:==如果只有一条执行语句(可能是return语句),可以省略这一对{}和return关键字==.
  * 6.==如果一个接口中，只声明了一个抽象方法，则此接口就称为函数式接口==

### 语法格式1 无参 无返回值

```java
public void test1() {
  Runnable r1 = new Runnable() {
    @Override
    public void run() {
      System.out.println("我爱北京天安门");
    }
  };
  r1.run();
  
  System.out.println("--------");
  
  Runnable r2 = () -> {
    System.out.println("我爱北京故宫");
  };
  r2.run();
}
```

### 语法格式2 需要一个参数,但是没有返回值.

```java
public void test2() {

  Consumer<String> con = new Consumer<String>() {
    @Override
    public void accept(String s) {
      System.out.println(s);
    }
  };
  con.accept("谎言和誓言的区别是什么呢?");

  System.out.println("****************");

  Consumer<String> con1 = (String s) -> {
    System.out.println(s);
  };
  con1.accept("一个是听得人当真了,一个是说的人当真了");

  //YannLau: 其实这里根据菱形表达式，会判断形参的类型
  Consumer<String> consumer = str -> {
    System.out.println(str);
  };
}
```

### 语法格式3 数据类型可以省略,因为可以通过编译器推断得出,称为"类型推断"

```java
public void test3() {

  Consumer<String> con1 = (String s) -> {
    System.out.println(s);
  };
  con1.accept("一个是听得人当真了,一个是说的人当真了");
  System.out.println("****************");

  Consumer<String> con2 = (s) -> { //这里括号都可以不加
    System.out.println(s);
  };
  con2.accept("一个是听得人当真了,一个是说的人当真了");
}

//类型推断说明
public void test4() {
  ArrayList<String> list = new ArrayList<>();//类型推断
  int[] arr = {1, 2, 3}; //类型推断
}
```

### 语法格式4 Lambda若只需要一个参数, 参数的小括号可以省略

```java
public void test5() {
  
  Consumer<String> con1 = (s) -> {
    System.out.println(s);
  };
  con1.accept("一个是听得人当真了,一个是说的人当真了");
  
  System.out.println("****************");
  
  Consumer<String> con2 = s -> {
    System.out.println(s);
  };
  con2.accept("一个是听得人当真了,一个是说的人当真了");
}
```

### 语法格式5 lambda需要两个或以上的参数,多条执行语句,并且有返回值

```java
public void test6() {
  
  Comparator<Integer> com1 = new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
      System.out.println(o1);
      System.out.println(o2);
      return o1.compareTo(o2);
    }
  };
  System.out.println(com1.compare(12, 31));

  System.out.println("****************");

  Comparator<Integer> com2 = (o1, o2) -> {
    System.out.println(o1);
    System.out.println(o2);
    return o1.compareTo(o2);
  };
  System.out.println(com2.compare(393, 55));
}
```

### 语法格式6：当Lambda体只有一条语句时，return与大括号若有，都可以省略

```java
public void test7() {
  Comparator<Integer> com1 = (o1, o2) -> {
    return o1.compareTo(o2);
  };
  System.out.println(com1.compare(393, 55));
  
  System.out.println("****************");

  Comparator<Integer> com2 = (o1, o2) -> o1.compareTo(o2);
  System.out.println(com2.compare(393, 595));
}
```

# 函数式接口 Functional Interface

- ==只包含一个抽象方法的接口==，称为函数式接口。
- 只有给函数式接口提供实现类的对象时，我们才可以使用lambda表达式。
- 你可以通过 Lambda 表达式来创建该接口的对象。（若Lambda 表达式抛出一个受检异常（即：非运行时异常），那么该异常需要在目标接口的抽象方法上进行声明）。
- 我们可以在一个接口上使用 @Functionallnterface 注解，这样做可以检查它是否是一个函数式接口。同时 javadoc 也会包含一条声明，说明这个接口是一个函数式接口。
- 在==java.util.function==包下定义了Java 8的丰富的函数式接口

> 如何理解函数式接口
>
> - Javal从诞生日起就是一直倡导“一切皆对象”，在Java里面面向对象（00P）编程是一切。但是随着python、scala等语言的兴起和新技术的挑战，Java不得不做出调整以便支持更加广泛的技术要求，也即java不但可以支持OOP还可以支持OOF（面向函数编程）
> - 在函数式编程语言当中，函数被当做一等公民对待。在将函数作为一等公民的编程语言中，Lambda表达式的类型是函数。但是在Java8中，有所不同。在Java8中，Lambda表达式是对象，而不是函数，它们必须依附于一类特别的对象类型——函数式接口。
> - 简单的说，在Java8中，Lambda表达式就是一个函数式接口的实例。这就是Lambda表达式和函数式接口的关系。也就是说，只要一个对象是函数式接口的实例，那么该对象就可以用Lambda表达式来表示。
> - 所以以前用 匿名实现类 表示的现在都可以用Lambda表达式来写。

| 函数式接口              | 参数类型 | 返回类型 | 用途                                                         |
| ----------------------- | -------- | -------- | ------------------------------------------------------------ |
| Consumer<T> 消费型接口  | T        | void     | 对类型为T的对象应用操作,包含方法:  void accept(T t)          |
| Supplier<T> 供给型接口  | 无       | T        | 返回类型为T的对象,包含方法:  T get()                         |
| Function<T,R>函数型接口 | T        | R        | 对类型为T的对象应用操作,并返回结果,结果是R类型的对象.  包含方法: R apply(T t) |
| Predicate<T>断定型接口  | T        | boolean  | 确定类型为T的对象是否满足某约束,并返回boolean值. 包含方法: boolean test(T t) |

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240308下午65355093.png" alt="image-20240308下午65355093" style="zoom:50%;" />

Java内置的4大核心函数式接口

### 消费型接口 Consumer<T> void accept(T t)

```java
public void test1() {
  happyTime(500.0, new Consumer<>() {
    @Override
    public void accept(Double o) {
      System.out.println("价格为 " + o);
    }
  });

  System.out.println("********************");
  //全面的写法
	this.<Double>happyTime(400.0, money -> System.out.println("价格为:" + money));
  //菱形表达式类型熔断 简写
  happyTime(400.0, money -> System.out.println("价格为:" + money));
}

//泛型方法
public <T> void happyTime(T money, Consumer<T> con) {
  con.accept(money);
}
```

 * 供给型接口 Supplier<T> T get()

 * 函数型接口 Function<T,R> R apply(T t)
 * 断定型接口 Predicate<T> boolean test(T t)

### 过滤器-断定应用实例

```java
public void test2() {
  List<String> list = Arrays.asList("北京", "天津", "东京", "西京", "普京");
  List<String> list1 = filterString(list, new Predicate<>() {
    @Override
    public boolean test(String s) {
      return s.contains("京");
    }
  });
  System.out.println(list1);

  System.out.println("****************");

  List<String> list2 = filterString(list,s -> s.contains("京"));
  System.out.println(list2);
}

/**
     * @param list
     * @param pre
     * @Author YannLau
     * @Description 根据给定的规则, 过滤集合中的T.此规则由Predicate的方法决定
     * @Date 2024/3/8
**/
public <T> List<T> filterString(List<T> list, Predicate<T> pre) {
  ArrayList<T> filterList = new ArrayList<>();
  for (T s : list) {
    if (pre.test(s)) {
      filterList.add(s);
    }
  }
  return filterList;
}
```

# 方法引用

> - 当要传递给Lambda体的操作，==已经有实现的方法了,一般是静态方法==，可以使用方法引用！
> - 方法引用可以看做是Lambda表达式深层次的表达。换句话说，方法引用就是Lambda表达式，也就是函数式接口的一个实例，通过方法的名字来指向一个方法，可以认为Lambda表达式的一个语法糖。
> - 要求：==实现接口的抽象方法的参数列表和返回值类型，必须与方法引用的方法的参数列表和返回值类型保持一致==(适用于情况1和情况2)
> - 格式：使用操作符“：”将类（或对象）与 方法名分隔开来。
> - 如下三种主要使用情况：
>   ＞对象 :: 实例方法名
>   ＞类 :: 静态方法名
>   ＞类 :: 实例方法名

 * 1.使用情境：当要传递给Lambda体的操作，已经有实现的方法了，可以使用方法引用！
 * 2.方法引用，本质上就是Lambda表达式，而Lambda表达式作为函数式接口的实例。所以
 * 方法引用，也是函数式接口的实例。
 * 3.使用格式：类(或对象)::方法名
 * 4.具体分为如下的三种情况：
   * 情况一 对象::非静态方法
   * 情况二 类::静态方法
   * 情况三 类::非静态方法
 * 5.方法引用使用的要求: 要求接口中的抽象方法的形参列表和返回值类型与方法引用的方法的形参列表和返回值类型相同!(适用于情况1和情况2)

### 情况一 对象::实例方法

要求：
函数式接口中的抽象方法a与其内部实现时调用的对象的某个方法b的形参列表和返回值类型都相同（或一致）。
此时，苛以考虑使用方法b实现对方法a的替换、覆盖。此替换或覆盖即为方法引用。

```java
//Consumer中的 void accept(T t)
public void test1() {
  Consumer<String> con1 = str -> System.out.println(str);
  con1.accept("北京");

  System.out.println("********************");

  PrintStream out = System.out;
  Consumer<String> con2 = out::println;
  con2.accept("beijing");
}

//Supplier中的 T get()
public void test2() {
  Employee lyp = new Employee("lyp", 1000000, 1);

  Supplier<String> con1 = new Supplier<String>() {
    @Override
    public String get() {
      return lyp.getName();
    }
  };
  System.out.println(con1.get());
  
  System.out.println("**************");
  
  Supplier<String> con2 = () -> lyp.getName();
  System.out.println(con2.get());
  
  System.out.println("**************");
  
  Supplier<String> con3 = lyp::getName;
  System.out.println(con3.get());
}
```

### 情况二  类::静态方法

要求：函数式接口中的抽象方法a与其内部实现时调用的类的某个静态方法b的形参列表和返回值类型都相同（或一致）。
此时，可以考虑使用方法b实现对方法a的替换、覆盖。此替换或覆盖即为方法引用。
注意：此方法b是静态的方法，需要类调用。

```java
//Comparator中的int compare(T t1,T t2)
public void test3() {

  Comparator<Integer> com = new Comparator<Integer>() {
    @Override
    public int compare(Integer o1, Integer o2) {
      return Integer.compare(o1, o2);
    }
  };
  System.out.println(com.compare(33, 22));

  System.out.println("****************");

  Comparator<Integer> com1 = (t1, t2) -> Integer.compare(t1, t2);
  System.out.println(com1.compare(12, 21));

  System.out.println("****************");

  Comparator<Integer> com2 = Integer::compare;
  System.out.println(com2.compare(11, 1));
}

//Function中的 R apply(T t)
public void test4() {
  Function<Double, Long> fun1 = new Function<Double, Long>() {
    @Override
    public Long apply(Double aDouble) {
      return Math.round(aDouble);
    }
  };
  System.out.println(fun1.apply(567.654));
  
  System.out.println("*********************");
  
  Function<Double, Long> fun2 = (d) -> Math.round(d);
  System.out.println(fun2.apply(2234.23));
  
  System.out.println("*********************");
  
  Function<Double, Long> fun3 = Math::round;
  System.out.println(fun3.apply(24324.234));
}
```

### 情况3  类::实例方法  (有难度)

要重写的方法有 n 个参数 = 1 + （n-1）

我们调用的非静态实例方法有 n-1 个参数

重写方法的 第 1 个参数作为 我们非静态实例方法 的调用者出现了。

终级理解：实例方法 作为桥梁 能够连接 接口中重写方法的 形参。即可以替换



要求：函数式接口中的抽象方法a与其内部实现时调用的对象的某个方法b的返回值类型相同。
同时，抽象方法a中有n个参数，方法b中有n-1个参数，且抽象方法a的第1个参数作为方法b的调用者，且抽象方法a
的后n-1个参数与方法b的n-1个参数的类型相同（或者一致）。

注意：此方法b是非静态的方法，需要对象调用。但是形式上，写出对象a所属的类

```java
/* Comparator 中的 int compare(T t1,T t2)
 * String 中的 int t1.compareTo(t2) -- t1是作为调用者出现的
 compareTo 是一个实例方法。
 compare的参数要求是一样的 且 是 (T a,T b)
 String的实例方法 compareTo 是 T.compareTo(T)
 */

public void test5() {

  Comparator<String> com = new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
      return o1.compareTo(o2);
    }
  };
  System.out.println(com.compare("ada", "daw"));

  System.out.println("**********************");

  Comparator<String> com1 = (s1, s2) -> s1.compareTo(s2);
  System.out.println(com1.compare("sf", "r3w"));

  System.out.println("**********************");

  Comparator<String> con2 = String::compareTo;
  System.out.println(con2.compare("rvb", "DVRG"));
}

//Description BiPredicate中的 boolean test(T t1,T t2);

public void test6() {
  BiPredicate<String, String> pre = new BiPredicate<String, String>() {
    @Override
    public boolean test(String s, String s2) {
      return s.equals(s2);
    }
  };
  System.out.println(pre.test("BBC", "BBC"));

  System.out.println("**********************");

  BiPredicate<String, String> pre1 = (s1, s2) -> s1.equals(s2);
  System.out.println(pre1.test("dad", "dawd"));

  System.out.println("**********************");
  
  boolean Bipredicate(String s,String s);
  boolean stringObj.equals(String stringObj);

  BiPredicate<String, String> pre2 = String::equals;
  System.out.println(pre2.test("ad", "awd"));
}

//Description Function中的 R apply(T t)
//Employee 中的 String getName();
public void test7() {
  Employee lyp = new Employee("lyp", 223.12, 123);
  Function<Employee, String> fun1 = new Function<Employee, String>() {
    @Override
    public String apply(Employee employee) {
      return employee.getName();
    }
  };
  System.out.println(fun1.apply(lyp));
  
  System.out.println("******************");
  
  Function<Employee,String> fun2 = (emp) ->emp.getName();
  System.out.println(fun2.apply(lyp));
  
  System.out.println("*****************");
   
  String apply(Employee emp);
  String employeeInstance.getName();
  
  Function<Employee,String> fun3 = Employee::getName;
  System.out.println(fun3.apply(lyp));
}
```

# 构造器引用

构造器引用 和 数组引用
 * 1. 构造器引用
 * 和方法引用类似,函数式接口的抽象方法的形参列表和构造器的形参列表一致
 * 抽象方法的返回值类型即为构造器所属的类的类型
 * 2. 数组引用
 * 大家可以把数组看做是一个特殊的类,则写法与构造器引用一致.

#### 构造器引用 Supplier中的 T get()
   * Employee 的空参构造器 Employee()

```java
public void test1() {
  
  Supplier<Employee> sup1 = new Supplier<Employee>() {
    @Override
    public Employee get() {
      return new Employee();
    }
  };
  sup1.get();
  
  System.out.println("***************");
  
  Supplier<Employee> sup2 = () -> new Employee();
  sup2.get();
  
  System.out.println("***************");
  
  Supplier<Employee> sup3 = Employee::new;
  sup3.get();
}
```

### 构造器引用 Function 中的 R apply(T t)

- Employee中的 public Employee(long id)

```java
public void test2() {
  
  Function<Integer, Employee> fun1 = new Function<Integer, Employee>() {
    @Override
    public Employee apply(Integer integer) {
      return new Employee(integer);
    }
  };
  fun1.apply(456);
  
  System.out.println("***************");
  
  Function<Integer, Employee> fun2 = (i) -> new Employee(i);
  fun2.apply(345);
  
  System.out.println("****************");
  
  Function<Integer, Employee> fun3 = Employee::new;
  fun3.apply(123);
}
```

### BiFunction中的 R apply(T t,U u)
* Employee中的 public Employee(long id,String name)

```java
public void test3() {
  
  BiFunction<String, Integer, Employee> bifun1 = new BiFunction<String, Integer, Employee>() {
    @Override
    public Employee apply(String s, Integer integer) {
      return new Employee(s, integer);
    }
  };
  bifun1.apply("Jack", 2333);
  
  System.out.println("****************");
  
  BiFunction<String, Integer, Employee> bifun2 = (s, id) -> new Employee(s, id);
  bifun2.apply("Tom", 3444);
  
  System.out.println("*****************");
  
  BiFunction<String, Integer, Employee> bifun3 = Employee::new;
  bifun3.apply("John", 22);
}
```

### 数组引用

```java
public void test4() {
  
  Function<Integer,String[]> fun0 = new Function<Integer, String[]>() {
    @Override
    public String[] apply(Integer integer) {
      return new String[integer];
    }
  };
  System.out.println(Arrays.toString(fun0.apply(3)));
  
  System.out.println("*************");
  
  Function<Integer,String[]> func1 = length -> new String[length];
  System.out.println(Arrays.toString(func1.apply(4)));
  
  System.out.println("***********");
  
  Function<Integer,String[]> func2 = String[]::new;
  System.out.println(Arrays.toString(func2.apply(5)));
}
```

