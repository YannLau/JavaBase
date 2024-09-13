[toc]

# 集百家所长

Java核心技术卷I 12ed 第八章泛型程序设计

[【Java必备知识】泛型擦除？不怕！这里有获取泛型信息的秘诀！1_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1wk4y1s7j2/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=9bfe1ca76c0d7c42a70bafd019e62999)

[(31 封私信 / 59 条消息) Java 不能实现真正泛型的原因是什么？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/28665443/answer/3340713988)



# 对 泛型1 的基础易错补充

- 概念补充

  类型变量：T K 擦除后为 Object

  类型变量限定 `<T extends Comparable>` `<K super List>` 擦除后为 Comparable List

  无限定通配符 `<?> `通配符不是类型变量，因此，不能在编写代码中使用“？”作为一种类型。 `? element; 非法`

>`<T,E,..>`中的T、E 应该被称为类型参数。
>
>泛型类型(<T,E,..>) 和 通配符类型(<?/T extends/super xxx>) 是两个不同的概念。
>
>泛型类型和通配符类型甚至可以混在一起使用。`comparing(Function<? super T, ? extends U> keyExtractor)`
>
>通配符限定 `? extends/super xx` 和 类型变量限定 `T extends/super xx`
>
>泛型方法调用的标准形式 `method.<type..>(Para..)`
>
>泛型方法和泛型类都会对传入类型进行检查，但是对于泛型方法，虽然son不满足T extends comparble<T>，但是它的父类father满足这个条件，会自动进行向上转换，把son转换为其父类然后继续调用。
>
>> 对于泛型方法，当你传入一个类型参数时，编译器会根据该类型参数的约束来进行检查。如果你传入的类型不满足约束条件，编译器会报错。但是在你提到的情况下，如果子类的类型不满足约束条件，但其父类的类型满足约束条件，编译器会允许这种情况发生，因为子类可以被向上转型为父类，所以编译器会把子类当作父类类型来处理。
>>
>> 但是需要注意的是，虽然编译器会允许这种情况发生，但是在运行时如果实际传入的对象类型不满足约束条件，可能会导致类型转换异常或者其他运行时异常。
>
>一个 类型变量T 或者 通配符？ 可以有多个限定 `<T extends comparable & Serializable, E extends HttpServlet>`

- 易错易混辨析

> List<List<String>> list1 = new ArrayList<ArrayList<String>>(); //是非法的，泛型不具备继承性
>
> ArrayList<List<String>> list1 = new ArrayList<ArrayList<String>>(); // 也是非法，泛型不具备继承性
>
> List<? extends List<String>> list1 = new ArrayList<ArrayList<String>>(); //是合法的,但是没法使用
>
> 当然 ArrayList<? extends List<String>> list1 = new ArrayList<ArrayList<String>>(); //更是合法的，但是也没法使用
>
> 这里我想说的是，？通配符是在变相实现泛型继承性的同时，还能允许原本类之间的继承性，List <- ArrayList
>
> 但是其实是有限制的，这个限制正是我在 泛型1 中的结尾所写的我个人的迷思。
>
> 我们知道 通配符? 的出现是Java设计者为了弥补 保证泛型安全性所带来的恼人的限制 所作出的灵活解决方案。

# 泛型代码和虚拟机

对于Java泛型的转换，需要记住：

1. 虚拟机中没有泛型，所有对象都属于普通类。
2. 所有的类型参数都会被替换为它们的限定类型
3. 会合成桥方法来保持多态。（弥补类型擦除给多态实现带来的困扰）
4. 为了保证类型的安全性，必要时会插入强制类型转换。

