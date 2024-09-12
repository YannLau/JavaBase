P460---P498

[toc]

# 包装类 Wrapper

• 包装类的分类
1. 针对八种基本定义相应的引用类型-------包装类。
2. 有了类的特点，就可以调用类中的方法。

(标黄的父类都是 Number类)

![image-20240207下午83240594](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午83240594.png)

> 包装类和基本数据的转换
>
> 包装类 和 基本数据类型的相互转换，这里以int 和 Integer演示。
> 1）jdk5 前的手动装箱和拆箱方式，装箱：基本类型->包装类型，反之，拆箱
> 2）jdk5 以后（含jdk5）的自动装箱和拆箱方式
> 3）自动装箱底层调用的是==valueOf方法==，比如Integer.valueOf()
> 4）其它包装类的用法类似，不一一举例



## Boolean继承关系

![image-20240207下午84237052](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午84237052.png)

## Character继承关系

![image-20240207下午84225255](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午84225255.png)

## Byte Short Integer Long Float Double 继承关系

![image-20240207下午84158894](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午84158894.png)

> 包装类和基本数据类型的转换
>
> 演示 包装类 和 基本数据类型的相互转换，这里以int 和 Integer演示。
> 1）jdk5 前的手动装箱和拆箱方式，
> 装箱：基本类型->包装类型，反之，拆箱
>
> ```java
> int n1 = 100;
> 
> //手动装箱
> Integer integer = new Integer(n1);
> Integer integer1 = Integer.valueOf(n1);
> 
> //手动拆箱
> int i = integer.intValue();
> ```
>
> 2）jdk5 以后（含jdk5）的自动装箱和拆箱方式
>
> ```java
> int n2 = 200;
> //自动装箱
> Integer integer2 = n2; //底层使用的是 Integer.valueOf(n2)
> 
> //自动拆箱
> int n3 = integer2; //底层仍然使用的是 intValue()方法
> ```
>
> 3） 自动装箱底层调用的是valueOf方法，比如`Integer.valueOf();`

![image-20240207下午90410145](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午90410145.png)

不同 , 三元运算符会自动提升精度 !

> 包装类型 和 String 类型 的相互转换 WrapperVSString.java
>
> ```java
> // 包装类型->String类型
> Integer i = 10;
> // 方式1：
> String s1 = i.toString();
> //方式2：
> String s2 = String.valueOf();
> // 方式3：
> String s3 = i + "";
> System.out.println(s3);
> 
> // String一>包装类
> String s1 = "123";
> // 方式1：
> Integer j = new Integer(s1); //已被弃用
> // 方式2：
> Integer j2 = Integer.valueOf(s2);
> //方式 3:
> Integer j3 = Integer.parseInt(s1);//会自动装箱
> ```

> Integer类 和 Character 类的常用方法
>
> System.out.println(Integer.MIN_VALUE); //返回最小值
> System.out.println(Integer.MAX_VALUE)：//返回最大值
> System.out.printIn(Character.isDigit('a')) : //判断是不是数字
> System.out.printIn(Character.isLetter('a'))：//判断是不是字母
> System.out.println(Character.isUpperCase('a'))：//判断是不是大写
> System.out.println（Character.isLowerCase（"'a'））：//判断是不是小写
> System.out.println（Character.isWhitespace（'a'））：//判断是不是空格
> System.out.println（Character.toUpperCase（'a'））：//转成大写
> System.out.println（Character.toLowerCase（'A'））：//转成小写

# Integer创建机制

```java
public void method10 {
	Integer i = new Integer(1);
	Integer j = new Integer(1);
	System.out.println(i == j); //False,因为是新 new 的
  
	Integer m = 1; //底层使用 Integer.valueOf();
	Integer n = 1;//阅读源码
	System.out.println(m == n);  //true
	Integer x = 128;
	Integer y = 128;
	System.out.println(x == y);  //false
}

//valueOf 源码
public static Integer valueof(int i) {
if (i >= IntegerCache.low (-128) && i <= IntegerCache.high (127) )
	return IntegerCache.cache[i + (-IntegerCache.low)];
return new Integer(i);
｝
//所以，这里主要是看范围 -128~127 就是直接返回，否则，就new Integer（xx）；
  
  Integer -128 --- 127  是在 Integer 类加载的时候就创建好的
  
  == 判断,只要有基本数据类型,就是对值进行判断!
  int i = 127;
  Integer i1 = 127;
  i == i1; true
```

