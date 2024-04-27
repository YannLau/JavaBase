# 强大的Stream API

[toc]

## 说明

> • Java8中有两大最为重要的改变。第一个是Lambda 表达式：另外一个则是 Stream API。
> • Stream API （java.util.stream）把真正的函数式编程风格引入到Java中。这是目前为止对Java类库最好的补充，因为Stream API可以极大提供Java程序员的生产力，让程序员写出高效率、干净、简洁的代码。
> • Stream 是 Java8 中处理集合的关键抽象概念，它可以指定你希望对集合进
> 行的操作，可以执行非常复杂的查找、过滤和映射数据等操作。***使用Stream API 对集合数据进行操作，就类似于使用 SQL 执行的数据库查询。***也可以使用 Stream API 来 并行 执行操作。简言之，Stream API 提供了一种高效且易于使用的处理数据的方式。

## 为什么要使用Stream API

> • 实际开发中，项目中多数数据源都来自于Mysql，Oracle等关系型数据库。但现在数据源可以更多了，有MongDB，Radis等，而这些NoSQL非关系型数据库的数据就需要Java层面去处理。

## 什么是Stream

是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。

“集合讲的是数据的存储（面向内存的），Stream讲的是计算（排序、查找、过滤、映射、遍历等），面向CPU的！”

Stream API之于集合，类似于SQL之于数据表的查询。

• Stream 和 Collection 集合的区别：
Collection 是一种静态的内存数据结构，而 Stream 是有关计算的。前者是主要面向内存，存储在内存中，后者主要是面向 CPU，通过 CPU 实现计算。

> 注意：
> ①Stream 自己不会存储元素。
> ②Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
> ③Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行，即一旦执行终止操作，就执行中间操作链，并产生结果。
> ④Strean一旦执行了终止操作，就不能再调用其它中间操作或终止操作了。

## Stream的操作的三个步骤

> • 1- 创建 Stream Stream的实例化
> 一个数据源（如：集合、数组），获取一个流
> • 2- 一系列中间操作
> 一个中间操作链，对数据源的数据进行处理
> • 3- 执行终止操作（终端操作）
> 一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用

![image-20240309下午23036171](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240309下午23036171.png)

## 创建

### 创建Stream方式一 通过集合

```java
import com.lyp.java8.functional_interface.Employee;
import com.lyp.java8.functional_interface.EmployeeData;
import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.List;
import java.util.stream.IntStream;
import java.util.stream.Stream;

/**
 * @author YannLau
 * @version 1.0
 * @program java8_NewProperties
 * @ClassName StreamAPITest
 * @ClassPath com.lyp.java8.streamAPITest.StreamAPITest
 * @create 2024-03-09 14:34
 * @description 1.Stream关注的是对数据的运算，与CPU打交道
 * 集合关注的是数据的存储，与内存打交道
 * <p>
 * 2.
 * ①Stream 自己不会存储元素。
 * ②Stream 不会改变源对象。相反，他们会返回一个持有结果的新stream。
 * ③Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行
 * <p>
 * 3.Stream 执行流程
 * ① Stream的实例化
 * ② 一系列的中间操作（过滤、映射、⋯.）
 * ③ 终止操作
 * <p>
 * 4. 说明：
 * 4.1 一个中间操作链，对数据源的数据进行处理
 * 4.2 一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用
 */
public class StreamAPITest {
    /**
     * @Author YannLau
     * @Description 创建Stream方式一 通过集合
     * @Date 2024/3/9
     **/
    @Test
    public void test1() {
        List<Employee> employee = EmployeeData.getEmployee();
        //default Stream<E> stream() : 返回一个顺序流
        Stream<Employee> stream = employee.stream();
        //default Stream<E> parallelStream() : 返回一个并行流
        Stream<Employee> employeeStream = employee.parallelStream();
    }
```
### 创建Stream方式二 通过数组
```java
    /**
     * @Author YannLau
     * @Description 创建Stream方式二 通过数组
     * @Date 2024/3/9
     * @Time 3:04PM
     */
    @Test
    public void test2() {
        int[] arr = {1, 2, 3, 4, 5, 6};
        //通过Arrays类的static <T> Stream<T> stream(T[] array):返回一个流
        IntStream stream = Arrays.stream(arr);
      
        Employee e1 = new Employee("Tom", 1001);
        Employee e2 = new Employee("Jerry", 1002);
        Employee[] arr1 = new Employee[]{e1, e2};
        Stream<Employee> stream1 = Arrays.stream(arr1);
    }
```
### 通过Stream的of()方法