> - 类型擦除
>
> 无论何时定义一个泛型类型，都会自动提供一个相应的原始类型(raw type)。这个原始类型的名字就是去掉类型参数后的泛型类型名。类型变量会被擦除，并替换为其限定类型（限定类型指的是 T extends xx 中的类型 xx）。对于无限定的变量则替换为Object。
>
> 对于有多个限定类型的(T extends AA&BB&CC&..)，首先，类要写在接口前面。其次，会用第一个进行替换。如果都是接口，就无所谓顺序。
>
> 你可能想知道，`class Interval<T extends Comparator & Serializable>`限定切换为`class Interval<T extends Serializable & Comparator>`会发生什么。如果这样做，原始类型会用Serializable替换T，而且编译器会在必要时插入转换为Comparator的强制类型转换。==为了提高效率应该将标记(tagging)接口（即没有方法的接口）放在限定列表的末尾。==
>
> - 转换泛型表达式
>
> 编译器将自动插入强制转换来实现泛型，省去了以前的手动的强转。（并且在编译阶段可以进行检查，防止放入错误类型，所以说双重作用）
>
> - 转换泛型方法
>
> `(类型擦除会导致Override的一些方法变得不再是同一个方法，影响多态的调用) 可以通过桥方法实现多态。桥方法实现真正意义上的@Override！(桥方法是定义在子类中的，实现对父泛型类类型擦除后的方法进行重写，从而调用本类中正确的方法，实现多态)`
>
> <img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726207830105.png" alt="QQ_1726207830105" style="zoom:50%;" />

# 限制与局限性

1. 不能用基本数据类型(int)实例化类型参数(T)。
2. 运行时类型查询(instanceof)只适用于原始类型(raw List) `aobj instanceof List<String> Error!`
3. 不能创建参数化类型的数组。`var table = new Pair<String>[10]; Error!!! `但是可以声明
4. 不能实例化类型变量 `T t = new T(); Error!`
5. 不能构造泛型数组 `new T[10]; Error!`
6. 泛型类的静态上下文中类型变量无效，不能在静态字段或方法中引用类型变量。应该把静态方法做成泛型方法。
7. Varargs警告
   <img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726208085750.png" alt="QQ_1726208085750" style="zoom:50%;" />
8. 既不能抛出也不能捕获泛型类的对象。实际上，泛型类扩展Throwable 甚至都是不合法的。
9. 注意擦除后的冲突



# 为什么说Java没有实现真正的泛型

由于类型擦除，导致无法实现方法的重载。真正意义上的泛型不会导致下面的问题。

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240429上午100859006.png" alt="image-20240429上午100859006" style="zoom: 50%;" />