# String类

> String 类的理解和创建对象
>
> 1) String 对象用于保存字符串，也就是一组字符序列（更是码元序列）
> 2) 字符串常量对象是用双引号括起的字符序列。例如:"你好"、"12.97"、"boy"等
> 3) 字符串的字符使用Unicode字符编码，一个字符(不区分字母还是汉字)占两个字节。
> 4) String类较常用构造器 (其它看手册) : 
>    1) String s1 = new String();
>    2) String s2 = new String(String original);
>    3) String s3 = new String(char[] a);
>    4) String s4 = new String(char[] a,int startIndex,int count)

![image-20240207下午105114908](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午105114908.png)

> 5. Serializable 可串行化接口,表示String 对象可以串行化,可以在网络中传输!
>
> 6. String 类实现了 Comparable 接口说明可以进行比较
>
> 7. String 是一个 final 类,不能被继承.
>
> 8. String类中有属性 private final char value[] ; （==好像最新的是一个byte[]==）用于存储字符串内容.  final 说明一旦赋值<u>***不能修改***</u>.  想要理解这里的不能修改需要一定的功力! value[]是一个数组,那么它的名字就相当于一个指针, 不能修改指的是 指针所存的地址 不能修改了,不能再指向别处了 ,  但是指向的地址的内容就不一定了!

> String 创建对象的两种方式
>
> 1)方式一 :  直接赋值 String  s="hspedu";
>
> 2)方式二 : 调用构造器 String s = new String("hspedu”);

> 两种创建String对象的区别
>
> 方式一 : 直接赋值 String s="hsp”;
>
> 方式二 : 调用构造器 String s2 = new String("hsp");
>
> 1. 方式一 : 先从常量池查看是否有"hsp"数据空间，如果有，直接指向 ; 如果没有则重新创建，然后指向。s 最终指向的是常量池的空间地址
>
> 2. 方式二:先在堆中创建空间，里面维护了 value属性 ，指向常量池的hsp空间 . 如果常量池没有"hsp"，重新创建，如果有，直接通过value指向。最终指应的是堆中的空间地址
>
> 3. 画出两种方式的<u>***内存分布***</u>图

![image-20240207下午112932581](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午112932581.png)

> ![image-20240207下午114531586](/Users/yannlau/Library/Application Support/typora-user-images/image-20240207下午114531586.png)
>
> 当调用 ==intern 方法==时，如果池已经包含一个等于此 String 对象的字符串(用equals(Object)方法确定)，则返回池中的字符串(地址)。否则，将此 String 对象添加到池中，并返回此 String 对象的引用
>
> 解读 ; (1) b.intern() 方法最终返回的是常量池的地址(对象)

# 字符串的特性

> 1. String是一个final类，代表不可变的字符序列
>
> 2. 字符串是不可变的。一个字符串对象一旦被分配，其内容是不可变的
>
>    ```java
>     String s1 = "hello";
>    s1="haha"; 
>    //创建了2个对象
>    ```
> 
> 3. String a = "hello"+"abc";
>
>    创建了几个对象? 只有一个对象
>
>    //老韩解读：String a = "hello"+"abc"： //==＞优化等价 String a = "helloabc";
>
>    分析
>
>    1. 编译器不傻，做一个优化，判断创建的常量池对象，是否有引用指向
>
>    2. String a = "hello"+"abc";  =》 String a = "helloabc"；
>
> 4. String a = "hello"： //创建 a对象
>     String b ="abc"：//创建 b对象
>    String c = a+b；创建了几个对象？3个 画出内存图？
> 
>    1. 先 创建一个 StringBuilder sb = new StringBuilder();
>       2. 执行 sb.append("hello"); 
>    3. sb.append("abc"); 
>    4. String c= sb.toString(); (该方法 new 一个 String)
>    5. 最后其实是 C 指向堆中的对象（String） value[]
>       -> 池中 "helloabc"
>    6. 就算有一个 String d = "helloabc" 直接指向常量池也是和 C不同的对象.
>    7. ![image-20240208上午123609188](/Users/yannlau/Library/Application Support/typora-user-images/image-20240208上午123609188.png)

底层 StringBuilder sb = new StringBuilder());

sb.append(a);

sb.append(b);

sb是在堆中，并且append是在原来字符串的基础上追加的.

重要规则:

`String c1 = "ab" + "cd"：常量相加，看的是池。`

`String c1 = a+b；变量相加，是在堆中.`

学习思路:

