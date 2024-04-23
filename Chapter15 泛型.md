P554---P568

[toc]

# 泛型的好处和引出

1） 传统方法不能对加入到集合 ArrayList中的数据类型进行约束（不安全）
2） 遍历的时候，需要进行类型转换，如果集合中的数据量较大，对效率有影响

> 泛型解决问题
>
> 1. 当我们 ArrayList<Dog> 表示存放到 ArrayList
> 2. 如果编译器发现添加的类型，不满足要求，就会报错
> 1. 在遍历的时候，可以直接取出 Dog 类型而不是
> Object集合中的元素是Dog类型（细节后面说）

> 好处
>
> 1） 编译时，检查添加元素的类型，提高了安全性
> 2） 减少了类型转换的次数，提高效率［说明］
>
> - 不使用泛型:
>   Dog -加入> Object -取出> Dog //放入到ArrayList 会先转成 Object，在取出时，还需要转换成Dog
> - 使用泛型
>   Dog -放入> Dog -取出> Dog // 放入时，和取出时，不需要类型转换，提高效率
>
> 3） 不再提示编译警告

> 老韩理解：泛（广泛）型（类型）=> Integer, String, Dog
> 1）泛型又称参数化类型，是Jdk5.0 出现的新特性，解决数据类型的安全性问题
> 2） 在类声明或实例化时只要指定好需要的具体的类型即可。
> 3） Java泛型<u>***可以保证如果程序在编译时没有发出警告***</u>，运行时就不会产生ClassCastException异常。同时，代码更加简洁、健壮 .
> 4）泛型的作用是：可以在类声明时通过<u>***一个标识(T)表示类中某个属性的类型，或者是某个方法的返回值的类型，或者是参数类型。***</u>

特别强调：E具体的数据类型在定义Person对象的时候指定，即在编译期间，就确定E是什么类型 !

> 泛型的语法
>
> - 泛型的声明
>   `interface 接口<T>{}`和 `class 类<K,V>{}`
>   //比如：List,ArrayList
>   - 说明：
>     1）其中，T,K,V不代表值，而是表示类型。
>     2）任意字母都可以。常用T表示，是Type的缩写
>
> - 泛型的实例化：
>   要在类名后面指定类型参数的值（类型）。如：
>
> 1) List<String > strList = new ArrayList<String>();
> 2) Iterator<Customer> iterator = customers.iterator();

> 泛型使用的注意事项和细节
>
> 1. interface List<T>{  } , public class HashSet<E>{  }.. 等等说明: T, E只能是引用类型看看下面语句是否确?List<Integer> list = new ArrayList<Integer>();
>     List<int> list2 = new ArrayList<int>();
>
> 2. 在指定泛型具体类型后，可以传入该类型或者其子类类型
>
> 3. 泛型使用形式
>     List<Integer> list1 = new ArrayList<Integer>();
>     List<Integer> list2 = new ArrayList<>(); 推荐使用简写,由编译器进行类型推断
>
> 4. 如果我们这样写 List list3 = new ArrayList();
>
>   默认给它的 泛型是 [<E> E就是 Object]
>
>   ArrayList arrayList = new ArrayList();
>
>   //等价于 ArrayList<Obiect> arrayList = new ArravList<>()

> 自定义泛型类(难度)
> >基本语法
> >class 类名<T, R....>{
> >	成员!
> >}
> >
> >注意细节
> >
> >1. <u>***普通成员***</u>可以使用泛型 (属性、方法)
> >2. <u>***使用泛型的数组，不能初始化***</u>   T[] ts = new T[8];不被允许,因为数组在new时不能确定T的类型,就无法在内存中开空间
> >3. <u>***静态方法(静态属性也不行)中不能使用类的泛型***</u> ,(因为静态方法是和类相关的,在类加载时,对象还没创建,  而泛型是在类创建对象定义的时候才会指定的,如果静态方法涉及泛型,JVM无法处理无法完成初始化)
> >4. 泛型类的类型，是在创建对象时确定的 (因为创建对象时,需要指定确定类型)
> >5. 如果在创建对象时，没有指定类型，默认为Object
>
> 解读:
>
> 1. Tiger 后面有泛型，所以我们把 Tiger 就称为自定义泛型
> 2. T，R，M泛型的标识符，一般是单个大写字母
> 3. 泛型标识符可以有多个
> 4. 普通成员可以使用泛型 (属性、方法)