```java
    /**
     * @Author YannLau
     * @Description 通过Stream的of()方法
     * @Date 2024/3/9
     * @Time 3:13 PM
     */
    @Test
    public void test3() {
        Stream<Integer> integerStream = Stream.of(1, 2, 3, 4, 5, 6);
        Stream<Object> array = Stream.of(EmployeeData.getEmployee().toArray());
    }
```
### 创建Stream方式4 创建无限流
```java
  /**
       * @Author YannLau
       * @Description 创建Stream方式4 创建无限流
       * @Date 2024/3/9
       * @Time 3:19 PM
       */
  @Test
  public void test4() {
    //可以使用静态方法 Stream.iterate() 和 Stream.generate()，创建无限流。
    //•迭代
    //public static <T> Stream<T> iterate(final T seed, final UnaryOperator<T> f)
    //遍历前十个种子
    Stream<Integer> iterate = Stream.iterate(0, t -> t + 2);
    iterate.limit(10).forEach(System.out::println);

    //•生成
    //public static <T> Stream<T> generate(Supplier<T> s)
    Stream.generate(Math::random).limit(10).forEach(System.out::println);
  }
}
```

## 中间操作

> 多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”。

### 1-筛选与切片

```java
import com.lyp.java8.functional_interface.Employee;
import com.lyp.java8.functional_interface.EmployeeData;
import org.junit.jupiter.api.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Stream;

/**
 * @author YannLau
 * @version 1.0
 * @program java8_NewProperties
 * @ClassName StreamAPITest1
 * @ClassPath com.lyp.java8.streamAPITest.StreamAPITest1
 * @create 2024-03-09 15:58
 * @description 演示Stream的中间操作
 */
public class StreamAPITest1 {

    //1-筛选与切片
    @Test
    public void test1() {
        List<Employee> list = EmployeeData.getEmployee();
        //filter(Predicate p) 接收 Lambda，从流中排除某些元素
        Stream<Employee> stream = list.stream();
        // 练习:查询员工表中薪资大于7000的员工信息
        stream.filter(emp -> emp.getSalary() > 7000).forEach(System.out::println);
      //foreach 相当于是一个终止操作
      //后续不能再对stream操作了，除非再建一个新的
      
        System.out.println("*********************");
      
        //limit(long maxSize) 截断流，使其元素不超过给定数量
        //这里需要重新生成一个Stream流
        list.stream().limit(3).forEach(System.out::println);
      
        System.out.println("*********************");
      
        //skip(long n) 跳过元素，返回一个扔掉了前n个元素的流。若流中元素不足n个，则返回一个空流。与 limit(n)互补
        list.stream().skip(3).forEach(System.out::println);
      
      System.out.println("*********************");
      
        //distinct() 去重,通过流所生成元素的 hashCode()和 equals()去除重复元素
        list.add(new Employee("刘华强", 12.2, 1001, 100));
        list.add(new Employee("刘华强", 12.2, 1001, 100));
        list.add(new Employee("刘华强", 12.2, 1001, 100));
        list.add(new Employee("刘华强", 12.2, 1001, 100));
        list.add(new Employee("刘华强", 12.2, 1001, 100));
        list.stream().distinct().forEach(System.out::println);
    }
```
### 2-映射
```java
    //2-映射
    //map(Function f)
    //接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
    //map ToDouble(ToDoubleFunction f)
    //接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 DoubleStream。
    //mapToInt(ToIntFunction f)
    //接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 IntStream。
    //mapToLong(ToLongFunction f)
    //接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 LongStream。
    //flatMap(Function f)
    //接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
    @Test
    public void test2() {
      
        //map(Function f)
        //接收一个函数作为参数,将元素转换成其他形式或提取信息,该函数会被应用到每个元素上,并将其映射成一个新的元素.
        List<String> list = Arrays.asList("aa", "bb", "cc", "dd");
        list.stream().map(String::toUpperCase).forEach(System.out::println);
      
        //练习1 获取员工姓名长度大于3的员工的姓名
        List<Employee> employee = EmployeeData.getEmployee();
        Stream<String> stringStream = employee.stream().map(Employee::getName);
      
        stringStream.filter(name -> name.length() > 3).forEach(System.out::println);
      
        System.out.println("****************");
      
        //练习2 模拟flatMap
        Stream<Stream<Character>> streamStream = list.stream().map(StreamAPITest1::fromStringToStream);
      
        streamStream.forEach(s -> {
            s.forEach(System.out::println);
        });

        System.out.println("***********flatMap示例**********");
        //flatMap(Function f)
        //接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
        list.stream().flatMap(StreamAPITest1::fromStringToStream).forEach(System.out::println);
    }

    /**
     * @param str 目标字符串
     * @return Stream<Character>
     * @Author YannLau
     * @Description 将字符串中的多个字符构成的集合转换为对应的Stream实例
     * @Date 2024/3/9
     * @Time 9:31 PM
     */
    public static Stream<Character> fromStringToStream(String str) {
        ArrayList<Character> list = new ArrayList<>();
        for (Character c : str.toCharArray()) {
            list.add(c);
        }
        return list.stream();
    }
```
### 3-排序
```java
    //3-排序
    // 1. sorted()  产生一个新流,其中按照自然顺序排序
    // 2. sorted(Comparator com) 产生一个新流,其中按照比较器顺序排序
    @Test
    public void test3() {
      
        //自然排序
        List<Integer> list = Arrays.asList(12, 4, 432, 4, 423, 4235, 45, 435, 423, 454, 35, 66, 456, 4, 56);
        list.stream().sorted().forEach(System.out::println);
      
        System.out.println("************************");
      
        list.stream().sorted(new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        }).forEach(System.out::println);
      
        System.out.println("**************************");
      
        list.stream().sorted((o1, o2) -> o2 - o1).forEach(System.out::println);

        //抛异常.原因 Employee没有实现Comparable接口
        //List<Employee> employees = EmployeeData.getEmployee();
        //employee.stream().sorted().forEach(System.out::println);

        //sorted(Comparator com)  -- 定制排序
        List<Employee> employees = EmployeeData.getEmployee();
        employees.stream().sorted((e1, e2) -> {
            int i = Integer.compare(e1.getAge(), e2.getAge());
            if (i != 0) {
                return i;
            } else {
                return Double.compare(e1.getSalary(),e2.getSalary());
            }
        }).forEach(System.out::println);
    }
}
```