看源码学习 !

> String 的特性
>
> 下列程序运行的结果是什么，尝试画出内存布局图？
>
> ```java
> public class Test1 {
> 	String str = new String("hsp");
> 	final char[] ch = {'j', 'a', 'v', 'a'};
> 	public void change(String str, char ch[]){ 
> 		str = "java";
> 		ch[0] = 'h';
>   }
> 	public static void main(String[] args) {
> 		Test1 ex = new Test1();
> 		ex.change(ex.str, ex.ch);
> 		System.out.print(ex.str + "and ");
> 		System.out.println(ex.ch);
>   }
> }
> //打印  "hsp and hava" !!!
> //关键点在于传参
> //在调用 change 方法时,在方法中又创建开辟了一个新的 String 引用str ,也指向堆中的 String"hsp" , 此时改变 str 的指向,对原来的 ex.str 并没有影响.
> //等于说 java 中的参数传递的都是 引用传递  而不是 实际的传递.
> ```

> String 类的常见方法
>
> String类是保存字符串常量的。每次更新都需要重新开辟空间，效率较低，因此java设计者还提供了StringBuilder 和 StringBuffer 来增强String的功能，并提高效率。［后面我们还会详细介绍 StringBuilder 和 StringBuffer］
>
> ```java
> //下面这段代码的运行效率很低
> String s = new String("");
> for(int i = 0; i < 80000; i++) {
> 	s += "hello";
> }
> ```
>
> ```java
> String类的常见方法一览［告诉你怎么用，就可以］
> equals
> equalsIgnoreCase
> length();  //字符串是方法获取长度,数组是常量 length 获取长度
> indexOf();//获取字符在串中第一次出现的索引位置,找不到返回-1;
> lastindexOf(); //最后一次出现位置,找不到返回-1,可以是字符串,上同
> substring(); //截取指定范围子串
> //substring(6):从索引 6 处,取到最后
> //substring(0,5): 取出 0-4 包头不包尾
> trim();// 去掉串中的前后空格,中间的空格不管
> charAt();//获取某索引处的字符
> toUpperCase();
> toLowerCase();
> concat("ss");//拼接字符串"ss"到后面
> replace(A,B); //将字符串中的 A 替换为 B
> //replace 方法得到新的字符串而不是修改原来字符串!
> split("xx");//以 xx 进行分割,得到字符串数组
> //如果有特殊字符需要转义
> compareTo();// 返回第一个不同字符的前面减后面的值,如果是 "jack".compareTo("jack123"); 返回长度差值 -3.完全相同发字符串比较返回0
> toCharArray();
> format();//格式化输出
> //1.%s，%d，%.2f %c 称为占位
> //2.这些占位符由后面变量来替换
> //3.%s 表示后面由 字符串来替换
> //4.%d 是整数来替换
> //5.%.2f 表示使用小数来替换，替换后，只会保留小数点两位，并且进行四舍五入的处理
> //6.%c 使用char 类型来替换
> String formatStr ="我的姓名是年龄是%d，成绩是%.2f 性别是%c.希望大家喜欢我!";
> String info2 = String.format(formatStr, name
> score, gender);
> 
> ```

# StringBuffer

java.lang.StringBuffer代表可变的字符序列，可以对字符串内容进行增删.

很多方法与String相同，但StringBuffer是可变长度的。

StringBuffer是一个容器。

![image-20240208下午13343730](/Users/yannlau/Library/Application Support/typora-user-images/image-20240208下午13343730.png)

> 1. StringBuffer 的直接父类 是 AbstractStringBuilder
> 2. StringBuffer 实现了 Serializable，即StringBuffer的对象可以串行化
> 3. 在父类中 AbstractStringBuilder 有属性 char[]  value，不是final   .  该 value 数组存放 字符串内容，因此存放在堆中的
> 4. StringBuffer 是一个 final类，不能被继承
> 5. 因为StringBuffer 字符内容是存在char[] value，所以在变化（增加/删除）不用每次都更换地址（即创建新对象）, 所以效率高于 String

> String VS StringBuffer
>
> 1） String 保存的是字符串常量，里面的值不能更改，每次String类的更新实际上就是更改地址，效率较低 //private final char value[ ];
>
> 2） StringBuffer保存的是字符串变量，里面的值可以更改，每次 StringBuffer 的更新实际上可以更新内容，不用更新地址，效率较高  // char[ ] value; // 这个放在堆.

