[toc]

# 五、 第三代日期时间
由于第二代日期类也存在着以下问题

可变性：像日期这样的类应该时不可变的
偏移性：年份是从1900开始的，而月份从0开始
格式化：格式化只对Date类有用，Calendar则不行，要格式化只能将Calendar 转换为Date
线程安全性：都不是线程安全的
时区问题：时区处理麻烦
闰秒问题：不能处理闰秒（每隔2天，多出1s）

所以在JDK8中出现了第三代日期时间类，第三代日期时间API具有以下特点

线程安全性： 是线程安全的。不仅没有 setter 方法，而且任何对实例的变更都会返回一个新的实例，保证原来的实例不变
方便性： 提供了大量的方法，简化了日期时间的处理
时区问题：引入了 域 ( domain ) 这个概念对时区进行处理

## 5.1 继承关系

java.time.LocalDate 用于表示 “本地日期”，无 “时间”。LocalDate 不承载时区信息。

java.time.LocalTime 用于表示 “本地时间”，无 “日期”。LocalTime 不承载时区信息。

java.time.LocalDateTime 用于表示 “本地日期与时间”。LocalDateTime 不承载时区信息。

java.time.ZonedDateTime 用于表示承载时区信息的时间

java.time.ZoneId 是引入的新的时区类

LocalDate 实例与 LocalTime 实例能够共同构建 LocalDateTime 实例；

由 LocalDateTime 实例能够获取 LocalDate 实例与 LocalTime 实例。

可以简单地把ZonedDateTime理解成 LocalDateTime 加 ZoneId

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726190595572.png" alt="QQ_1726190595572" style="zoom:50%;" />

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726190602076.png" alt="QQ_1726190602076" style="zoom:50%;" />

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726190612160.png" alt="QQ_1726190612160" style="zoom:50%;" />

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/QQ_1726190622901.png" alt="QQ_1726190622901" style="zoom:50%;" />

## 5.2 获取实例对象

- **通过静态方法** ：`now()`（获取的时间是系统当**前的时间**）
- **通过静态方法**：`of()`（方法参数可以**指定时间**）

```java
import java.text.ParseException;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //获取当前日期时间
        LocalDateTime now = LocalDateTime.now();
        System.out.println("当前日期时间："+now);

        //获取当前日期
        LocalDate nowdate = LocalDate.now();
        System.out.println("当前日期：" + nowdate);

        //获取当前时间
        LocalTime time = LocalTime.now();
        System.out.println("当前时间：" + time);

        System.out.println("********************************************");

        //设置日期时间
        LocalDateTime localdatetime = LocalDateTime.of(1992, 9, 13, 23, 56, 59, 333);
        System.out.println("日期时间设置为：" + localdatetime);

        //设置日期
        LocalDate localdate = LocalDate.of(1992, 9, 13);
        System.out.println("日期设置为：" + localdate);

        //设置时间
        LocalTime localtime = LocalTime.of(23, 56, 59, 333);
        System.out.println("时间设置为：" + localtime);

    }
}
//输出
当前日期时间：2022-11-22T06:34:13.280
当前日期：2022-11-22
当前时间：06:34:13.280
********************************************
日期时间设置为：1992-09-13T23:56:59.000000333
日期设置为：1992-09-13
时间设置为：23:56:59.000000333
```

## 5.3 获取年月日时分秒
getYear()：获取年
getMonth()：获得月份(返回一个 Month 枚举值)
getMonthValue()：获得月份(1-12)
getDayOfMonth()：获得月份天数(1-31)
getDayOfYear()：获得年份天数(1-366)
getHour()：获取小时
getMinute()：获取分钟
getSecond()：获取秒值
now.getNano()：获取纳秒
getDayOfWeek()：获得星期几(返回一个 DayOfWeek枚举值)

```java
import java.text.ParseException;
import java.time.*;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //获取当前日期时间
        LocalDateTime now = LocalDateTime.now();
        System.out.println("当前日期时间："+now);

        //获取年份
        int year = now.getYear();
        System.out.println("当前年份：" + year);

        //获取月份
        Month month = now.getMonth();
        int monthValue = now.getMonthValue();
        System.out.println("当前月份枚举值获取：" + month);
        System.out.println("当前月份值：" + monthValue);

        //获取日
        int dayOfMonth = now.getDayOfMonth();
        int dayOfYear = now.getDayOfYear();
        DayOfWeek dayOfWeek = now.getDayOfWeek();
        System.out.println("一个月中的天：" + dayOfMonth);
        System.out.println("一年中的天：" + dayOfYear);
        System.out.println("一周中的天：" + dayOfWeek);

        //获取小时
        int hour = now.getHour();
        System.out.println("小时：" + hour);

        //获取分钟
        int minute = now.getMinute();
        System.out.println("分钟：" + minute);

        //获取秒
        int second = now.getSecond();
        System.out.println("秒：" + second);

        //获取纳秒
        int nano = now.getNano();
        System.out.println("纳秒：" + nano);
        
    }
}
//输出
当前日期时间：2022-11-22T06:53:29.172
当前年份：2022
当前月份枚举值获取：NOVEMBER
当前月份值：11
一个月中的天：22
一年中的天：326
一周中的天：TUESDAY
小时：6
分钟：53
秒：29
纳秒：172000000
```

