[toc]

原文链接 ：http://t.csdnimg.cn/71ApP

[美国时间/华盛顿时间/美国现在时间几点 (beijing-time.org)](https://www.beijing-time.org/US/)这个网址可以查看夏令时等全面时间信息！

# Preface

日常开发中，日期和时间是我们经常遇到并且需要处理的。由于日期时间的复杂性，随着Java的发展，也诞生了三代日期处理API。

接下来就让我们一起来探究下Java日期时间的前世今生。

# 一、Basic Conception

## 1.1 日期与时间

> 日期是指某一天，它不是连续变化的;而时间分为带日期的时间和不带日期的时间
>
> 带日期的时间能唯一确定某个时刻，不带日期的时间是无法确定一个唯一时刻的

日期如下

- 2022-05-20
- 1992-09-13

时间如下

- 2022-11-20 08:01:59
- 08:01:59

## 1.2 本地时间

当前时刻是2022年11月20日早上8:08，我们实际上说的是本地时间，也就是北京时间。

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726185756444.png" alt="QQ_1726185756444" style="zoom:33%;" />

有点常识的我们都知道，如果这一个时间点，地球是不同地区的小伙伴看手表，看到的本地时间是不同的。不同的时区，在同一时刻，本地时间是不同的。

## 1.3 时区

> 由于本地时间没法确定一个准确时刻，所以时区的概念就出现了
>
> 全球一共分为**24个时区**，**伦敦**所在的时区称为**标准时区**，
>
> 其他时区按东／西偏移的小时区分，北京所在的时区是**东八区**

时区的三种表示方式

- 以GMT或者UTC加时区偏移表示

> GMT是前世界标准时，UTC是现世界标准时，UTC 比 GMT 更精准，以原子时计时，每隔几年会有一个闰秒，我们在开发程序的时候可以忽略两者的误差。（误差在0.9s以内）

例如：`GMT+08:00`或者`UTC+08:00` 表示东八区 `UTC/GMT +9:00` 表示东九区

- 以缩写表示
  例如：`CST`表示`China Standard Time`，也就是**中国标准时间**
- 以洲／城市表示
  例如：Asia/Shanghai，表示**上海**所在地的时区。城市名称不是**任意的城市**，而是由国际标准组织规定的城市。

这里好学的小伙伴就会问了，中国的时区为什么是`Asia/Shanghai`，而不是`Asia/Beijing` 呢？

北京和上海处于同一时区（**东八区**），只能保留一个。而作为时区代表上海已经足够具有代表性。

**注：**虽然身处不同时区的两个小伙伴，同一时刻表上显示的本地时间不同，但两个表示的时刻是相同的

## 1.4 夏令时

> 是夏天开始的时候，把时间往后拨1小时，夏天结束的时候，再把时间往前拨1小时

夏令时是比时区更为复杂的计算方式，很多地区都是不执行夏令时的，我国也在1992年就废除了

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726186004291.png" alt="QQ_1726186004291" style="zoom:33%;" />

# 二、Java三代日期时间API

第一代日期时间类定义在`java.util`包下，主要包含`Date 、SimpleDateFormat`类

第二代日期时间类也是定义在java.util包下，主要是 `Calendar、TimeZone` 类

第三代日期时间类是在Java 8引入的，定义在 `java.time` 包下，主要包含`LocalDateTime 、ZonedDateTime 、ZoneId 、DateTimeFormatter`类

此时，小伙伴可能会问了

① 为什么会出现三代日期时间类呢？

答：历史遗留问题，早期的API存在着很多问题，所以java在不断迭代更新，引入了新的API。更加方便我们处理日期时间

②既然早期API存在诸多问题，我们能不能直接使用最新API呢？

答：如果是新项目，就建议使用最新API。因为新的API中的类是不变对象，并且是线程安全的；

如果读者和我一样苦逼，还得维护JDK1.6 这样的遗留代码，就必须对一二代API有所了解。

③如果要维护历史遗留代码，新旧API可以相互转换吗？

答：当然了，新旧API之间也是可以相互转换的。

# 三、 第一代日期时间Date

## 3.1 继承关系

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726186207330.png" alt="QQ_1726186207330" style="zoom:33%;" />

## 3.2 获取实例对象

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726186224862.png" alt="QQ_1726186224862" style="zoom: 50%;" />

从上图可以看出，已经弃用了好多。这里我们说一说常用的两种

- `Date()`
  创建的对象可以获取本地当前时间，精确到毫秒
- `Date(long date)`
  以从`1970 年 1 月 1 日 0 时 0 分 0 秒` 开始的毫秒数初始化时间对象

```java
import java.util.Date;

public class DateTest {
  public static void main(String[] args) {
    Date date1 = new Date();
    System.out.println(date1);
    Date date2 = new Date(15000);
    System.out.println(date2);
  }
}
//输出
Sun Nov 20 09:55:58 CST 2022
Thu Jan 01 08:00:15 CST 1970
```

## 3.3 常用方法

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240913上午82650638.png" alt="image-20240913上午82650638" style="zoom:50%;" />

我们发现好多也是已弃用了，这里面的很多方法将会被第二代日期时间中的方法取代

## 3.3 获取当前年月日时分秒

getYear() 、getMonth()、getDate()、getHours()、getMinutes()、getSeconds()、toString()、toGMTString()、toLocaleString()

注：

getYear()返回的年份必须加上1900，

getMonth() 返回的月份是0-11 分别表示1-12月，所以要+1

getDate() 返回的日期范围是1~31，不能加1

```java
import java.util.Date;

public class DateTest {
  public static void main(String[] args) {
    //获取当前时间
    Date date = new Date();
    //获取当前年份
    System.out.println((date.getYear() + 1900));
    //获取当前月份
    System.out.println((date.getMonth() + 1));
    //获取当前日
    System.out.println(date.getDate());
    //获取小时
    System.out.println(date.getHours());
    //获取分钟
    System.out.println(date.getMinutes());
    //获取秒
    System.out.println(date.getSeconds());
  }
}
//输出
2022
  11
  20
  10
  17
  1
```

## 3.4 日期时间转换

toString()`、`toGMTString()`、`toLocaleString()

```java
import java.util.Date;

public class DateTest {
  public static void main(String[] args) {
    //获取当前时间
    Date date = new Date();
    //转换为字符串
    System.out.println(date.toString());
    //转换为GMT时区
    System.out.println(date.toGMTString());
    //转换为本地时区
    System.out.println(date.toLocaleString());
  }
}
//输出
Sun Nov 20 10:19:25 CST 2022
  20 Nov 2022 02:19:25 GMT
    2022-11-20 10:19:25
```

## 3.5 日期时间格式化

> 格式化日期表示将日期/时间格式转换为预先定义的日期/时间格式;
>
> 例如将 `Sun Nov 20 13:48:40 CST 2022` 格式化为 `2022-11-20 13:48:40`

### 3.5.1 `DateFormat`类 进行格式化

日期/时间格式化子类的抽象类，它以与语言无关的方式格式化并解析日期或时间

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726188006174.png" alt="QQ_1726188006174" style="zoom:33%;" />

创建 `DateFormat` 对象时不能使用 new 关键字，而应该使用 DateFormat 类中的静态方法 `getDateInstance()`。

`getDateInstance()`的变种有以下形式

|                             方法                             |                             描述                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|            `static DateFormat getDateInstance()`             |        获取具有默认格式化风格和默认语言环境的日期格式        |
|        `static DateFormat getDateInstance(int style)`        |        获取具有指定格式化风格和默认语言环境的日期格式        |
| `static DateFormat getDateInstance(int style, Locale locale)` |        获取具有指定格式化风格和指定语言环境的日期格式        |
|          `static DateFormat getDateTimeInstance()`           |     获取具有默认格式化风格和默认语言环境的日期/时间格式      |
| `static DateFormat getDateTimeInstance(int dateStyle, int timeStyle)` | 获取具有指定日期/时间格式化风格和默认语言环境的日期/时间格式 |
| `static DateFormat getDateTimeInstance(int dateStyle, int timeStyle, Locale locale)` | 获取具有指定日期/时间格式化风格和指定语言环境的日期/时间格式 |
|            `static DateFormat getTimeInstance()`             |        获取具有默认格式化风格和默认语言环境的时间格式        |
|        `static DateFormat getTimeInstance(int style)`        |        获取具有指定格式化风格和默认语言环境的时间格式        |
| `static DateFormat getTimeInstance(int style, Locale locale)` |        获取具有指定格式化风格和指定语言环境的时间格式        |

格式化样式主要通过 DateFormat 常量设置。将不同的常量传入getDateInstance()的方法中，以控制结果的长度。

DateFormat 类的常量有以下几种

SHORT：完全为数字，如 12.5.10 或 5:30pm。
MEDIUM：较长，如 May 10，2016。
LONG：更长，如 May 12，2016 或 11:15:32am。
FULL：是完全指定，如 Tuesday、May 10、2012 AD 或 11:l5:42am CST

```java

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

public class DateTest {
    public static void main(String[] args) {
        Date date = new Date();
        // 获取不同格式化风格和中国环境的日期
        DateFormat df1 = DateFormat.getDateInstance(DateFormat.SHORT, Locale.CHINA); //public static final int SHORT = 3
        DateFormat df2 = DateFormat.getDateInstance(DateFormat.FULL, Locale.CHINA);
        DateFormat df3 = DateFormat.getDateInstance(DateFormat.MEDIUM, Locale.CHINA);
        DateFormat df4 = DateFormat.getDateInstance(DateFormat.LONG, Locale.CHINA);
       // 获取不同格式化风格和中国环境的时间
        DateFormat df5 = DateFormat.getTimeInstance(DateFormat.SHORT, Locale.ENGLISH);
        DateFormat df6 = DateFormat.getTimeInstance(DateFormat.FULL, Locale.CHINA);
        DateFormat df7 = DateFormat.getTimeInstance(DateFormat.MEDIUM, Locale.CHINA);
        DateFormat df8 = DateFormat.getTimeInstance(DateFormat.LONG, Locale.CHINA);

        // 将不同格式化风格的日期格式化为日期字符串
        String date1 = df1.format(date);
        String date2 = df2.format(date);
        String date3 = df3.format(date);
        String date4 = df4.format(date);
       // 将不同格式化风格的时间格式化为时间字符串
        String time1 = df5.format(date);
        String time2 = df6.format(date);
        String time3 = df7.format(date);
        String time4 = df8.format(date);
        // 输出日期
        System.out.println("SHORT格式：" + date1 + " " + time1);
        System.out.println("FULL格式：" + date2 + " " + time2);
        System.out.println("MEDIUM格式：" + date3 + " " + time3);
        System.out.println("LONG格式：" + date4 + " " + time4);
    }
}
//输出
SHORT格式：22-11-20 2:24 PM
FULL格式：2022年11月20日 星期日 下午02时24分59秒 CST
MEDIUM格式：2022-11-20 14:24:59
LONG格式：2022年11月20日 下午02时24分59秒
```

### 3.5.2 `SimpleDateFormat`类进行格式化

`DateFormat` 类的子类，具有更加强大的日期格式化功能；`DateFormat`只能单独对日期或时间格式化，而 `SimpleDateFormat`可以选择任何用户定义的日期/时间格式的模式

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726188451705.png" alt="QQ_1726188451705" style="zoom:33%;" />

`SimpleDateFormat` 类主要有如下 3 种构造方法。

SimpleDateFormat()：用默认的格式和默认的语言环境构造 SimpleDateFormat。
SimpleDateFormat(String pattern)：用指定的格式和默认的语言环境构造 SimpleDateFormat。
SimpleDateFormat(String pattern,Locale locale)：用指定的格式和指定的语言环境构造 SimpleDateF ormat。

| 字母 |                             描述                             |                             示例                             |
| :--: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  G   |                  纪元标记，根据语言环境显示                  | Locale.CHINA 语言环境下，如：公元；Locale.US语言环境下，如：AD |
|  y   |             年份。yy表示2位年份，yyyy表示4位年份             |      使用yy 表示年份，如22；使用yyyy表示年份，例如2022       |
|  M   | 月份。一般用 MM 表示月份，如果使用 MMM,则会根据语言环境显示不同语言的月份 | 使用 MM 表示的月份，如 11； 使用 MMM 表示月份，在 Locale.CHINA语言环境下，如“十月”；在 Locale.US环境下 如Nov |
|  d   |                月份中的天数。一般用dd表示天数                |                   使用dd表示天数，例如：20                   |
|  h   |           一天中的小时（1~12）。一般使用hh表示小时           | 如 10 (注意 10 有可能是 10 点，也可能是 22 点，取决于AM/PM)  |
|  H   |                     一天中的小时 (0~23)                      |                            如：22                            |
|  m   |                  分钟数。一般用mm表示分钟数                  |                   使用mm表示分钟数，如：59                   |
|  s   |                   秒数。一般使用ss表示秒数                   |                    使用ss表示秒数，如：55                    |
|  S   |                毫秒数。一般使用SSS 表示毫秒数                |                  使用SSS表示毫秒数，如：234                  |
|  E   |      星期几。会根据语言环境的不同，显示不同语言的星期几      | Locale.CHINA 语言环境下，如：星期日；在 Locale.US 语言下，如：Sun |
|  D   |                         一年中的日子                         |                     324 一年中的第324天                      |
|  F   |                        一个月中第几周                        |                    3 表示一个月中的第三周                    |
|  w   |                         一年中第几周                         |                    48 表示一年中的第48周                     |
|  W   |   一个月中第几周（注意与F的区别，具体含义可能依赖于实现）    |                              1                               |
|  a   |    A.M./P.M. 或上午/下午标记。根据语言环境不同，显示不同     | Locale.CHINA 语言环境下，如：下午；Locale.US语言环境下，如：PM |
|  k   |                      一天中的小时(1~24)                      |                     15 一天中的第15小时                      |
|  K   |          一天中的小时(0~11)，通常与 AM/PM 一起使用           | 3 下午3小时（注意这里K的示例可能与描述不完全匹配，因为K通常用于12小时制但不包括12，且通常与AM/PM一起使用，但给出的示例为3且未指明AM/PM） |
|  z   |                             时区                             |                   中国的东八区，如：+0800                    |

```java

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;


public class DateTest {
    public static void main(String[] args)  {
        SimpleDateFormat sdf1 = new SimpleDateFormat("今天是："+"yyyy年MM月dd日 HH点mm分ss秒 SSS 毫秒 E ", Locale.CHINA);
        System.out.println(sdf1.format(new Date()));

        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-mm-dd hh:mm:ss", Locale.CHINA);
        System.out.println(sdf2.format(new Date()));
    }
}
//输出
今天是：2022年11月20日 15点27分37秒 091 毫秒 星期日 
2022-27-20 03:27:37
```

## 3.6 日期字符串互转

### 3.6.1 `DateFormat` 类实现

```java
import java.text.DateFormat;
import java.text.ParseException;
import java.util.Date;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //将日期转为字符串
        Date date1 = new Date();
        DateFormat df1 = DateFormat.getDateInstance(DateFormat.MEDIUM);
        String strdate = df1.format(date1);
        System.out.println(strdate);
        //将字符串转日期
        DateFormat df2 = DateFormat.getDateInstance(DateFormat.MEDIUM);
        Date date2 = df2.parse("1998-19-13");
        System.out.println(date2);
    }
}
//输出
2022-11-20
Tue Jul 13 00:00:00 CST 1999
```

### 3.6.2 `SimpleDateFormat`类实现

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;

public class DateTest {
    public static void main(String[] args) throws ParseException {

        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
        //日期转字符串
        Date date1 = new Date();
        String strdate1 = sdf.format(date1);
        System.out.println(strdate1);

        //字符串转日期
        Date date2 = sdf.parse("1992-09-13 08:34:45");
        System.out.println(date2.toString());
    }
}
//输出
2022-11-20 03:32:21
Sun Sep 13 08:34:45 CST 1992
```

## 3.7 日期时间比较

boolean before(Date when)`、`boolean after(Date when)` 、`boolean equals(Object obj)` 、`int compareTo(Date anotherDate)

```java

public class DateTest {
    public static void main(String[] args) throws ParseException {

        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
        Date date1 = sdf.parse("1992-10-08 11:23:59");
        Date date2 = sdf.parse("1992-09-13 08:34:45");
        Date date3 = sdf.parse("1992-09-13 08:34:45");

        System.out.println("date1:" + sdf.format(date1));
        System.out.println("date2:" + sdf.format(date2));
        System.out.println("date3:" + sdf.format(date3));
        System.out.println("********************************************************************");
        //compareTo方法比较
        System.out.println("compareTo方法比较结果1:"+date1.compareTo(date2));
        System.out.println("compareTo方法比较结果2:"+date2.compareTo(date3));
        System.out.println("compareTo方法比较结果2:"+date2.compareTo(date1));
        System.out.println("-------------------------------------------------------------------");
        //equals方法比较
        System.out.println("equals方法比较结果1:"+date1.equals(date2));
        System.out.println("equals方法比较结果2:"+date2.equals(date3));
        System.out.println("equals方法比较结果2:"+date2.equals(date1));
        System.out.println("-------------------------------------------------------------------");
        //before方法比较
        System.out.println("before方法比较结果1:"+date1.before(date2));
        System.out.println("before方法比较结果2:"+date2.before(date3));
        System.out.println("before方法比较结果2:"+date2.before(date1));
        System.out.println("-------------------------------------------------------------------");
        //after方法比较
        System.out.println("after方法比较结果1:"+date1.after(date2));
        System.out.println("after方法比较结果2:"+date2.after(date3));
        System.out.println("after方法比较结果2:"+date2.after(date1));
    }
}
//输出
date1:1992-10-08 11:23:59
date2:1992-09-13 08:34:45
date3:1992-09-13 08:34:45
********************************************************************
compareTo方法比较结果1:1
compareTo方法比较结果2:0
compareTo方法比较结果2:-1
-------------------------------------------------------------------
equals方法比较结果1:false
equals方法比较结果2:true
equals方法比较结果2:false
-------------------------------------------------------------------
before方法比较结果1:false
before方法比较结果2:false
before方法比较结果2:true
-------------------------------------------------------------------
after方法比较结果1:true
after方法比较结果2:false
after方法比较结果2:false
```

# 四、 第二代日期时间类

由于第一代日期时间类Date 存在几个严重问题：

不能转换时区。除了toGMTString()可以按GMT+0:00输出外，Date总是以当前计算机系统的默认时区为基础进行输出。

我们也很难对日期和时间进行加减，计算两个日期相差多少天，计算某个月第一个星期一的日期等。

由于以上诸多问题，第二代日期时间类Calendar 便来了。

上一小节中我们发现Date 类下的很多方法都废弃了，当然了在Calendar 中有新的方法可以取代

## 4.1 继承关系

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726189312238.png" alt="QQ_1726189312238" style="zoom:50%;" />

## 4.2 获取实例对象

因为 Calendar 类是一个抽象类，创建 Calendar 对象不能使用 new 关键字，但是它提供了一个 getInstance() 方法来获得 Calendar类的对象

```java
//默认是当前日期
Calendar c = Calendar.getInstance();  
```

## 4.3 TimeZone 时区

`Calendar`和`Date`相比，提供了时区类`TimeZone`

```java
import java.text.ParseException;
import java.util.TimeZone;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        // 当前时区
        TimeZone tzDefault = TimeZone.getDefault();
        System.out.println("当前时区：" + tzDefault.getID());

        //获取纽约时区
        TimeZone tzNY = TimeZone.getTimeZone("America/New_York");
        System.out.println("纽约时区：" + tzNY.getID());
    }
}
//输出
当前时区：Asia/Shanghai
纽约时区：America/New_York
```

```java
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.TimeZone;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        // 当前时间:
        Calendar c = Calendar.getInstance();
        // 设置为纽约时区:
        c.setTimeZone(TimeZone.getTimeZone("America/New_York"));
        // 格式化显示时间:
        SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        sdf1.setTimeZone(TimeZone.getTimeZone("America/New_York"));
        System.out.println("纽约时间："+sdf1.format(c.getTime()));
        
        // 设置为北京时区:
        c.setTimeZone(TimeZone.getTimeZone("Asia/Shanghai"));
        // 格式化显示时间:
        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        sdf2.setTimeZone(TimeZone.getTimeZone("Asia/Shanghai"));
        System.out.println("北京时间："+sdf2.format(c.getTime()));
    }
}
//输出
纽约时间：2022-11-22 18:49:08
北京时间：2022-11-23 07:49:08
```

## 4.4 常用时间点的获取

**注：**

- 与中国人习惯不同，一年中1月的值为0。
- 与中国人的习惯不同, 一周中的第一天为 周日. 一周的顺序依次为: 周日(1), 周一(2), 周二(3), 周三(4), 周四(5), 周五(6), 周六(7)

Calendar类中各个常量含义

|           常量           |                             含义                             |
| :----------------------: | :----------------------------------------------------------: |
|     `Calendar.YEAR`      |                             年份                             |
|     `Calendar.MONTH`     | 月份（注意：月份是从0开始的，即0代表1月，1代表2月，以此类推） |
| `Calendar.DAY_OF_MONTH`  |                   日期（一个月中的第几天）                   |
|     `Calendar.DATE`      |   日期，和`DAY_OF_MONTH`字段意义完全相同（另一种表示方式）   |
|     `Calendar.HOUR`      | 12小时制的小时（注意：可能依赖于具体的`Calendar`实现和时区设置） |
|  `Calendar.HOUR_OF_DAY`  |                        24小时制的小时                        |
|    `Calendar.MINUTE`     |                             分钟                             |
|    `Calendar.SECOND`     |                              秒                              |
|  `Calendar.MILLISECOND`  |                             毫秒                             |
|  `Calendar.DAY_OF_WEEK`  | 星期几（周日(1), 周一(2), 周二(3), 周三(4), 周四(5), 周五(6), 周六(7)）。注意：周日的值在不同的地区和语言中可能有所不同，这里给出的是Java中的标准表示） |
| `Calendar.WEEK_OF_YEAR`  | 一年中第几周（基于ISO 8601标准或其他标准，具体取决于`Calendar`的实现） |
| `Calendar.WEEK_OF_MONTH` | 一月中第几周（这个字段的具体含义可能依赖于`Calendar`的实现，因为不同的文化和地区对月份中周的定义可能不同） |

```java
//获取时间点
import java.text.ParseException;
import java.util.Calendar;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //获取当前时间
        Calendar c = Calendar.getInstance();
        //获取年份
        System.out.println("获取年份："+c.get(Calendar.YEAR));
        //获取月份
        System.out.println("获取月份："+(c.get(Calendar.MONTH) + 1));
        //获取一月中的天
        System.out.println("获取一月中的天："+c.get(Calendar.DAY_OF_MONTH));
        System.out.println("获取一月中的天："+c.get(Calendar.DATE));
        //获取一天中的小时 24小时制
        System.out.println("获取一天中的小时（24小时制）："+c.get(Calendar.HOUR_OF_DAY));
        //获取一天中的小时 12小时制
        System.out.println("获取一天中的小时（12小时制）："+c.get(Calendar.HOUR));
        //获取分钟
        System.out.println("获取分钟："+c.get(Calendar.MINUTE));
        //获取秒
        System.out.println("获取秒：" + c.get(Calendar.SECOND));
        //获取毫秒
        System.out.println("获取毫秒：" + c.get(Calendar.MILLISECOND));
        //获取一周中星期几
        System.out.println("获取一周中星期几："+ c.get(Calendar.DAY_OF_WEEK));
        //获取一年中星期几
        System.out.println("获取一年中第几周："+c.get(Calendar.WEEK_OF_YEAR));
        //获取一月中第几周
        System.out.println("获取一月中第几周："+c.get(Calendar.WEEK_OF_MONTH));

        System.out.println("当前时间："+(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-"+ c.get(Calendar.DAY_OF_MONTH) +
                " "+ c.get(Calendar.HOUR_OF_DAY) + ':' + c.get(Calendar.MINUTE) + ':' + c.get(Calendar.SECOND) +
                ':' + c.get(Calendar.MILLISECOND) ));
    }
}
//输出
获取年份：2022
获取月份：11
获取一月中的天：20
获取一月中的天：20
获取一天中的小时（24小时制）：16
获取一天中的小时（12小时制）：4
获取分钟：39
获取秒：37
获取毫秒：238
获取一周中星期几：1
获取一年中第几周：48
获取一月中第几周：4
当前时间：2022-11-20 16:39:37:238
```

## 4.5 常用时间点设置

**注：在使用set方法之前，必须先 clear 一下，否则很多信息会继承自系统当前时间**

### 4.5.1 分别对年、月、日、时、分、秒每个值进行设置

```java
//设置时间点 
import java.text.ParseException;
import java.util.Calendar;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //初始化Calendar类
        Calendar c = Calendar.getInstance();
        //将时间设置成1992-09-13 23:35:59:123
        c.clear();  //先clear一下，否则很多信息会继承自系统当前时间
        c.set(Calendar.YEAR,1992);
        c.set(Calendar.MONTH,9);
        c.set(Calendar.DAY_OF_MONTH,13);
        c.set(Calendar.HOUR_OF_DAY,23);
        c.set(Calendar.MINUTE,35);
        c.set(Calendar.SECOND,59);
        c.set(Calendar.MILLISECOND,123);

        System.out.println("设置好的时间为："+(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-"+ c.get(Calendar.DAY_OF_MONTH) +
                " "+ c.get(Calendar.HOUR_OF_DAY) + ':' + c.get(Calendar.MINUTE) + ':' + c.get(Calendar.SECOND) +
                ':' + c.get(Calendar.MILLISECOND) ));

    }
}
//输出
设置好的时间为：1992-10-13 23:35:59:123
```

### 4.5.2 一起设置年月日、年月日时分、年月日时分秒
set(int year ,int month,int date)
set(int year ,int month,int date,int hour,int minute)
set(int year ,int month,int date,int hour,int minute,int second)

```java
import java.text.ParseException;
import java.util.Calendar;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //初始化Calendar类
        Calendar c = Calendar.getInstance();
        System.out.println("当前时间为："+(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-"+ c.get(Calendar.DAY_OF_MONTH) +
                " "+ c.get(Calendar.HOUR_OF_DAY) + ':' + c.get(Calendar.MINUTE) + ':' + c.get(Calendar.SECOND) +
                ':' + c.get(Calendar.MILLISECOND) ));
        //将时间设置成1992-09-13 23:35:59
        c.clear();
        c.set(1992,9,13,23,35,59);

        System.out.println("设置好的时间为："+(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-"+ c.get(Calendar.DAY_OF_MONTH) +
                " "+ c.get(Calendar.HOUR_OF_DAY) + ':' + c.get(Calendar.MINUTE) + ':' + c.get(Calendar.SECOND) +
                ':' + c.get(Calendar.MILLISECOND) ));
    }
}
//输出
当前时间为：2022-11-20 17:29:27:98
设置好的时间为：1992-10-13 23:35:59:0
```

## 4.6 时间计算

`Calendar.add()` 对时间进行加减运算

```java
import java.text.ParseException;
import java.util.Calendar;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //初始化Calendar类
        Calendar c = Calendar.getInstance();
        System.out.println("当前时间是："+(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-"+ c.get(Calendar.DAY_OF_MONTH) +
                " "+ c.get(Calendar.HOUR_OF_DAY) + ':' + c.get(Calendar.MINUTE) + ':' + c.get(Calendar.SECOND) +
                ':' + c.get(Calendar.MILLISECOND) ));
        c.add(Calendar.DATE,5);
        System.out.println("当前日期+5天后："+(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-"+ c.get(Calendar.DAY_OF_MONTH) +
                " "+ c.get(Calendar.HOUR_OF_DAY) + ':' + c.get(Calendar.MINUTE) + ':' + c.get(Calendar.SECOND) +
                ':' + c.get(Calendar.MILLISECOND) ));

        c.add(Calendar.DATE,-15);
        System.out.println("当前日期-15天后："+(c.get(Calendar.YEAR) + "-" + (c.get(Calendar.MONTH) + 1) + "-"+ c.get(Calendar.DAY_OF_MONTH) +
                " "+ c.get(Calendar.HOUR_OF_DAY) + ':' + c.get(Calendar.MINUTE) + ':' + c.get(Calendar.SECOND) +
                ':' + c.get(Calendar.MILLISECOND) ));

    }
}
//输出
当前时间是：2022-11-20 17:23:57:406
当前日期+5天后：2022-11-25 17:23:57:406
当前日期-15天后：2022-11-10 17:23:57:406
```

## 4.7 获取时间戳（ 获取当前毫秒数 ）

```java
import java.text.ParseException;
import java.util.Calendar;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //初始化Calendar类
        Calendar c = Calendar.getInstance();
        //获取毫秒数
        System.out.println(c.getTimeInMillis());
    }
}
//输出
1668936885795
```

## 4.8 时间日期比较

Calender.before()、Calender.after()、Calender.compareTo() 和 Calender.equals()

```java
import java.text.ParseException;
import java.util.Calendar;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //初始化Calendar类
        Calendar c1 = Calendar.getInstance();
        Calendar c2 = Calendar.getInstance();
        Calendar c3 = Calendar.getInstance();

        c1.clear();
        c2.clear();
        c3.clear();

        //c1设置为1992-09-13 23:35:59
        c1.set(1992,8,13 ,23,35,59);
        //c2设置为1992-10-08 21:21:55
        c2.set(1992,9,8 ,21,21,55);
        //c3设置为1992-10-08 21:21:55
        c3.set(1992,9,8 ,21,21,55);

        System.out.println("c1日期值："+(c1.get(Calendar.YEAR) + "-" + (c1.get(Calendar.MONTH) + 1) + "-"+ c1.get(Calendar.DAY_OF_MONTH) +
                " "+ c1.get(Calendar.HOUR_OF_DAY) + ':' + c1.get(Calendar.MINUTE) + ':' + c1.get(Calendar.SECOND) +
                ':' + c1.get(Calendar.MILLISECOND) ));
        System.out.println("c2日期值："+(c2.get(Calendar.YEAR) + "-" + (c2.get(Calendar.MONTH) + 1) + "-"+ c2.get(Calendar.DAY_OF_MONTH) +
                " "+ c2.get(Calendar.HOUR_OF_DAY) + ':' + c2.get(Calendar.MINUTE) + ':' + c2.get(Calendar.SECOND) +
                ':' + c2.get(Calendar.MILLISECOND) ));

        System.out.println("c3日期值："+(c3.get(Calendar.YEAR) + "-" + (c3.get(Calendar.MONTH) + 1) + "-"+ c3.get(Calendar.DAY_OF_MONTH) +
                " "+ c3.get(Calendar.HOUR_OF_DAY) + ':' + c3.get(Calendar.MINUTE) + ':' + c3.get(Calendar.SECOND) +
                ':' + c3.get(Calendar.MILLISECOND) ));

        System.out.println("******************************************************************");

        //Calender.compareTo() 方法比较
        System.out.println("compareTo方法比较结果1："+c1.compareTo(c2));
        System.out.println("compareTo方法比较结果2："+c2.compareTo(c3));
        System.out.println("compareTo方法比较结果3："+c2.compareTo(c1));

        System.out.println("------------------------------------------------------------------");
        //Calender.equals() 方法比较
        System.out.println("equals方法比较结果1："+c1.equals(c2));
        System.out.println("equals方法比较结果2："+c2.equals(c3));
        System.out.println("equals方法比较结果3："+c2.equals(c1));

        System.out.println("------------------------------------------------------------------");
        //Calender.before() 方法比较
        System.out.println("before方法比较结果1："+c1.before(c2));
        System.out.println("before方法比较结果2："+c2.before(c3));
        System.out.println("before方法比较结果3："+c2.before(c1));

        System.out.println("------------------------------------------------------------------");
        //Calender.after() 方法比较
        System.out.println("after方法比较结果1："+c1.after(c2));
        System.out.println("after方法比较结果2："+c2.after(c3));
        System.out.println("after方法比较结果3："+c2.after(c1));

    }
}
//输出
c1日期值：1992-9-13 23:35:59:0
c2日期值：1992-10-8 21:21:55:0
c3日期值：1992-10-8 21:21:55:0
******************************************************************
compareTo方法比较结果1：-1
compareTo方法比较结果2：0
compareTo方法比较结果3：1
------------------------------------------------------------------
equals方法比较结果1：false
equals方法比较结果2：true
equals方法比较结果3：false
------------------------------------------------------------------
before方法比较结果1：true
before方法比较结果2：false
before方法比较结果3：false
------------------------------------------------------------------
after方法比较结果1：false
after方法比较结果2：false
after方法比较结果3：true
```