> StirngBuffer 构造器
>
> 1. StringBuffer();
>
>    构造一个其中不带字符的字符串缓冲区，其初始容量为16个字符。
>
> 2. StringBuffer(CharSequence seq)
>    public java.lang.stringBuilder(CharSequence seq)构造一个字符串缓冲区，它包含与指定的 CharSequence 相同的字符。
>
> 3. StringBuffer(int capacity)  //capacity [容量]
>    构造一个不带字符，但具有指定初始容量的字符串缓冲区。即对 char[]大小进行指定
>
> 4. StringBuffer(String str)
>    构造一个字符串缓冲区，并将其内容初始化为指定的字符串内容。初始长度为 str.length+16

> String 和 StringBuffer 怎么互相转换
>
> 1. String -> StringBuffer
>
>    String s = "hello";
>
>    1. StringBuffer b1 = new StringBuffer(s);//对 str 本身没有影响
>    2. StringBuffer b2 = new StringBuffer();
>       b2.append (s);
>
> 2. StringBuffer -> String
>
>    1. String s2 = b1. toString(); //b1 [StringBuffer]
>    2. String s3 = new String(b1);

> StringBuffer 常用方法
>
> 1. append(); 返回值仍是本对象, append(x),若 x 为 null,会将其转为"null"追加!
> 2. delete(x,y);  删除 index 为 [x , y-1] 的字符. 
> 3. replace(index1,index2,"xxx"); 用 xxx 替换 [index1,index2-1]处的内容.
> 4. indexOf("xx")  返回 xx 第一次在字符串中出现位置,找不到返回-1.
> 5. insert(n,"xxx");  在索引为 n 处插入"xxx". 原本索引为 n的内容自动后移.
> 6. length();

# StringBuilder

1）一个可变的字符序列。此类提供一个与 StringBuffer 兼容的 API，但不保证同步（StringBuilder 不是线程安全）。该类被设计用作 StringBuffer 的一个简易替换，用在字符串缓冲区被单个线程使用的时候。如果可能，建议优先采用该类，因为在大多数实现中，它比 StringBuffer 要快【后面测］。
2） 在 StringBuilder上的主要操作是 append 和 insert方法，可重载这些方法，以接受任意类型的数据。

![image-20240208下午24829776](/Users/yannlau/Library/Application Support/typora-user-images/image-20240208下午24829776.png)

> 1. StringBuilder 继承 AbstractStringBuilder 类
>
> 2. 实现了 Serializable，说明StringBuilder对象是可以串行化（对象可以网络传输，可以保存到文件）
> 3. StringBuilder 是final类，不能被继承
> 4. StringBuilder 对象字符序列仍然是存放在其父类 AbstractstringBuilder的 char[] value；因此，字符序列是堆中
> 5. StringBuilder 的方法，没有做互斥的处理，即没有synchronized 关键字，因此在单线程的情况下使用StringBuilder

> StringBuilder 的常用方法
>
> StringBuilder 和 StringBuffer 均代表可变的字符序列，方法是一样的，所以使用和StringBuffer一样 , 看老师演示. [参考StringBuffer] 

# String StringBuffer StringBuilder 比较

1. StringBuilder 和 StringBuffer 非常类似，均代表可变的字符序列，而且方法也一样
2. String：不可变字符序列，效率低，但是复用率高。
3. StringBuffer： 可变字符序列、效率较高（增删）、线程安全，看源码

4) StringBuilder: 可变字符序列、效率最高、线程不安全
5) string s="a"： //创建了一个字符串
    s+= "b"：//实际上原来的“a"字符串对象已经丢弃了，现在又产生了一个字符串s+"b"（也就是“ab"）。如果多次执行这些改变串内容的操作，会导致大量副本字符串对象存留在内存中，降低效率。如果这样的操作放到循环中，会极大影响程序的性能 => <u>***结论：如果我们对String 做大量修改，不要使用String***</u>
6) 效率: StringBuilder > StringBuffer > String

> 选择:
>
> 1. 如果字符串存在大量的修改操作，一般使用 String Buffer 或StringBuilder
> 2. 如果字符串存在大量的修改操作，并在单线程的情况，使用 StringBuilder
> 3. 如果字符串存在大量的修改操作，并在多线程的情况，使用 StringBuffer
> 4. 如果我们字符串很少修改，被多个对象引用，使用String，比如配置信息等
> 5. StringBuilder 的方法使用和 StringBuffer 一样，不再说

# Math 方法