## 5.4 日期时间比较
isAfter()：判断一个日期是否在指定日期之后
isBefore()：判断一个日期是否在指定日期之前
isEqual()：判断两个日期是否相同
isLeapYear()：判断是否是闰年（注意是LocalDate类 和 LocalDateTime类特有的方法）
compareTo(): 两个日期比较返回0，1，-1

```java
import java.text.ParseException;
import java.time.*;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //dateTime1时间初始化为1992-09-13 23:35:59
        LocalDateTime dateTime1 = LocalDateTime.of(1992, 9, 13, 23, 35, 59);
        //dateTime2时间初始化为1992-10-08 23:35:50
        LocalDateTime dateTime2 = LocalDateTime.of(1992, 10, 8, 23, 35, 50);
        //dateTime3时间初始化为1992-10-08 23:35:50
        LocalDateTime dateTime3 = LocalDateTime.of(1992, 10, 8, 23, 35, 50);

        System.out.println("******************输出三个时间*****************************");
        System.out.println("dateTime1时间1："+dateTime1);
        System.out.println("dateTime2时间2："+dateTime2);
        System.out.println("dateTime3时间3："+dateTime3);


        //isAfter()方法比较
        System.out.println("------------isAfter()方法比较-------------------");
        System.out.println("dateTime1在dateTime2之后？"+dateTime1.isAfter(dateTime2));
        System.out.println("dateTime2在dateTime3之后？"+dateTime2.isAfter(dateTime3));
        System.out.println("dateTime2在dateTime1之后？"+dateTime2.isAfter(dateTime1));


        //isBefore()方法比较
        System.out.println("------------isBefore()方法比较-------------------");
        System.out.println("dateTime1在dateTime2之前？"+dateTime1.isBefore(dateTime2));
        System.out.println("dateTime2在dateTime3之前？"+dateTime2.isBefore(dateTime3));
        System.out.println("dateTime2在dateTime1之前？"+dateTime2.isBefore(dateTime1));


        //isEqual()方法比较
        System.out.println("------------isEqual()方法比较-------------------");
        System.out.println("dateTime1与dateTime2相等？"+dateTime1.isEqual(dateTime2));
        System.out.println("dateTime2与dateTime3相等？"+dateTime2.isEqual(dateTime3));
        System.out.println("dateTime2与dateTime1相等？"+dateTime2.isEqual(dateTime1));

        //compareTo()方法比较
        System.out.println("------------compareTo()方法比较-------------------");
        System.out.println("dateTime1与dateTime2比较？"+dateTime1.compareTo(dateTime2));
        System.out.println("dateTime2与dateTime3比较？"+dateTime2.compareTo(dateTime3));
        System.out.println("dateTime2与dateTime1比较？"+dateTime2.compareTo(dateTime1));
    }
}
//输出
******************输出三个时间*****************************
dateTime1时间1：1992-09-13T23:35:59
dateTime2时间2：1992-10-08T23:35:50
dateTime3时间3：1992-10-08T23:35:50
------------isAfter()方法比较-------------------
dateTime1在dateTime2之后？false
dateTime2在dateTime3之后？false
dateTime2在dateTime1之后？true
------------isBefore()方法比较-------------------
dateTime1在dateTime2之前？true
dateTime2在dateTime3之前？false
dateTime2在dateTime1之前？false
------------isEqual()方法比较-------------------
dateTime1与dateTime2相等？false
dateTime2与dateTime3相等？true
dateTime2与dateTime1相等？false
------------compareTo()方法比较-------------------
dateTime1与dateTime2比较？-1
dateTime2与dateTime3比较？0
dateTime2与dateTime1比较？1
```

## 5.5 日期时间计算
### 5.5.1 增加年月日时分秒
plusYears(int offset)：增加指定年份
plusMonths(int offset)：增加指定月份
plusDates(int offset)：增加指定日
plusHours(int offset)：增加指定时
plusMinuets(int offset)：增加指定分
plusSeconds(int offset)：增加指定秒
plusNanos(int offset)：增加指定纳秒
plusWeeks(int offset)：增加指定周