> 自定义接口泛型
>
> ＞基本语法
> `interface 接口名<T,R...＞{`
> `}`
> ＞注意细节
> 1）接口中，静态成员也不能使用泛型（这个和泛型类规定一样）
> 2） 泛型接口的类型，在继承接口或者实现接口时确定
> 3） 没有指定类型，默认为Object使用细节
>
> 在继承接口 指定泛型接口的类型
>
> 当我们去实现IA接口时，因为IA在继承IUsu 接口时，指定了U 为String R为Double . 在实现IUsu接口的方法时，使用String替换U，Double替换R

> 自定义泛型方法
>
> - 基本语法
>   访问修饰符＜T,R> 返回类型 方法名（参数列表）｛
>   }
>
> - 注意细节
>
>   1. 泛型方法，可以定义在普通类中，也可以定义在泛型类
>
>   2. 当泛型方法被调用时，类型会确定
>
>   3. public void eat(E e) {}， 修饰符后没有<T,R.> eat
>      方法不是泛型方法，而是使用了泛型(来自类定义时的泛型指定)
>   4. 泛型方法，可以使用类声明的泛型，也可以使用自己声明泛型

> - 泛型不具备继承性
>   List <Object> list = new ArrayList <String>():// 对吗？ 不对!
>   
>   ```java
>           List<String> list = new ArrayList<>(); // Yes!!
>           List<Object> list1 = new ArrayList<String>(); //Error!!!
>           List<String> list2 = new ArrayList<String>(); //Yes!!
>   ```
>   
> - <>：支持任意泛型类型
>
> - <? extends A>：支持A类以及A类的子类，规定了泛型的上限
>
> - <？ super A>：支持A类以及A类的父类，不限于直接父类，规定了泛型的下限

# JUnit

> 为什么需要JUnit
>
> 1. 一个类有很多功能代码需要测试，为了测试，就需要写入到main方法中
>
> 2. 如果有多个功能代码测试，就需要来回注销，切换很麻烦
> 3. 如果可以直接运行一个方法，就方便很多，并且可以给出相关信息， 就好了->JUnit

> - 基本介绍
>
> 1. JUnit是一个Java语言的单元测试框架
> 2. 多数Java的开发环境都已经集成了JUnit作为单元测试的工具

> @Test  command + return 插入 JUnit 5.X

# 补充

## 泛型迷思汇总

```java
List<String> list = new ArrayList<>();
List<? extends List<String>> list1 = new ArrayList<ArrayList<String>>(); //这里编译器不提示错误!!!
ArrayList<String> test = new ArrayList<>();
test.add("Fuck!!!");
list1.add(test); // Errors!!! 这里出错!!!

/*
Required type:
capture of ? extends List<String>

Provided:
ArrayList  <java.lang.String>
*/


ArrayList<? extends List<String>> list1 = new ArrayList<ArrayList<String>>();
list1.add(new ArrayList<String>());
//你提供的代码会导致编译错误，因为在 Java 中，使用通配符 ? extends 声明的泛型类型是不可变的。换句话说，ArrayList<? extends List<String>> 表示一个具体的未知类型，它可能是 ArrayList<ArrayList<String>>，也可能是 ArrayList<LinkedList<String>> 或其他任何扩展自 List<String> 的列表类型。编译器无法确定 list1 具体是哪种类型，因此它不允许直接向其中添加元素。


//所以说我的写法就很离谱对吧？
//ChatGPT
//是的，你之前提供的代码是不符合通配符的使用规范的。在你的代码中，使用了 ArrayList<? extends List<String>> 这样的通配符，但是却试图向其添加具体的元素，这是不允许的。因为通配符 <? extends List<String>> 表示一个未知的类型，编译器无法确定具体是哪种类型，因此不允许直接添加元素。

//如果你希望创建一个可以添加 ArrayList<String> 元素的列表，你可以考虑使用 List<List<String>> 类型，或者如果需要更加精确的类型约束，你可以使用带有通配符的上界 <? extends List<String>>。不过，需要注意的是，使用带有通配符的类型会使得一些操作受到限制，因此在选择使用时需要根据具体情况进行权衡和考虑。
```