Math 类包含用于执行基本数学运算的方法，如初等指数、对数、平方根和三角函数。

方法均为静态方法 !

```java
1） abs 绝对值  (返回值根据传入类型而定)
2） pow 求幂  Math.pow(x,y); 求 x 的 y 次方 (返回double)
3） ceil 向上取整 (返回 double)
4） floor 向下取整 (返回 double)
5） round 四舍五入 (返回 long)只对小数点后第一位进行处理
6） sqrt 求开方  (返回 double)
7） random 求随机数 //返回 0<=x<1 的 double
  //思考：请写出获取 a-b之间的一个随机整数，a,b均为整数？2<=x<=7
8)  max 求两个数的最大值
9)  min 求两个数的最小值
```

# Arrays 类

大多都是 static 工具方法

1. toString();

2. sort();

   1. 默认排序方法
   2. 自定义排序方法

   > 1. 可以直接使用冒泡排序，也可以直接使用Arrays提供的sort方法排序
   > 2. 因为数组是引用类型，所以通过sort排序后，会直接影响到 实参 arr
   > 3. sort重载的，也可以通过传入一个接口 Comparator 实现定制排序
   > 4. 调用 定制排序 时，传入两个参数（1） 排序的数组 arr (2)实现了Comparator接口的匿名内部类，要求实现compare方法
   > 5. 6．这里体现了接口编程的方式，看看源码，就明白
   >    源码分析

3. binarySearch 通过二分搜索法进行查找，要求必须排好序
   int index = Arrays.binarySearch(arr, 3);

   如果不存在该元素,返回负值-(low+1). 存在返回 index

   -(low+1): low 为若该元素存在与数组中,应该所在的位置的 index

4. copyOf();

   从 arr 数组中，拷贝 arr.length个元素给 newArr数组中
   Integer[] newArr = Arrays.copyOf(arr, arr.length);

   如果拷贝的长度 > arr.length ,就在新数组后面置空.

   如果拷贝长度<0 , 抛出异常

5. fill();

   Arrays.fill(num,99); 使用 99 去填充 num数组

6. boolean equals = Arrays.equals(arr1,arr2);

7. List<Integer> asList = Arrays.asList(1,23,53,224);

   将一组值转换为 list

# System方法

1. System.exit(整数值) 退出当前程序
   1. 0 表示正常状态
   2. 后面的程序还会执行
2. arraycopy：复制数组元素，比较适合底层调用，一般使用
   Arrays.copyOf完成复制数组.
   int[] src={1,2,3};
   int[] dest = new int[3];
   System.arraycopy(sc, 0, dest, 0, 3); <u>***3 代表copy个数***</u>
3. currentTimeMillens()：返回当前时间距离1970-1-1的毫秒数
4. gc：运行垃圾回收机制 System.gcO；

# BigInteger 和 BigDecimal

1） Biglnteger适合保存比较大的整型
2） BigDecimal适合保存精度更高的浮点型（小数）

```java
BigInteger b = new BigIntegeer("xxxxxxxxxxx");//字符串传参
1)  add 加
2)  subtract减
3)  multiply乘
4)  divide除
BigDecimal b = new BigDecimal("xxxxx.x");
BigDecimal的 divide 可能会抛出异常!!!
  如何解决?
  指定精度即可!!!
  bigDecimal.divide(bigDecimal2,BigDecimal.ROUND_CEILING);
如果有无限循环小数,就会保留被除数的精度,那么长的位数
```

日期类［知道怎么查，怎么用即可，不用每个方法都背］

# 第一代日期类 Date类

1. Date：精确到毫秒，代表特定的瞬间
2. SimpleDateFormat： 格式和解析日期的类,配套使用
3. SimpleDateFormat 格式化和解析日期的具体类。它允许进行格式化（日期 文本）、解析（文本->日期）和规范化。

![image-20240208下午54449707](/Users/yannlau/Library/Application Support/typora-user-images/image-20240208下午54449707.png)

IDEA 中 Diagram 视图 , Properties 属性指的是 getXX setXX 后面的 XX.