## 终止操作

> • 终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例
>如：List、Integer，甚至是 void。
> • 流进行了终止操作后，不能再次使用。

> 收集
>
> 方法  collest(Collector c)
>
> - 描述
>   - 将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法.
> - Collector 接口中方法的实现决定了如何对流执行收集的操作（如收集到 List、Set、
>   Мар).
> - 另外，Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例，
>   具体方法与实例如下表：

![image-20240424下午42550170](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240424下午42550170.png)

![image-20240424下午42602958](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240424下午42602958.png)

```java
import com.lyp.java8.functional_interface.Employee;
import com.lyp.java8.functional_interface.EmployeeData;
import org.junit.jupiter.api.Test;

import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;

/**
* @author YannLau
* @version 1.0
* @program java8_NewProperties
* @ClassName StreamAPITest2
* @ClassPath com.lyp.java8.streamAPITest.StreamAPITest2
* @create 2024-03-09 22:13
* @description 测试Stream的终止操作
*/
public class StreamAPITest2 {
  @Test
  public void test1() {

    List<Employee> list = EmployeeData.getEmployee();
    //allMatch(Predicate P)一检查是否匹配所有元素。
    // 练习…是否所有的员工的年龄都大于18
    System.out.println("是否所有的员工的年龄都大于18");
    System.out.println(list.stream().allMatch(e -> e.getAge() > 18));

    //anyMatch(Predicate P)一检查是否至少匹配一个元素
    // 练习，是否存在员工的工资大于 10000
    System.out.println("是否存在员工的工资大于 10000");
    System.out.println(list.stream().anyMatch(e -> e.getSalary() > 10000));

    //noneMatch(Predicate P)一检查是否没有匹配的元素。
    // 练习，是否存在员工姓"雷"？
    System.out.println("是否不存在员工姓\"雷\"？");
    System.out.println(list.stream().noneMatch(e -> e.getName().charAt(0) == '雷'));

    //findFirst—返回第一个元素
    System.out.println("返回第一个元素");
    System.out.println(list.stream().findFirst());

    //findAny一返回当前流中的任意元素
    System.out.println("返回当前流中的任意元素");
    System.out.println(list.stream().findAny());
    System.out.println(list.parallelStream().findAny());

    //count一返回流中元素的总个数
    System.out.println("返回流中工资大于5000的元素的总个数");
    System.out.println(list.stream().filter(e -> e.getSalary() > 5000).count());

    //max(Comparator c)—返回流中最大值的元素
    System.out.println("返回流中最大值工资");
    System.out.println(list.stream().map(Employee::getSalary).max(Double::compare));

    //练习：返回最高的工资的员工;
    System.out.println("返回最高的工资的员工");
    Optional<Employee> e = list.stream().max((o1, o2) -> Double.compare(o1.getSalary(), o2.getSalary()));
    System.out.println(e);

    //min(Comparator c)一返回流中最小值
    //练习：返回最低工资的员工
    System.out.println("返回最低工资的员工");
    System.out.println(list.stream().min((o1, o2) -> Double.compare(o1.getSalary(), o2.getSalary())));

    //forEach(Consumer c)一内部迭代
    System.out.println("内部迭代");
    list.stream().forEach(System.out::println);
    
    //针对于集合，jdk8中增加了一个遍历的方法
    list.foreach(System.out::println);
    
    //针对于List来说，遍历的方式：① 使用Iterator ② 增强for ③ 一般for ④ forEach()
  }

  //2-归约
  @Test
  public void test2() {
    List<Employee> list = EmployeeData.getEmployee();
    //reduce(T identity, BinaryOperator) 可以将流中元素反复结合起来,得到一个值.返回和
    // 练习1 计算1-10的自然数的和
    List<Integer> list1 = Arrays.asList(1, 23, 3, 4, 4, 4, 2, 5);
    Integer reduce = list1.stream().reduce(0, Integer::sum);    //这里的0是初始值
    System.out.println(reduce);

    // reduce(BinaryOperator)  -- 可以将流中的元素反复结合起来,得到一个值
    //返回Optional<T>
    //练习2 计算公司所有的员工工资的总和
    Stream<Double> doubleStream = list.stream().map(Employee::getSalary);
    //Optional<Double> reduce1 = doubleStream.reduce(Double::sum);
    Optional<Double> reduce1 = doubleStream.reduce((o1, o2) -> o1 + o2);
    System.out.println(reduce1);
  }

  @Test
  public void test3() {
    // collect(Collector c) 将流转换为其他形式.接收一个Collector接口的实现,
    // 用于给Stream中元素做汇总的方法.
    //练习1 查找工资大于6000的员工,结果返回一个list或者Set
    List<Employee> employees = EmployeeData.getEmployee();
    List<Employee> collect = employees.stream().filter(e -> e.getSalary() > 6000).collect(Collectors.toList());
    collect.forEach(System.out::println);
    Set<Employee> collect1 = employees.stream().filter(e -> e.getSalary() > 6000).collect(Collectors.toSet());
    collect1.forEach(System.out::println);
  }
}
```