```java
import java.text.ParseException;
import java.time.*;

public class DateTest {
    public static void main(String[] args) throws ParseException {
       //获取当前时间
        LocalDateTime now = LocalDateTime.now();

        System.out.println("当前时间：" + now);

        //两年后
        LocalDateTime localDateTime1 = now.plusYears(2);
        System.out.println("两年以后：" + localDateTime1);

        //三个月后
        LocalDateTime localDateTime2 = now.plusMonths(3);
        System.out.println("三个月后：" + localDateTime2);

        //五天后
        LocalDateTime localDateTime3 = now.plusDays(5);
        System.out.println("五天后：" + localDateTime3);

        //4小时后
        LocalDateTime localDateTime4 = now.plusHours(4);
        System.out.println("4小时后："+localDateTime4);

        //30分钟后
        LocalDateTime localDateTime5 = now.plusMinutes(30);
        System.out.println("30分钟后：" + localDateTime5);

        //80秒后
        LocalDateTime localDateTime6 = now.plusSeconds(80);
        System.out.println("80秒后：" + localDateTime6);

        //3周后
        LocalDateTime localDateTime7 = now.plusWeeks(3);
        System.out.println("3周后："+localDateTime7);
    }
}
//输出
当前时间：2022-11-22T07:28:55.129
两年以后：2024-11-22T07:28:55.129
三个月后：2023-02-22T07:28:55.129
五天后：2022-11-27T07:28:55.129
4小时后：2022-11-22T11:28:55.129
30分钟后：2022-11-22T07:58:55.129
80秒后：2022-11-22T07:30:15.129
3周后：2022-12-13T07:28:55.129
```

### 5.5.1 减少年月日时分秒
minusYears(int offset)：减少指定年
minusMonths(int offset)：减少指定月
minusDates(int offset)：减少指定日
minusHours(int offset)：减少指定时
minusMinuets(int offset)：减少指定分
minusSeconds(int offset)：减少指定秒
minusNanos(int offset)：减少指定纳秒
minusWeeks(int offset)：减少指定周

```java
import java.text.ParseException;
import java.time.*;

public class DateTest {
    public static void main(String[] args) throws ParseException {
       //获取当前时间
        LocalDateTime now = LocalDateTime.now();

        System.out.println("当前时间：" + now);

        //10年以前
        LocalDateTime localDateTime1 = now.minusYears(10);
        System.out.println("10年以前：" + localDateTime1);

        //三个月以前
        LocalDateTime localDateTime2 = now.minusMonths(3);
        System.out.println("三个月以前：" + localDateTime2);

        //五天前
        LocalDateTime localDateTime3 = now.minusDays(5);
        System.out.println("五天前：" + localDateTime3);

        //4小时前
        LocalDateTime localDateTime4 = now.minusHours(4);
        System.out.println("4小时前："+localDateTime4);

        //30分钟前
        LocalDateTime localDateTime5 = now.minusMinutes(30);
        System.out.println("30分钟前：" + localDateTime5);

        //80秒前
        LocalDateTime localDateTime6 = now.plusSeconds(80);
        System.out.println("80秒前：" + localDateTime6);

        //3周后
        LocalDateTime localDateTime7 = now.minusWeeks(3);
        System.out.println("3周前："+localDateTime7);

    }
}
//输出
当前时间：2022-11-22T07:33:14.203
10年以前：2012-11-22T07:33:14.203
三个月以前：2022-08-22T07:33:14.203
五天前：2022-11-17T07:33:14.203
4小时前：2022-11-22T03:33:14.203
30分钟前：2022-11-22T07:03:14.203
80秒前：2022-11-22T07:34:34.203
3周前：2022-11-01T07:33:14.203
```

### 5.5.3 时间调节器

可以更加灵活地处理一些复杂的日期，例如：实现日期调整到下个周日、下个工作日，或者是下个月的第一天等等，也可以增加自己定制功能

```java
import java.text.ParseException;
import java.time.*;
import java.time.temporal.TemporalAdjuster;
import java.time.temporal.TemporalAdjusters;

public class DateTest {
    public static void main(String[] args) throws ParseException {
       //获取当前时间
        LocalDateTime now = LocalDateTime.now();

        System.out.println("当前时间：" + now);

        System.out.println("------------------下个月第一天--------------------------");

        //下个月第一天
        TemporalAdjuster temporalAdjuster = TemporalAdjusters.firstDayOfNextMonth();
        LocalDateTime localDateTime1 = now.with(temporalAdjuster);
        System.out.println(localDateTime1);

        System.out.println("------------------下周周五--------------------------");
        //下周周五
        TemporalAdjuster next = TemporalAdjusters.next(DayOfWeek.FRIDAY);
        LocalDateTime localDateTime12 = now.with(next);
        System.out.println(localDateTime12);
        
    }
}
//输出
当前时间：2022-11-22T07:56:56.906
------------------下个月第一天--------------------------
2022-12-01T07:56:56.906
------------------下周周五--------------------------
2022-11-25T07:56:56.906
```

