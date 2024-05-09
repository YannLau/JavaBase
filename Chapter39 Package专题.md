[toc]

本节内容主要框架来自于彭启民-Java程序设计课程-国科大

[java中import作用详解_java import-CSDN博客](https://blog.csdn.net/qq_25665807/article/details/74747868)

# 包（package）

###  Java跨平台特性的需求

java中的所有资源也是以文件方式组织，大量 的类文件需要组织管理。

java中同样采用了目录树形结构。虽然各种常 见操作系统平台对文件的管理都是以目录树的 形式的组织，但是它们对目录的分隔表达方式不同，==为了区别于各种平台，java中采用了"." 来分隔目录。==

### Java中包结构和平台的衔接

java中的资源存在于不同平台下时会有很大差异。跨平台的java包结构和平台之间通过 ==classpath== 的设置来衔接。

### Java语言核心API

 java.lang  java.lang.reflect  java.bean  java.rmi、java.rmi.registry和java.rmi.server  java.security、java.security.acl和java.security.interfaces  java.io  java.util  java.util.zip  java.net  java.awt  java.awt.image  java.awt.peer  java.awt.datatransfer  java.awt.event  java.sql  java.text

### 包的核心作用

提供了类的多层命名空间，用于解决类的命名冲突、类文件管理等问题。

3 个作用： 

- 区分相同名称的类
  - 允许将类组合成较小的单元（似文件夹），避免了名称上的冲突。
- 能够较好地管理大量的类
- 控制访问范围
  - 允许通过访问修饰在更广泛的范围内保护类、数据 和 方法

### 包命名-创建独一无二的包名

Java 的package名称我们可以自己取，不像人的姓没有太大的选择 ( 所以出现很多同名同姓的情况 )，如果依照 Sun 的规范来取套件名称，那理论上不同人所取的套件名称不会相同 ( 需要的话请参阅 “命名惯例” 的相关文章 )，也就不会发生名称冲突的情况。

可是现在问题来了，因為很多package的名称非常的长，在编程时，要使用一个类要将**多个包名.类名**完全写出，会让代码变得冗长，减低了简洁度。

```java
java.io.InputStream is = java.lang.System.in;
java.io.InputStreamReader isr= new java.io.InputStreamReader(is);
java.io.BufferedReader br = new java.io.BufferedReader(isr);
```

显得非常麻烦，于是Sun公司就引入了import。

把package名称分解为机器上的一个目录，利用 操作系统的层次化的文件结构来解决这个问题 

 package名称的第一部分可以用反顺的类的创建者所在 用的internet域名

运行方法： java pack1.Test 包名.类名

Java 包的命名规则： ⚫包名全部用小写字母 ⚫包含多个层次时用“.”分隔 ⚫自定义包名不能以 java 开头

### 定义包

java源文件的第一条语句： package 包名; 例：package ucas.test;

### 导入包

*C/C++ 的 #include会把所包含的内容在编译时添加到程序文件中，而java的import则不同。*

> Import does NOT copy code 
>
> ONLY to help with resolving the names
>
> Simply makes the names known 
>
> Exactly identical to typing the fully-qualified  name everywhere

import就是在java文件开头的地方，先说明会用到那些类别。
接着我们就能在代码中只用类名指定某个类，也就是只称呼名字，不称呼他的姓。

一个java文件就像一个大房间，我们在门口写着在房间里面的class的姓和名字，所以在房间里面提到某个class就直接用他的名字就可以。

但是如果一个java文件里面有多个同个“姓”，即包名相同的类（例如上面的InputStream，InputStreamReader，BufferedReader都是java.io中的类），我们一一写出显得比较繁杂，所以Sun就让我们可以使用

```java
import java.lang.*;
import java.io.*;
//表示文件里面说到的类不是java.lang包的就是java.io包的。编译器会帮我们选择与类名对应的包。
//那我们可不可以再懒一点直接写成下面声明呢？
import java.*;
//历史告诉我们，这样是不行的。因為那些类别是姓 java.io 而不是姓 java。就像姓『诸葛』的人应该不会喜欢你称他為『诸』 先生吧。
//这样写的话只会将java包下的类声明，而不不会声明子包的任何类。
```

> 这里注意，java.lang包里面的类实在是太常太常太常用到了，几乎没有类不用它的， 所以不管你有没有写 import java.lang，编译器都会自动帮你补上，也就是说编译器只要看到没有姓的类别，它就会自动去lang包里面查找。所以我们就不用特别去 import java.lang了。
>

import 包名.类名; //==单类型导入==

import 包名.*; //==按需 类型导入，不是导入一个包中的所有类!==

注：不使用import时要显示指定类所属的包名，如 java.sql.date

> 一开始说 import 跟 #include 不同，是因为import 的功能到此為止，它不像#include 一样，会将其他java文件的内容载入进来。import 只是让编译器编译这个java文件时把没有姓的类别加上姓，并不会把别的文件程序写进来。你开心的话可以不使用import，只要在用到类别的时候，用它的全部姓名来称呼它就行了(就像例子一开始那样)，这样跟使用import功能完全一样。
>

有如下属性：

java以这样两种方式导入包中的任何一个public的类和接口(只有public类和接口才能被导入)

上面说到导入声明仅导入声明目录下面的类而不导入子包，这也是为什么称它们为类型导入声明的原因。

导入的类或接口的简名(simple name)具有编译单元作用域。这表示该类型简名可以在导入语句所在的编译单元的任何地方使用.这并不意味着你可以使用该类型所有成员的简名,而只能使用类型自身的简名。

例如: java.lang包中的public类都是自动导入的,包括Math和System类.但是,你不能使用它们的成员的简名PI()和gc(),而必须使用Math.PI()和System.gc().你不需要键入的是java.lang.Math.PI()和java.lang.System.gc()。

<u>***程序员有时会导入 当前包 或 java.lang包，这是不需要的***</u>，因为当前包的成员本身就在作用域内,而java.lang包是自动导入的。java编译器会忽略这些冗余导入声明(redundant import declarations)。即使像这样
import java.util.ArrayList;
import java.util.*;
多次导入,也可编译通过。编译器会将冗余导入声明忽略.

> Collisions 
>
> ◼ What happens if two libraries are imported  and they include the same names? 
>
> package java.util has a Date 
>
> so does java.sql 
>
> ◼ If you don't use any Date, you're fine 
>
> ◼ If you do, you need to specify which one 
>
> `java.util.Date dt = new java.util.Date();`

### Java5.0增加的静态引用 [static](https://so.csdn.net/so/search?q=static&spm=1001.2101.3001.7020) import

在Java程序中，是不允许定义独立的函数和常量的。即什么属性或者方法的使用必须依附于什么东西，例如使用类或接口作为挂靠单位才行（在类里可以挂靠各种成员，而接口里则只能挂靠常量）。

static import和import其中一个不一致的地方就是==static import导入的是**静态成员**，而import导入的是**类或接口类型**。==

以前： import java.lang.Math; //程序开头处 

...  

`double x = Math.random(); ` 

新： import static java.lang.Math.random; //程序开头处 

...  

`double x = random();`

> 这里有几个问题需要弄清楚：

Static Import无权改变无法使用本来就不能使用的静态成员的约束.

导入的静态成员和本地的静态成员名字相同起了冲突，这种情况下的处理规则，是<u>本地优先</u>。

不同的类（接口）可以包括名称相同的静态成员。例如在进行Static Import的时候，出现了“两个导入语句导入同名的静态成员”的情况。在这种时候，J2SE 1.5会这样来加以处理：

如果两个语句都是精确导入的形式，或者都是按需导入的形式，那么会造成编译错误。

如果一个语句采用精确导入的形式，一个采用按需导入的形式，那么采用精确导入的形式的一个有效。

> static import这么叼那它有什么负面影响吗？

答案是肯定的，去掉静态成员前面的类型名，固然有助于在频繁调用时显得简洁，但是同时也失去了关于“这个东西在哪里定义”的提示信息，理解或维护代码就呵呵了。
但是如果导入的来源很著名（比如java.lang.Math），这个问题就不那么严重了。
### 按需导入机制

使用按需导入声明是否会降低Java代码的**执行效率**?

**绝对不会!**

一、import的按需导入

```java
import java.util.*;

public class NeedImportTest {
    public static void main(String[] args) {
        ArrayList tList = new ArrayList();
    }
}
```


编译之后的class文件 :

```java
//import java.util.*被替换成import java.util.ArrayList
//即按需导入编译过程会替换成单类型导入。
import java.util.ArrayList;

public class NeedImportTest {
    public static void main(String[] args) {
        new ArrayList();
    }
}
```


二、static import的按需导入

```java
import static com.assignment.test.StaticFieldsClass.*;
public class StaticNeedImportTest {
    public static void main(String[] args) {
        System.out.println(staticField);
        staticFunction();
    }
}
```

上面StaticNeedImportTest 类编译之后 :

```java
//可以看出 ： 
//1、static import的精准导入以及按需导入编译之后都会变成import的单类型导入
import com.assignment.test.StaticFieldsClass;

public class StaticNeedImportTest {
    public static void main(String[] args) {
    //2、编译之后“打回原形”，使用原来的方法调用静态成员
        System.out.println(StaticFieldsClass.staticField);
        StaticFieldsClass.staticFunction();
    }
}
```

这是否意味着你总是可以使用按需导入声明? **是,也不是!**

在类似Demo的非正式开发中使用按需导入声明显得很有用。

然而,有这四个理由让你可以放弃这种声明:

> 编译速度：在一个很大的项目中，它们会极大的影响编译速度.但在小型项目中使用在编译时间上可以忽略不计。
>
> 命名冲突：解决避免命名冲突问题的答案就是使用全名。而按需导入恰恰就是使用导入声明初衷的否定。
>
> 说明问题：毕竟高级语言的代码是给人看的，按需导入看不出使用到的具体类型。
>
> 无名包问题：如果在编译单元的顶部没有包声明，Java编译器首选会从无名包中搜索一个类型，然后才是按需类型声明。如果有命名冲突就会产生问题。

Sun的工程师一般不使用按需类型导入声明.这你可以在他们的代码中找到:
在java.util.Properties类中的导入声明:

```java
import java.io.IOException;
import java.io.PrintStream;
import java.io.PrintWriter;
import java.io.InputStream;
import java.io.OutputStream;
import java.io.Reader;
import java.io.Writer;
import java.io.OutputStreamWriter;
import java.io.BufferedWriter;
import java.security.AccessController;
import java.security.PrivilegedAction;
```

可以看到他们用单类型导入详细的列出了需要的java.io包中的具体类型。

### 正确使用包

对类路径的设置通常有两种方法： 

 在系统的环境变量中设置，设置方法依据平台而变； 

 以命令参数的形式来设置。

 如： (易错 ==javac的时候不能用.分割路径==)

javac -classpath <u>d:\jdk\lib</u> d:\ac\ucas\test\TestFile.java 

java -classpath .;d:\jdk\lib; d:\ac ucas.test.TestFile==.代表当前路径==

> 在根目录下（已编译得到class）
>
> java -classpath ~/Desktop com.lyp.test //OK!

类的目录结构要和类中第一句“包声明”一致。 如类TestFile.class对应的.java文件的第一句： package ac.ucas.test; 

为了便于工程发布，可以将自己的类树打成.jar文件。

对其它资源的使用，如图标文件，文本等资源文件的使用必须要注意，<u>***查找资源文件不应从类文件所在的目录开始***</u>，而是应该从package指定的类路径的起点开始。

> YannLau通过代码实验发现
>
> 源代码中读取资源  `FileReader fi = new FileReader("vim.txt");`
>
> （写成绝对路径就没有什么好说的了，直接去该路径下寻找资源）
>
> 如果写成相对路径，它拼接上你执行java命令时的工作目录的前缀。
>
> 比如 vim.txt就在Desktop中时，我在Desktop文件夹中执行
>
> java -classpath ~/Desktop com.lyp.test //OK! 可以找到vim.txt  其实此时-classpath ~/Desktop可以省略
>
> 当我在 / 根目录下执行或者其他任何不含vim.txt的工作目录中执行
>
> java -classpath ~/Desktop com.lyp.test
>
> 会找不到vim.txt!!! 只能执行和资源无关的sout语句。

![image-20240509下午34550831](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240509下午34550831.png)

### 确保类的存放路径和类中指明的"包路径"一致

i)编写.java文件时存放的目录事先确定好，如TestFile.java就直接放在cas\test目录下，然后用下面的语句编译： javac -classpath d:\jdk\lib d:\ac\ucas\test\TestFile.java 

当编译完成后，产生的TestFile.class文件会出现在编译命令中java文件的描述路径中。

即出现在d:\ac\ucas\test中

ii)通过-d参数的使用来编译程序。

如使用下面的语句来编译： javac -d d:\ d:\temp\TestFile.java 将在-d后指定的目录d:\下面自动按照packagek中指定的目录结构来创建目录，并且将产生的.class文件放在这个新建的目录下，即在d:\下面建立ac\ucas\test目录 ，然后产生的TestFile.class放在d:\ac\ucas\test目录下.

### 有益的建议

（1）保持数据私有。 

（2）初始化数据。 Java并不对本地变量初始化，但会初始化对象中的 实例字段，但是永远不要依赖于默认值。 

（3）不要在一个类中使用太多的基本类型。 

（4）次序约定： 公开部件；包作用域部件；私有部件 在每一部分内我们列出次序如下： 实例方法；静态方法；实例字段；静态字段 

（5）分解,简化。