```java
public void test() {

  List<Integer> list1 = new ArrayList<>();

  List list2 = list1; //raw List 指向 List<Integer>

  list1.add(1); // 此时 list=[1]

  list2.add("Hello"); //此时 list=[1,"Hello"]

  //list1.add("World"); Error 不能通过编译器检查
  System.out.println(list1.get(1)); // 我用使用了泛型的引用去取出 原始类型加入的"Hello",可以打印出 Hello
  System.out.println(list2.get(0));  // 同样可以打印出 1

  Object s = list1.get(1); //一切正常
  System.out.println(s.getClass()); //一切正常 打印出 String类型

  System.out.println(list1.get(1).getClass()); // Error!!! 发生运行时的类强转异常
  // class java.lang.String cannot be cast to class java.lang.Integer
  // 应该是泛型对类型进行了强转,但是为什么取出来的时候没有发生强转异常的呢?
  System.out.println(list2.get(0).getClass()); // 没有异常,得到 class java.lang.Integer
}


在这个代码示例中，你的 list1 使用了泛型 List<Integer>，而 list2 是一个原始类型的 List，这引起了一些有趣的运行时行为。让我们逐步解释为什么会出现这个运行时异常。
  
代码分析
泛型擦除和原始类型：
当你创建了 List<Integer> list1 时，编译器在编译时知道 list1 是一个包含 Integer 类型的列表。
但是你将 list1 赋值给 List list2，也就是一个原始类型（raw type）列表。在 Java 中，泛型是通过擦除机制来实现的，即在运行时，泛型的类型信息不会保留，编译器会将泛型的类型擦除。因此，在运行时，list1 和 list2 其实是同一个对象，但它们在编译时有不同的类型约束。

添加元素：

java
复制代码
list1.add(1); // 通过 list1 向列表中添加了一个 Integer 1
list2.add("Hello"); // 通过 list2 向同一个列表中添加了一个 String "Hello"
list1 和 list2 是同一个对象，所以操作一个列表时，另一个也会受到影响。在运行时，这两个操作都没有问题，因为 List 是原始类型，并没有进行类型检查。

取元素： 当你通过 list1.get(1) 取出 "Hello" 时，虽然 list1 在编译时被认为是 List<Integer>，但实际上 "Hello" 是一个 String，在运行时并没有立即发生异常。你可以打印 "Hello"，因为取出的对象仍然是 String。

为什么在 System.out.println(list1.get(1).getClass()); 出现了运行时异常： 关键在于 Java 泛型在编译时的类型擦除和运行时的类型转换。

编译时：list1 是一个 List<Integer>，编译器会假定从 list1 中获取的元素都是 Integer 类型。你可以通过 System.out.println(list1.get(1)) 正常输出 "Hello"，因为编译器没有对这个操作进行检查。
运行时：在运行时，list1.get(1) 实际上返回了一个 String 对象 ("Hello")，但由于 list1 是泛型的 List<Integer>，编译器隐式地在取值时进行了类型转换（强转为 Integer），这导致了类型转换异常。
异常发生的原因是，编译器认为 list1 只能包含 Integer，所以在运行时试图将 "Hello" 作为 Integer 返回给你。这时候会抛出 ClassCastException，因为 "Hello" 是 String 类型，不能被强制转换为 Integer。

运行时异常的根源
泛型类型擦除导致 list1 和 list2 其实是同一个对象，但 list1 在编译时被假定为只能包含 Integer。
当你通过 list1.get(1) 取出 String 类型的 "Hello" 时，编译器尝试将它强制转换为 Integer，这是不合法的操作，因此发生了 ClassCastException。
总结
在 System.out.println(list1.get(1).getClass()); 处发生异常，是因为泛型的类型擦除机制导致在运行时尝试将 String 强制转换为 Integer，这违反了类型安全规则。


您说：
那么为什么list1.get(1)不会引发异常？
ChatGPT 说：
ChatGPT
list1.get(1) 之所以不会立即引发异常，是因为在这个操作的过程中，Java 的泛型机制只是在编译时执行类型检查，而在运行时，类型信息已经被擦除，具体过程如下：

1. 编译时的行为：
在编译时，list1 被声明为 List<Integer>。这意味着编译器会认为 list1 中只能存储 Integer 类型的对象，并且编译器会假定任何从 list1 中取出来的对象都是 Integer 类型。
因为你通过 list1.get(1) 来获取第 1 个元素，编译器认为该元素是 Integer，并不会对具体的元素做进一步检查。在编译时，编译器并不会发现在 list1 中其实存储了一个 String 类型的 "Hello"，所以没有报错。
2. 运行时的行为（类型擦除）：
Java 的泛型在运行时通过类型擦除来实现，这意味着在运行时，List<Integer> 和 List（原始类型）都是同一个原始的 List 类型。运行时不保留 Integer 的类型信息，list1 和 list2 实际上是同一个对象。
当你执行 list1.get(1) 时，list1 中第 1 个元素是 "Hello"（一个 String）。此时，list1 实际上是一个包含了不同类型对象（Integer 和 String）的 List。
由于泛型类型在运行时已经擦除，Java 不会在 list1.get(1) 这一行立即进行类型转换。这意味着从 list1 中取出的元素会被认为是一个普通的 Object，所以直接执行 list1.get(1) 并不会立刻引发 ClassCastException。

3. 为什么后续会引发异常：
当你进一步调用 list1.get(1).getClass() 时，问题才显现出来：

list1.get(1) 返回的是 "Hello"，这是一个 String 类型的对象。
由于 list1 是 List<Integer>，编译器在取出元素后会隐式地将其视为 Integer，即尝试将 String 类型的 "Hello" 转换为 Integer 类型。这是 Java 泛型的类型转换机制之一。
但 "Hello" 不能转换为 Integer，因此在执行 list1.get(1).getClass() 时，就发生了运行时的 ClassCastException。
总结：
list1.get(1) 不会立即引发异常，因为在运行时，Java 的泛型类型信息已经被擦除。list1.get(1) 返回的是 Object，此时不会强制转换为 Integer，所以不会引发异常。
当你试图获取该对象的类型（通过 getClass() 或其他操作），系统才尝试将 String 强制转换为 Integer，这时类型不匹配，才会引发 ClassCastException。
```



# 泛型类型的继承规则

泛型不具备继承的特性，但是是用来泛型的类型仍然可能具有继承关系。

一般来讲，无论S和T有什么关系，`Pair<T>`和`Pair<S>`都没有任何关系。这里的根本原因，一个操作被不被允许，看的是编译类型。