### 5.5.4 with方法使用

withYear(int year):指定年
withMonth(int month):指定月
withDayOfMonth(int dayOfMonth):指定日
withDayOfYear(int dayOfYear):指定日
with(TemporalAdjuster adjuster):指定特殊时间

```java
import java.text.ParseException;
import java.time.*;
import java.time.temporal.TemporalAdjusters;

public class DateTest {
    public static void main(String[] args) throws ParseException {
       //获取当前时间
        LocalDateTime now = LocalDateTime.now();

        System.out.println("当前时间：" + now);
        //指定1992年
        LocalDateTime localDateTime1 = now.withYear(1992);
        System.out.println("指定1992年：" + localDateTime1);

        //指定11月
        LocalDateTime localDateTime2 = now.withMonth(11);
        System.out.println("指定11月份：" + localDateTime2);

        //指定11日
        LocalDateTime localDateTime3 = now.withDayOfMonth(11);
        System.out.println("指定11日：" + localDateTime3);

        //指定特殊日期
        LocalDateTime localDateTime4 = now.with(TemporalAdjusters.firstDayOfMonth());
        System.out.println("指定当月第一天："+localDateTime4);
    }
}
//输出
当前时间：2022-11-22T07:50:02.386
指定1992年：1992-11-22T07:50:02.386
指定11月份：2022-11-22T07:50:02.386
指定11日：2022-11-11T07:50:02.386
指定当月第一天：2022-11-01T07:50:02.386
```

### 5.5.5 计算两个“日期”间隔

`Period` 用于计算两个日期间隔， between() 方法只能接收 LocalDate 类型的参数

- 静态方法 `between()`：计算两个日期之间的间隔
- `getYears()`：获取年份
- `getMonths()`：获取月份
- `getDays()`：获取天数

```java
import java.text.ParseException;
import java.time.*;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //设置生日
        LocalDate birth = LocalDate.of(2004, 10, 8);
        //获取当前时间
        LocalDate now = LocalDate.now();

        //计算两个时间间隔
        Period between = Period.between(birth, now);
        int years = between.getYears();
        int months = between.getMonths();
        int days = between.getDays();
        System.out.println("从出生到现在" + years + "年" + months + "月" + days + "天了");
    }
}
//输出
从出生到现在18年1月15天了
  
  
        Period between = Period.between(LocalDate.now(), LocalDate.of(2000, 6, 11));
        System.out.println(between);
				//计算两个日期的天数间隔
        long daysBetween = Math.abs(ChronoUnit.DAYS.between(LocalDate.now(), LocalDate.of(2000, 6, 11)));
        System.out.println(daysBetween);
```

### 5.5.6 计算两个“时间”间隔

Duration 表示一个时间段，Duration 包含两部分：seconds 表示秒，nanos 表示纳秒，它们的组合表达了时间长度。
因为 Duration 表示时间段，所以 Duration 类中不包含 now() 静态方法。注意，Duration 不包含毫秒这个属性。
Duration只能处理两个LocalTime, LocalDateTime, ZonedDateTime; 如果传入的是LocalDate，将会抛出异常

静态方法 between()：计算两个时间的间隔，默认是秒
toDays()：将时间转换为以天为单位的
toHours()：将时间转换为以时为单位的
toMinutes()：将时间转换为以分钟为单位的
toMillis()：将时间转换为以毫秒为单位的
toNanos()：将时间转换为以纳秒为单位的

```java
import java.text.ParseException;
import java.time.*;

public class DateTest {
    public static void main(String[] args) throws ParseException {
      
        LocalTime beginTime = LocalTime.of(0, 1, 2);
        LocalTime now = LocalTime.now();
        Duration between = Duration.between(beginTime, now);

        long days = between.toDays();
        long hours = between.toHours();
        long minutes = between.toMinutes();
        long millis = between.toMillis();
        long nanos = between.toNanos();

        System.out.println("从0点1分1秒到现在一共" + days + "天" );
        System.out.println("从0点1分1秒到现在一共" + hours + "小时" );
        System.out.println("从0点1分1秒到现在一共" + minutes + "分" );
        System.out.println("从0点1分1秒到现在一共" + millis/1000 + "秒" );
        System.out.println("从0点1分1秒到现在一共" + nanos + "纳秒" );
    }
}
//输出
从0点1分1秒到现在一共0天
从0点1分1秒到现在一共6小时
从0点1分1秒到现在一共381分
从0点1分1秒到现在一共22888秒
从0点1分1秒到现在一共22888801000000纳秒
```