> 应用实例
>
> ```java
> Date d1 = new Date(); //获得当前系统时间
> 1.获取当前系统时间
> 2. 这里的Date类是在java.util包
> 3.默认输出的日期格式是国外的方式，因此通常需要对格式进行转换
>   
> //格式化方法
> 1．创建 SimpleDateFormat对象，可以指定相应的格式
> 2.这里的格式使用的字母是规定好，不能乱写
>   SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日  hh:mm:ss  E");
> String format = sdf.format(d1); //format:将日期转换成指定格式的字符串
> Sout(format)
> ```
>
> ```java
> Date d2 = new Date(long x); ////通过指定毫秒数得到时间
> 
> d2.getTime(); // 获取该对象时间对应的毫秒数!
> 
> //可以把一个格式化的String转换成对应的 Date
> //得到Date 仍然在输出时还是按照国外的形式，如果希望指定格式输出，需要转换
> //在把String -> Date，使用的 sdf 格式需要和你给的String的格式一样，否则会抛出转换异常
>   String s = "1996年01月01日 10:20:30 星期一";
>   Date parse = sdf.parse(s);
> ```

# 第二代日期类 Calendar类

![image-20240208下午61501366](/Users/yannlau/Library/Application Support/typora-user-images/image-20240208下午61501366.png)

`public abstract class Calendar extends Object implements Serializable, Cloneable, Comparable < Calendar >`

Calendar 类是一个抽象类，它为特定瞬间与一组诸如 YEAR、MONTH、DAY_OF_MONTH、HOUR 等日历字段之间的转换提供了一些方法，并为操作日历字段（例如获得下星期的日期）提供了一些方法。

1. Calendar是一个抽象类，并且构造器是private
2. 可以通过 getInstance() 来获取实例
3. 提供大量的方法和字段提供给程序员
4. Calendar 没有提供对应的格式化的类,因此需要程序员自己组合
5. 5.如果我们需要按照 24小时进制来获取时间，
    Calendar. HOUR==改成=＞Calendar. HOUR_OF_DAY

```java
Calendar c = Calendar.getInstance();//创建日历对象,比较简单自由
sout(c);//会打印输出一个超长数组,包含对象的很多字段!

c.get(Calendar.YEAR); //获取c对象的YEAR字段
c.get(Calendar.MONTH); 
(c.get(Calendar.DAY_OFMONTH))+1; //返回的月份是从零开始编号的
c.get(Calendar.HOUR); 
c.get(Calendar.MINITE); 
c.get(Calendar.SECOND); 
```

# 第三代日期类 LocalTime

> 前面两代日期类的不足分析
>
> JDK 1.0中包含了一个java.util.Date类，但是它的大多数方法已经在JDK 1.1引入Calendar类之后被弃用了。而Calendar也存在问题是：

1. 可变性：像日期和时间这样的类应该是不可变的。
2. 偏移性：Date中的年份是从1900开始的，而月份都从0开始。
3. 格式化：格式化只对Date有用，Calendar则不行。
4. 此外，它们也不是线程安全的；不能处理闰秒等（<u>***每隔2天，多出1s***</u>）。

> 第三代日期类常见方法
>
> 1. LocalDate（日期/年月日）、LocalTime（时间/时分秒）、LocalDateTime（日期时间/年月日时分秒）JDK8加入

![image-20240208下午71141946](/Users/yannlau/Library/Application Support/typora-user-images/image-20240208下午71141946.png)

```java
LocalDateTime ldt = LocalDateTime.now(); //LocalDate.now();//LocalTime.now()
System.out.println(ldt);

//使用now()返回当前日期时间的 对象
ldt. getYear ();
ldt. getMonthValue(); //得到月份对应的数字
ldt. getMonth();  //字符串 月份对应的英语
ldt. getDay0fMonth();
ldt. getHour();
ldt. getMinute();
ldt. getSecond ();

//LocalDate 只能获得 年月日
//LocalTime 只能获得 时分秒
```

> 使用 DateTimeFormatter格式日期类进行格式化
>
> 类似于 SimpleDateFormat
> `DateTimeFormatter dtf = DateTimeFormatter.ofPattern(格式);`
> `String str = dtf.format(日期对象);`

# 第三代日期类 Instant 时间戳

```java
//通过静态方法now() 获取表示当前时间戳的对象
Instant now = Instant.now();
sout(now); //输出内容和LocalDateTime一样
//通过from可以把Instant转换成Date
Date date = Date.from(now);
//通过date 的 toInstant转换成Instant对象
Instant instant = date.toInstant();
```

> 第三代日期类的更多方法 , 用到去查就好了
>
> LocalDateTime类
> MonthDay类：检查重复事件
> 是否是闰年
> 增加日期的某个部分
> 使用plus方法测试增加时间的某个部分
> 使用minus方法测试查看一年前和一年后的日期