```json
假如 Employee 是 Manager 的父类
Paris<Employee> buddies = new Pairs<Manager>(); //假如合法了，向上转型
buddies.setFirst(Aemployee); //会通过，根据编译类型，这个操作很正常。
这样不就破坏了泛型的初衷了，我们规定Pairs<Manager>就是为了只能放Manager，现在你放进去普通的员工了，算怎么回事？
这里和数组很不一样
数组可以做到
Employee[] buddies = new Manager[]{ceo,cfo};
这是因为数组有特别的保护，如果试图将一个底层员工存储到，buddies[0]时，虚拟机会抛出ArrayStoreException。
```

总是可以将参数化类型转化为一个原始类型。也就是说 `Pair`是任何`Pair<Type>`的父类。

那么这样的话，就会打破上面的例子中的限制。`Pair rawpair = new Pairs<Manager>(); `ok

`rawpair.setFirst(new File(...))` 竟然也ok，编译的时候只是一个警告。这看起来有点吓人。

不理解泛型底层机制的会认为，Manager不就已经让泛型T确认为Manager了，怎么还可以传入File类型呢？

看下面一个例子：

```java
public class PairTest {
  public static void main(String[] args) {

    A test = new A<File>(new File("a.txt"));
    test.setS("asd");
    System.out.println(test.getS());

  }
}
class A<T> {
  T s;

  public a(T s) {
    this.s = s;
  }

  public T getS() {
    return s;
  }

  public void setS(T s) {
    this.s = s;
  }
}

a test = new a<File>(new File("a.txt"));
// File o = test.getS(); 会报错，说提供的是Object，你用File来接受。可见类型被擦除了。
test.setS("asd");
System.out.println(test.getS());这三句话为啥不会导致错误？
```

 ChatGPT

在这个代码中，虽然在 PairTest 类中使用了泛型类 a<T>，<u>***但是在 main 方法中的代码并没有涉及到泛型类型的具体化***</u>。这意味着在 main 方法中，类型参数 T 被擦除了，编译器将它视为一个普通的未知类型（说明还是编译类型决定了是否生成泛型的字节码）。因此，test 对象在被创建时指定了类型参数 File，但是在 main 方法中，你可以给 test 对象设置任何类型的值，因为编译器会将它的类型视为 Object。所以，即使 setS 方法的参数类型是泛型类型 T，你可以传入任何类型的值。但是需要注意的是，在使用 getS 方法获取值时，它会返回一个 File 类型的对象，因为在创建 test 对象时指定了类型参数为 File。因此，System.out.println(test.getS()) 不会导致错误，因为 getS 方法的返回类型是 T，而在这个上下文中，T 被擦除为 File 类型。



<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240428下午83921865.png" alt="image-20240428下午83921865" style="zoom: 33%;" />

# `List、List<Object>、List<?>、List<String>`的区别

从继承关系上来说: 

```java
List<-List<?><-List<Object>
				↑←←←←←← List<String>

List<?> 表明编译器不知道所指向的List中的具体类型是什么，所以除了null什么类型的对象都不允许往里面放。
List 什么都可以往里面放。

  public void test() {
  Pair<?> p1 = new Pair<Integer>(1, 2);
  //p1.setFirst(3); //Error 需要的类型 capture of ? 提供的类型: int
  Object first = p1.getFirst();
  Object second = p1.getSecond();
  //Integer firstInteger = p1.getFirst(); //Error 需要的类型:Integer 提供的类型:capture of ?
}
```

# 通配符类型实现泛型类型的继承关系

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240428下午84522698.png" alt="image-20240428下午84522698" style="zoom: 33%;" />

`Pair<? extends Employee> pa = new Pairs<Manager>(cef,ceo);`这是可以的。但是会导致潜在的风险。