## 5.6 获取时间戳

java.time.Instant 可以获取时间线上的一个瞬时点，精确到纳秒级。

now(): 获取实例化对象。默认获取出来的是默认时区，和我们（东八区）相差8小时
atOffset():设置偏移量
atZone()：获取系统默认时区时间，参数是一个时区的编号(可以通过时区编号类获取ZonedDateTime类的对象)
getEpochSecond()：获取从1970-01-01 00:00:00到当前时间的秒数
toEpochMilli()：获取从1970-01-01 00:00:00到当前时间的毫秒数
getNano()：获取从1970-01-01 00:00:00到当前时间的纳秒数
ofEpochSecond()：给计算机元年增加秒数
ofEpochMilli()：给计算机元年增加毫秒数

```java
import java.text.ParseException;
import java.time.*;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //从1970-01-01 00:00:00 截止到当前时间的毫秒值(时区不是中国的)
        Instant now = Instant.now();
        System.out.println("1970-01-01 00:00:00到默认时区当前日期：" + now);

        //获取当前时区的，我们可以添加偏移量,返回偏移过后的日期
        OffsetDateTime offsetDateTime = now.atOffset(ZoneOffset.ofHours(8));
        System.out.println("1970-01-01 00:00:00到当前时区日期："+offsetDateTime);


        //1970-01-01 00:00:00 截止到当前时间间隔的毫秒值
        long ll = now.toEpochMilli();
        System.out.println("1970-01-01 00:00:00到当前时间间隔的毫秒值：" + ll);

        //1970-01-01 00:00:00 截止到当前时间间隔的秒数
        long epochSecond = now.getEpochSecond();
        System.out.println("1970-01-01 00:00:00 到当前时间间隔的秒数：" + epochSecond);


        //ofEpochMilli() 给计算机元年增加毫秒数
        Instant instant = Instant.ofEpochMilli(1000 * 60 * 60 * 24);
        System.out.println("计算机元年增加1天（86400000毫秒）："+instant);

        //ofEpochSecond() 方法 给计算机元年增加秒数
        Instant instant1 = Instant.ofEpochSecond(60 * 60 * 24);
        System.out.println("计算机元年增加1天（86400秒）："+instant1);

    }
}
//输出
1970-01-01 00:00:00到默认时区当前日期：2022-11-22T23:12:40.988Z
1970-01-01 00:00:00到当前时区日期：2022-11-23T07:12:40.988+08:00
1970-01-01 00:00:00到当前时间间隔的毫秒值：1669158760988
1970-01-01 00:00:00 到当前时间间隔的秒数：1669158760
计算机元年增加1天（86400000毫秒）：1970-01-02T00:00:00Z
计算机元年增加1天（86400秒）：1970-01-02T00:00:00Z
```

## 5.7 带时区的日期时间

`ZonedDateTime`是带有**带有特定时区**的日期时间类，类的方法和用法和 `LocalDateTime` 基本一样。
可以简单地把`ZonedDateTime`理解成`LocalDateTime`加`ZoneId``

``ZoneId`是`java.time`引入的新的时区类

### 5.7.1 获取不同时区当前时间

```java
import java.text.ParseException;
import java.time.ZoneId;
import java.time.ZonedDateTime;
import java.util.Set;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //获取世界各个地方的时区编号
        Set<String> availableZoneIds = ZoneId.getAvailableZoneIds();
        for (String availableZoneId : availableZoneIds) {
            System.out.println(availableZoneId);
        }
        System.out.println("+++++++++++++++++++++++++++++++++++++++++++++++");

        //获取系统默认时区编号
        ZoneId zoneId = ZoneId.systemDefault();
        System.out.println("系统默认时区编号：" + zoneId);
        System.out.println("+++++++++++++++++++++++++++++++++++++++++++++++");

        ZonedDateTime zbj = ZonedDateTime.now(); // 默认时区
        System.out.println("获取默认时区时间："+zbj);

        System.out.println("+++++++++++++++++++++++++++++++++++++++++++++++");
        ZonedDateTime zny = ZonedDateTime.now(ZoneId.of("America/New_York")); // 用指定时区获取当前时间
        System.out.println("获取指定时区时间："+zny);
    }
}
**注：时区不同，但表示的时间都是同一时刻**

//输出
Asia/Aden
America/Cuiaba
Etc/GMT+9
....... 由于时区比较多，这里就不一一例举了
Europe/Athens
US/Pacific
Europe/Monaco
+++++++++++++++++++++++++++++++++++++++++++++++
系统默认时区编号：Asia/Shanghai
+++++++++++++++++++++++++++++++++++++++++++++++
获取默认时区时间：2022-11-23T21:24:03.304+08:00[Asia/Shanghai]
+++++++++++++++++++++++++++++++++++++++++++++++
获取指定时区时间：2022-11-23T08:24:03.305-05:00[America/New_York]
```

