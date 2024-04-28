[toc]

# 集百家所长

Java核心技术卷I 12ed 第八章泛型程序设计

[【Java必备知识】泛型擦除？不怕！这里有获取泛型信息的秘诀！1_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1wk4y1s7j2/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=9bfe1ca76c0d7c42a70bafd019e62999)

[(31 封私信 / 59 条消息) Java 不能实现真正泛型的原因是什么？ - 知乎 (zhihu.com)](https://www.zhihu.com/question/28665443/answer/3340713988)



# 对 泛型1 的基础易错补充

- 概念补充

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
>一个 类型变量T 或者 通配符？ 可以有多个限定 `<T extends comparable & Serializable, E extends HttpServlet>`

- 易错易混辨析

> List<List<String>> list1 = new ArrayList<ArrayList<String>>(); //是非法的，泛型不具备继承性
>
> ArrayList<List<String>> list1 = new ArrayList<ArrayList<String>>(); // 也是非法，泛型不具备继承性
>
> List<? extends List<String>> list1 = new ArrayList<ArrayList<String>>();是合法的
>
> 当然 ArrayList<? extends List<String>> list1 = new ArrayList<ArrayList<String>>();更是合法的
>
> 这里我想说的是，？通配符在是在变相实现泛型继承性的同时，还能允许原本类之间的继承性，List <- ArrayList
>
> 但是其实是有限制的，这个限制正是我在 泛型1 中的结尾所写的我个人的迷思。
>
> 我们知道 通配符? 的出现是Java设计者为了弥补 保证泛型安全性所带来的恼人的限制 所作出的灵活解决方案。

# 泛型代码和虚拟机

对于Java泛型的转换，需要记住：

1. 虚拟机中没有泛型
2. 所有的类型参数都会被替换为它们的限定类型
3. 会合成桥方法来保持多态。
4. 为了保证类型的安全性，必要时会插入强制类型转换。

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

总是可以将参数化类型转化为一个原始类型。也就是说 `Pair`是任何`Pair<Type>`的父类，等价于 `Pair<?>`。

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

# 通配符类型实现泛型类型的继承关系

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240428下午84522698.png" alt="image-20240428下午84522698" style="zoom: 33%;" />

`Pair<? extends Employee> pa = new Pairs<Manager>(cef,ceo);`这是可以的。但是会导致潜在的风险。

```java
Pair<? extends Employee>类型对应的方法体变为

  ? extend Employee getFirst();
	void setFirst(? extends Employee); 
为了安全，直接不让调用了。 尽管你现在看着是 ? extends Employee 但是运行起来的时候肯定是要有一个确定的类型的，可能是 Employee 也可能是 其他任何 Employee的子类，比如是 Boss 类，根本确定不了。
  万一你传进一个employee我放行了，但是我实际指向的是Pairs<Manager>
  结果发生 Manager first = employee 这不就错了。所以直接不让你用了。
  但是getFirst方法，我不管我现在指向的是什么家伙，返回的first 的类型一定是Employee或其子类，所以可以赋值给 Employee类型的引用。

  所以 ? extends Employee 对应安全的访问器 不安全的修改器 ，==想要修改只能传入null==

  相反 ， ? super Employee 对应安全的修改器 不安全的访问器
  method(? super Employee) 可以传入任何 Employee 以及其子类的对象
  ？super Employee get()  不能保证返回的类型是什么，只能保证可以能赋值Object类型
```

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240428下午91816805.png" alt="image-20240428下午91816805" style="zoom:33%;" />

直观的说，带有超类型限定的通配符，允许你写入一个泛型对象，而带有子类型限定的通配符，允许你读取一个泛型对象。



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

# 通配符捕获

通配符？不是类型变量。不能编写使用 ？ 作为类型的代码。

但是一些交换操作，必须保存第一个元素。需要使用类型变量。

我们可以编写一个辅助方法：

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
  swapHelper(p);
}
```

在这种情况下，swapHelper方法的参数T捕获通配符。

通配符捕获只有在非常有限的情况下是合法的。编译器必须能够保证通配符表示单个确定的类型。

例如ArrayList<Pair<?>>中？不能够被捕获，因为列表中？可能包含多个不同的类型。

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