```java
Pair<? extends Employee>类型对应的方法体变为

  ? extend Employee  getFirst();
	void setFirst(? extends Employee); 

为了安全，直接不让调用setFirst方法了。 尽管你现在看着是 <? extends Employee> 但是运行起来的时候肯定是要有一个确定的类型的，可能是 Employee 也可能是 其他任何 Employee的子类，比如是 Boss 类，根本确定不了。比如 Pair<? extends Employee> = new Pair<Boss>(boos1,boos2);

此时，万一你setFirst进一个employee,我放行了，但是我的Piar中的两个元素实际是Boos类型。结果发生 Boos first = employee 这不就错了。所以直接不让你用了。但是getFirst方法，我不管我现在指向的是什么家伙，返回的first的类型一定是Employee或其子类，所以可以安全赋值给 Employee 类型的引用（向上转型）。

  所以 ? extends Employee 对应安全的访问器getter 不安全的修改器setter ，==此时想要调用setter并且不报错，只能传入null==

  相反， ? super Employee 对应安全的修改器 setter 不安全的访问器 getter
  
  method(? super Employee) 可以传入任何 Employee 以及其子类的对象
  
  ？super Employee get()  不能保证返回的类型是什么，只保证可以能赋值给Object类型的引用。
```

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240428下午91816805.png" alt="image-20240428下午91816805" style="zoom:33%;" />

直观的说，带有超类型限定的通配符，允许你写入一个泛型对象，而带有子类型限定的通配符，允许你读取一个泛型对象。

>   所以 ? extends Employee 对应安全的访问器getter 不安全的修改器 setter
>
>   相反，? super Employee 对应安全的修改器 setter 不安全的访问器 getter

# 通配符?和泛型类型组合的威力

Java核心技术卷中，定义了一个简单的泛型类：

```java
public class Pair<T>{
  private T first;
  private T second;
  public Pair(){
    first = null;
    second = null;
  }
  public Pair(T first, T second){
    this.first = first;
    this.second = second;
  }
  public T getFirst() {
    return first;
  }
  public void setFirst(T first) {
    this.first = first;
  }
  public T getSecond() {
    return second;
  }
  public void setSecond(T second) {
    this.second = second;
  }
}

class PairTest {
  public static void main(String[] args) {
    String[] words = {"Mary","had","a","little","lamb"};
    Pair<String> mm = ArrayAlg.minmax(words);
    System.out.println("min = " + mm.getFirst());
    System.out.println("max = " + mm.getSecond());
  }
}

class ArrayAlg {
  public static Pair<String> minmax(String[] a){
    if(a==null || a.length==0) return null;
    String min = a[0];
    String max = a[0];
    for(int i=1;i<a.length;i++){
      if(a[i].compareTo(min)<0) min = a[i];
      if(a[i].compareTo(max)>0) max = a[i];
    }
    return new Pair<>(min,max);
  }
}
```

上面的minmax的通用性很差，只能用来搞String[],我们其实希望他可以尽可能地适用于更多的类型。只要这个数组中装的元素是可以比较的，即实现了 Comparable 接口即可。于是有了通配符的帮忙

```java
public static <T extends Comparable<T>> Pair<T> minmax(T[] a){
  if(a==null || a.length==0) return null;
  T min = a[0];
  T max = a[0];
  for(int i=1;i<a.length;i++){
    if(a[i].compareTo(min)<0) min = a[i];
    if(a[i].compareTo(max)>0) max = a[i];
  }
  return new Pair<>(min,max);
}
//<T extends Comparable<T>> 这是一个泛型方法，且这里的通配符类型对 泛型的类型做了判定，所使用的泛型类型T必须要实现了Comparable<T>,这就保证了传进来的T[]中的元素可以调用compareTo方法。
```

> 这里书上提到了一个点，说`<T extends Comparable<T>`比只使用 `<T extends Comparable>`更彻底。
>
> 我当时认为 `<T extends Comparable>`  等价于 `<T extends Comparable<Object>>`
>
> 然后我就想泛型不具备继承性，完全没有关联的，那么怎么可以放在一起比较谁更彻底，也就是限制范围。
>
> 其实不对，ChatGPT说了：
>
> `<T extends Comparable>`确实等价于`<T extends Comparable<?>>`。它们两者都表示类型参数 `T`必须实现了`Comparable`接口，但是没有限定具体的泛型类型参数。这意味着`T`可以是实现了任意类型的`Comparable` 接口的类。
>
> 这两种写法在功能上是等价的，但是 `<T extends Comparable<?>>` 的形式更加明确地表达了对泛型类型的限定，因为它使用了通配符 `?` 来表示不确定的泛型类型参数。
>
> 这样我就对书中说法更加理解了。
>
> ```json
> public class PairTest {
>     public static void main(String[] args) {
>         String[] words = {"Mary","had","a","little","lamb"};
>         Pair<String> mm = ArrayAlg.minmax(words);
>         System.out.println("min = " + mm.getFirst());
>         System.out.println("max = " + mm.getSecond());
>         Comparable comparable1 = new compa(); //Warning!
>         Comparable<compa> comparable2 = new compa(); // Correct
>         Comparable<Object> comparable3 = new compa(); // Error!!!
>     }
> }
> class compa implements Comparable<compa>{
>     @Override
>     public int compareTo(compa o) {
>         return 0;
>     }
> }
> 
> ```