### 5.7.2 设置一个时区时间

```java
import java.text.ParseException;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        LocalDateTime localDateTime = LocalDateTime.of(1992, 9, 13, 23, 35, 59);

        ZonedDateTime zonedDateTime1 = localDateTime.atZone(ZoneId.systemDefault());
        ZonedDateTime zonedDateTime2 = localDateTime.atZone(ZoneId.of("America/New_York"));

        System.out.println("默认时区时间："+zonedDateTime1);
        System.out.println("America/New_York时区时间："+zonedDateTime2);
    }
}
//输出
默认时区时间：1992-09-13T23:35:59+08:00[Asia/Shanghai]
America/New_York时区时间：1992-09-13T23:35:59-04:00[America/New_York]
```

**注：它的日期和时间与`LocalDateTime`相同，但附加的时区不同，因此是两个不同的时刻**

### 5.7.3 时区转换

将关联时区转换到另一个时区，转换后日期和时间都会相应调整

```java
import java.text.ParseException;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //以中国时区获取当前时间
        ZonedDateTime now1 = ZonedDateTime.now(ZoneId.of("Asia/Shanghai"));
        //将中国时间转换为美国纽约时间
        ZonedDateTime zonedDateTime = now1.withZoneSameInstant(ZoneId.of("America/New_York"));

        System.out.println("中国时区当前时间：" + now1);
        System.out.println("将中国时间转换为美国纽约时间："+zonedDateTime);

    }
}
//输出
中国时区当前时间：2022-11-23T21:43:31.065+08:00[Asia/Shanghai]
将中国时间转换为美国纽约时间：2022-11-23T08:43:31.065-05:00[America/New_York]
```

### 5.7.4 日期时间格式化

第一代`Date`对象用`SimpleDateFormat`进行格式化显示，而`DateTimeFormatter`用于解析第三代日期字符串和格式化日期输出

```java
import java.text.ParseException;
import java.time.LocalDateTime;

import java.time.format.DateTimeFormatter;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //指定日期格式
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
        LocalDateTime now = LocalDateTime.now();

        String format = dateTimeFormatter.format(now);
        System.out.println("未格式化日期显示：" + now);
        System.out.println("当前日期格式化后：" + format);

    }
}
//输出
未格式化日期显示：2022-11-23T21:51:00.197
当前日期格式化后：2022-11-23 09:51:00
```

### 5.7.5 日期字符串转日期

```java
import java.text.ParseException;
import java.time.LocalDate;
import java.time.LocalDateTime;

import java.time.format.DateTimeFormatter;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //指定日期格式
        DateTimeFormatter dateTimeFormatter1 = DateTimeFormatter.ofPattern("yyyy-MM-dd");
        DateTimeFormatter dateTimeFormatter2 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        LocalDate localDate = LocalDate.parse("1992-09-13", dateTimeFormatter1);
        System.out.println("字符串转LocalDate：" + localDate);
      
        LocalDateTime localDateTime = LocalDateTime.parse("1992-09-13 23:35:59", dateTimeFormatter2);
        System.out.println("字符串转LocalDateTime：" + localDateTime);
    }
}
//输出
字符串转LocalDate：1992-09-13
字符串转LocalDateTime：1992-09-13T23:35:59
```

# 六、三代日期之间相互转换

由于历史遗留问题，有时候我们不得不与遗留代码打交道。这时就涉及了三代日期时间之间的相互转换

**注：** 在转换中我们需要特别注意，java8之前`Date`是包含**日期和时间**的，而`LocalDate`只包含**日期**，`LocalTime`只包含**时间**，所以与`Date`与之互转过程中，会丢失**日期**或者**时间**。最好是转换成`LocalDateTime`,这样就不存在日期或时间丢失问题

## 6.1 Calendar 与 Date 互转

### 6.1.1 Date 转换为Calendar

```java
import java.text.ParseException;
import java.util.Calendar;
import java.util.Date;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //Date 转换为Calendar
        Date date = new Date();
        System.out.println("当前Date时间为："+date);
        Calendar cal= Calendar.getInstance();
        cal.setTime(date);
        System.out.println("Date转换为Calendar后："+(cal.get(Calendar.YEAR) + "-" + (cal.get(Calendar.MONTH) + 1) + "-"+ cal.get(Calendar.DAY_OF_MONTH) +
                " "+ cal.get(Calendar.HOUR_OF_DAY) + ':' + cal.get(Calendar.MINUTE) + ':' + cal.get(Calendar.SECOND)));

    }
}
//输出
当前Date时间为：Fri Nov 25 07:17:06 CST 2022
Date转换为Calendar后：2022-11-25 7:17:6
```