书中又提到，处理一个LocalDate对象数组时，我们会遇到一个新的问题。LocalDate实现了ChronoLocalDate，而ChronoLocalDate扩展了`Comparable<ChronoLocalDate>`。因此，LocalData实现的是`Comparable<ChronoLocalDate>`（通过implements/extends 间接实现）。在这种情况下，我们可以利用超类来解决。

使得，只要其超类实现过接口就可以，不必一定是自己实现的。

```java
public static <T extends Comparable<? super T>> Pair<T> minmax(T[] a){
  if(a==null || a.length==0) return null;
  T min = a[0];
  T max = a[0];
  for(int i=1;i<a.length;i++){
    if(a[i].compareTo(min)<0) min = a[i];
    if(a[i].compareTo(max)>0) max = a[i];
  }
  return new Pair<>(min,max);
}
```

现在 compareTo 方法变为：`int compareTo( （? super T） obj)`。这里的 obj 类型可能是T或者T超类的类型，那么就一定能接受 类型T的对象 或者 类型T的子类的对象。能确保传compareTo方法一定能传入所有的T[] a中元素。

同时，<T extends Comparable<? super T>> 又确保了T一定有compareTo方法。

完美！

# 超类型限定的另一个常见用法

作为一个函数式接口的参数类型。例如，Collection 接口有一个方法：

`default boolean removeIf(Predicate<? super E> filter)`

这个方法会删除所有的满足给定谓词条件的元素。例如，你不喜欢有奇怪散列码的员工，就可以这么删除他们：

`ArrayList<Employee> staff = ...`

`Predicate<Object> oddHashCode = obj -> obj.hashCode()%2!=0;`

`staff.removeIf(oddHashCode);`

你希望能够传入一个`Predicated<Object>`,而不是只是`Predicate<Employee>`。super通配符可以完成这个愿望。

# 无限定通配符

![QQ_1726211667637](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726211667637.png)

（==但可以调用setFirst(null) 合法！==）

甚至还可以使用根本无限定的通配符，例如 Pair<?>

实际上，原始的Pair和Pair<?> 有很大的不同

```java

原始的 Pair 和 Pair<?> 在使用上有一些重要的区别：

原始的 Pair：
原始的 Pair 没有指定类型参数，相当于使用了原始类型，即未加泛型限定的类型。
在原始的 Pair 中，不会进行类型检查，所有类型的对象都可以被接受和返回，但是可能会导致类型安全性问题。
例如，如果你将一个 String 对象传递给原始的 Pair，然后尝试使用 getFirst() 方法获取值，它将返回一个 Object 类型的对象，需要进行显式的类型转换。

和这里类似
    A test = new A<File>(new File("a.txt"));
    test.setS("asd");
    System.out.println(test.getS());

Pair<?>：
Pair<?> 是一个泛型类型，但是泛型类型参数是未知的。
在 Pair<?> 中，表示可以接受任何类型的 Pair 对象，但是对具体的类型参数没有限制。
使用通配符 <?> 表示对泛型类型参数的具体类型没有确定，但是在编译时会进行类型检查，从而提高了类型安全性。
例如，如果你使用 Pair<?>，则无法向其中添加任何元素，因为具体的类型参数是未知的，编译器无法确保添加的元素类型与实际的类型参数兼容。
```

# 为什么反射中常用Class<?> 而不直接用raw的Class

ChatGPT

在反射中常用`Class<?>`而不直接使用原始类型的`Class`，主要是为了保持类型安全性和灵活性。