### 6.1.2 Calendar 转换为Date

```java
import java.text.ParseException;
import java.util.Calendar;
import java.util.Date;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //Calendar 转换为Date
        Calendar cal=Calendar.getInstance();
        System.out.println("Calendar获取的当前时间是："+(cal.get(Calendar.YEAR) + "-" + (cal.get(Calendar.MONTH) + 1) + "-"+ cal.get(Calendar.DAY_OF_MONTH) +
                " "+ cal.get(Calendar.HOUR_OF_DAY) + ':' + cal.get(Calendar.MINUTE) + ':' + cal.get(Calendar.SECOND)));

        Date date=cal.getTime();
        System.out.println("当前Date时间为："+date);
    }
}
//输出
Calendar获取的当前时间是：2022-11-25 7:17:29
当前Date时间为：Fri Nov 25 07:17:29 CST 2022
```

## 6.2 Date和Instant互换

### 6.2.1 Date转为Instant

```java
import java.text.ParseException;
import java.time.Instant;
import java.util.Date;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //Date转为Instant
        Date date = new Date();
        System.out.println("当前Date获取的时间是："+date);
        Instant instant = date.toInstant();
        System.out.println("转为Instant后的时间是："+instant);
    }
}
//输出
当前Date获取的时间是：Fri Nov 25 07:27:13 CST 2022
转为Instant后的时间是：2022-11-24T23:27:13.860Z
```

### 6.2.2 Instant转为Date

```java
import java.text.ParseException;
import java.time.Instant;
import java.util.Date;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //Instant转为Date
        Instant instant  = Instant.now();
        System.out.println("Instant获取的时间是："+instant);
        Date date = Date.from(instant);
        System.out.println("转换为Date后："+date);
    }
}
//输出
Instant获取的时间是：2022-11-24T23:29:18.857Z
转换为Date后：Fri Nov 25 07:29:18 CST 2022
```

## 6.3 Date与LocalDateTime互转

### 6.3.1 Date转LocalDateTime

```java
import java.text.ParseException;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.util.Date;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //Date 转LocalDateTime
        Date date = new Date();
        System.out.println("Date获取的当前时间" + date);

        LocalDateTime localDateTime = date.toInstant().atZone(ZoneId.systemDefault()).toLocalDateTime();
        System.out.println("第一种方式转为LocalDateTime后: " + localDateTime);

        LocalDateTime localDateTime1 = LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault());
        System.out.println("第二种方式转为localDateTime2: " + localDateTime1);


    }
}
//输出
Date获取的当前时间Fri Nov 25 07:35:04 CST 2022
第一种方式转为LocalDateTime后: 2022-11-25T07:35:04.379
第二种方式转为localDateTime2: 2022-11-25T07:35:04.379
```

### 6.3.2 LocalDateTime 转 Date

```java
import java.text.ParseException;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.util.Date;

public class DateTest {
    public static void main(String[] args) throws ParseException {
        //LocalDateTime 转 Date
        LocalDateTime localDateTime = LocalDateTime.now();
        System.out.println("localDateTime获取的当前时间: " + localDateTime);

        Date date = Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());
        System.out.println("转为Date后的时间: " + date);

    }
}
//输出
localDateTime获取的当前时间: 2022-11-25T07:36:58.216
转为Date后的时间: Fri Nov 25 07:36:58 CST 2022
```

# 七、阿里巴巴关于日期时间编程规约

> 阿里巴巴java开发手册，第一章1.5小结对日期时间做了相关规约。这里的规约也值得我们每个开发者遵守

①【强制】在日期格式化时，传入pattern中表示年份统一使用小写y

>  日期格式化时，yyyy表示当天所在的年，大写的YYYY表示当天所在的周所属的年份。一周以周日开始，周六结束，只要本周跨年，返回的YYYY就是下一年。

```java
//正例
new SimpleDateFormat("yyyy-MM-dd HH:mm:ss")
```

②【强制】在日期格式中，分清楚**大写的M**和**小写的m**、**大写的H**和**小写的h**分别代表的含义

- 表示月份，用大写M
- 表示分钟，用小写m
- 表示24小时制，用大写H
- 表示12小时制，用小写h

③ 【强制】获取当前毫秒数，是System.currentTimeMillis();而不是new Date().getTime()。

> 说明：如果像获取更加精确的纳秒级时间值，则使用System.nanoTime() 的方法。在JDK8中，针对时间等场景，推荐使用Instant类

④ 【强制】不允许在程序的任何地方使用：
1）java.sql.Date;