1. **类型安全性：** 使用原始类型的`Class`意味着你不关心具体的类型信息，可以接受任意类型的`Class`对象。这可能会导致在运行时类型不匹配的错误，因为你无法在编译时对类型进行检查。而使用`Class<?>`表示未知类型，但仍然保留了泛型的信息，编译器会在编译时对类型进行检查，从而保证类型安全。
2. **灵活性：** 使用`Class<?>`表示未知类型，可以在运行时接受任意类型的`Class`对象。这种灵活性使得你可以编写更加通用的代码，不需要事先知道具体的类型。例如，在编写通用的框架或工具类时，你可能需要处理多种类型的对象，此时使用`Class<?>`可以更加灵活地适应不同的类型。
3. **与泛型的兼容性：** Java 泛型是通过类型擦除实现的，使用原始类型的`Class`丢失了泛型信息。而使用`Class<?>`保留了泛型的信息，与泛型的使用更加兼容。

综上所述，虽然在某些情况下直接使用原始类型的`Class`可能会更加简洁，但使用`Class<?>`可以提供更好的类型安全性和灵活性，在大多数情况下更推荐使用`Class<?>`。

# 通配符捕获

通配符？不是类型变量。不能编写使用 ？ 作为类型的代码。

但是一些交换操作，必须保存第一个元素。需要使用类型变量。

我们可以编写一个辅助方法：注意，swapHelper是一个泛型方法，而swap不是，他有一个固定的Pair<?> 类型参数。

```java
public static <T> void swapHelper(Pair<T> p){
  T t = p.get.First;
  p.setFirst(p.getSecond);
  p.setSecond(t);
}
```

注意，swapHelper是一个泛型方法，而swap不是，他有一个固定的Pair<?>类型的参数

```json
public static void swap(Pair<?> p){
  // ? first = p.getFirst(); Error！
  swapHelper(p);
}
```

在这种情况下，swapHelper方法的参数T捕获通配符。

通配符捕获只有在非常有限的情况下是合法的。编译器必须能够保证通配符表示单个确定的类型。

例如ArrayList<Pair<?>>中？不能够被捕获，因为列表中 ？可能包含多个不同的类型。

# 源码挑战

## Comparator.comparing

```java
public static <T, U extends Comparable<? super U>> Comparator<T> comparing(Function<? super T, ? extends U> keyExtractor)
{
    Objects.requireNonNull(keyExtractor);
    return (Comparator<T> & Serializable)
        (c1, c2) -> keyExtractor.apply(c1).compareTo(keyExtractor.apply(c2));
}
```

现在想看懂这段代码变得容易很多

1. 这段代码的目的：传入一个键提取器（一个实现了Function接口的函数），然后返回一个新的比较器，可以对提取出的健进行比较。

2. Function接口的原型

   ```java
   public interface Function<T, R> {
   
     /**
        * Applies this function to the given argument.
        *
        * @param t the function argument
        * @return the function result
        */
     R apply(T t);
   }
   
   public interface Function<? super T, ? extends R> {
   
     /**
        * Applies this function to the given argument.
        *
        * @param t the function argument
        * @return the function result
        */
     ? extends R apply(? supper T t);
   }
   ```

3. 为什么要用这么多限定符来规定泛型类型的范围？根据上述的 安全的getter\安全的setter原则可以很好地理解。
   泛型类型T是要传入的值，可以直接写为`Function<T, U> keyExtractor`而不使用限定符，但是这样会给代码增加了无谓的限制，导致有些键提取器其实可以作用于T\U，但是无法传入，因为对T进行提取操作，只要是T的子类都可以实现该操作，因此用`? super T`使得能够传入的Function实现类的范围更广。
   本质：类型 `? super T` 可以实现的操作，T以及子类都可以实现。

4. <T, U extends Comparable<? super U>>中T没什么好说的，U extends Comparable<? super U>保证了提出来的键是可以比较的，保证了compareTo方法的可用性。

## gson.reflect.TypeToken

```java
com.google.gson.reflect

public class TypeToken<T> {
  final Class<? super T> rawType;
	final Type type;
	final int hashCode;

protected TypeToken() {
  this.type = getSuperclassTypeParameter(this.getClass());
	this.rawType = Types.getRawType(this.type);
	this.hashCode = this.type.hashCode();
	}
}
```