2) java.sql.Time;
3) java.sql.Timestamp;

> 说明：第1个不记录时间，`getHours()` 抛出异常；
> 第2个不记录日期，`getYear()` 抛出异常
> 第3个在构造方法`supper((time/1000)*1000)`时，在Timestamp属性henanos中分别存贮秒和纳秒信息

⑤【强制】不要在程序中写死一年未365天，避免在闰年时出现日期转换错误或程序逻辑错误。

//正列

```java
//获取几年的天数
int days =localDate.now().lengthOfYear();
//获取某年的天数
Localdate.of(2022,1,2).lengthOfYear();
```

//反列

```java
//第一种情况：在闰年（366天）时，出现数组越界异常
int[] dayArray = new int[365]
//第二种情况：一年有效期会员制，今年1月26日注册，
//一年未365天，返回的却是1月25日
Calendar calendar = Calendar.getInstance();
calendar.set(2022,0,26);
calendar.add(Calendar.DATE,365);
```

⑥ 【推荐】避免出现闰年2月问题。闰年2月有29天，一年后的那一天不可能是2月29日。
⑦ 【推荐】使用枚举值指代月份（month）取值在0~11之间

> 说明：参考JDK原生注释：Month value is 0-based.e.g.0 for January
> //正例
> 用Calendar.JANUARY、Calendar.FEBRUARY、Calendar.MARCH等指代月份，进行传参或比较

# 八、夏令时

java第三代日期时间类进行时区转换时，比如北京时间改为纽约时间，会自动考虑夏令时吗？

ChatGPT

是的，Java 的第三代日期时间 API（从 Java 8 引入的 `java.time` 包）在进行时区转换时，会自动考虑夏令时（DST，Daylight Saving Time）。Java 使用的 `ZoneId` 类表示一个时区，该时区包括了夏令时的规则。

当你从一个时区（例如北京时间 `Asia/Shanghai`）转换到另一个时区（例如纽约时间 `America/New_York`）时，Java 会自动处理夏令时，确保转换后的时间是准确的。

举个例子：

```java
import java.time.LocalDateTime;
import java.time.ZonedDateTime;
import java.time.ZoneId;

public class TimeZoneConversion {
    public static void main(String[] args) {
        // 创建一个北京的时间
        LocalDateTime localDateTime = LocalDateTime.now();
        ZonedDateTime beijingTime = ZonedDateTime.of(localDateTime, ZoneId.of("Asia/Shanghai"));
        System.out.println("北京时间: " + beijingTime);

        // 转换为纽约时间
        ZonedDateTime newYorkTime = beijingTime.withZoneSameInstant(ZoneId.of("America/New_York"));
        System.out.println("纽约时间: " + newYorkTime);
    }
}
```

输出的结果会自动根据纽约的夏令时规则调整时间。例如，如果转换的日期是在夏令时期间，纽约时间会比标准时间多加一小时。

因此，无需手动处理夏令时，Java 会根据时区的定义和日期自动处理。

##### 您说：

如果确定不需要进行夏令时的转换应该怎么办？

ChatGPT

如果你确定在时区转换时不希望考虑夏令时，可以使用`ZoneOffset`而不是`ZoneId`来进行时间转换。`ZoneOffset`是一个固定的时区偏移量，不会包含夏令时的调整逻辑。例如，如果你希望使用固定的 UTC 偏移时间，不受夏令时影响，可以这样做：

```java
import java.time.LocalDateTime;
import java.time.OffsetDateTime;
import java.time.ZoneOffset;

public class NoDaylightSavingTime {
    public static void main(String[] args) {
        // 创建一个当前的本地时间
        LocalDateTime localDateTime = LocalDateTime.now();
        
        // 假设北京时间为 +08:00 的固定偏移，不使用夏令时
        OffsetDateTime beijingFixedTime = localDateTime.atOffset(ZoneOffset.ofHours(8));
        System.out.println("北京时间（不考虑夏令时）: " + beijingFixedTime);

        // 假设纽约时间为 -05:00 的固定偏移，不使用夏令时
        OffsetDateTime newYorkFixedTime = beijingFixedTime.withOffsetSameInstant(ZoneOffset.ofHours(-5));
        System.out.println("纽约时间（不考虑夏令时）: " + newYorkFixedTime);
    }
}
```

在这个例子中：

- `ZoneOffset.ofHours(8)` 表示 UTC+08:00 的固定偏移（例如北京时间的标准时区）。
- `ZoneOffset.ofHours(-5)` 表示 UTC-05:00 的固定偏移（例如纽约时间，不受夏令时影响）。

通过使用 `ZoneOffset`，你可以确保时间不会因为夏令时而发生变化，无论是在转换时还是在任何时区处理过程中。
