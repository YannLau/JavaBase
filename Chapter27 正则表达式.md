P878---P904

[toc]

> 简单的说：正则表达式是对字符串执行模式匹配的技术。
> 正则表达式：regular expression => RegExp

# 正则表达式基本介绍

> 介绍
>
> 1. 一个正则表达式，就是用某种模式去匹配字符串的一个公式。很多人因
>    为它们看上去比较古怪而且复杂所以不敢去使用，不过，经过练习后，
>    就觉得这些复杂的表达式写起来还是相当简单的，而且，一旦你弄懂它
>    们，你就能把数小时辛苦而且易错的文本处理工作缩短在几分钟（甚至
>    几秒钟）内完成
>
> 2. 老韩这里要特别强调，正则表达式不是只有java才有，实际上很多编程语
> 言都支持正则表达式进行字符串操作！如图所示。

# 正则表达式的底层实现*

为让大家对正则表达式底层实现有一个直观的映象，给大家举个实例 给你一段字符串(文本),请找出所有四个数字连在一起的子串， 比如: 应该找到 1998 1999 3443 9889 ===> 分析底层实现 RegTheory.java

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegTheory {
    public static void main(String[] args) {

        String content = "2006年11月13日，Java技术的发明者Sun公司宣布，将Java" +
                "技术作为免费软件对外发布12345678。Sun公司正式4466发布的有4664关Jav7788a平台标准版9889的第一批源代" +
                "码，以及Java迷你版的可执行源代码2332。从2007年3月起，全世界所有的开发人员均可对" +
                "Java源代码进行修改";
        //目标:匹配所有四个数字
        //说明
        //1. \\d表示一个任意数字
        String regStr = "(\\d\\d)(\\d\\d)";
        //2. 创建模式对象[即正则表达式对象]
        Pattern pattern = Pattern.compile(regStr);
        //3. 创建匹配器
        //说明: 创建匹配器matcher ,按照正则表达式的规则
        //去匹配 content 字符串
        Matcher matcher = pattern.matcher(content);
        //4. 开始匹配
        /**
         * matcher.find() 完成的任务
         * 1. 根据指定的规则，定位满足规则的子字符串（比如2006）
         * 2. 找到后，将 子字符串的开始的索引记录到 matcher对象的属性 int[] groups；
         * groups[0] = 0，把该子字符串的结束的索引+1的值记录到groups[1] = 4
         * 3.同时记录oldLast 的值为 子字符申的结束的 索引+1的值即4，即下次执行find时，就从4开始匹配
         *
         * group(0)分析
         *    源码:
         *    public String group(int group) {
         *         if (first < 0)
         *             throw new IllegalStateException("No match found");
         *         if (group < 0 || group > groupCount())
         *             throw new IndexOutOfBoundsException("No group " + group);
         *         if ((groups[group*2] == -1) || (groups[group*2+1] == -1))
         *             return null;
         *         return getSubSequence(groups[group * 2], groups[group * 2 + 1]).toString();
         *     }
         * 1. 根据 groups[0]=0 和 groups[1]=4 的记录的位置，从content开始截取子字符串返回
         * 就是[0,4)包含0但是不包含索引为4的位置
         *
         * 如果再次指向find方法,仍然按照上面分析来执行,更新groups[0]和groups[1]的值
         *
         * find() 考虑分组
         * 什么是分组? 比如(\\d\\d)(\\d\\d).
         * 正则表达式中有()表示分组,第一个()表示第一组,第二个()表示第二组
         * 找到后，将 子字符串的开始的索引记录到 matcher 对象的属性 int[] groups;
         *    以下为总分结构
         * 2.1 groups[0] = 0，把该子字符串的结束的索引+1的值记录到 groups［1］ = 4
         * 2.2 记录1组（）匹配到的字符串 groups［2］ = 0 groups［3］ = 2
         * 2.3 记录2组（）匹配到的字符串 groups［4］ = 2 groups［5］ = 4
         * 2.4.如果有更多的分组⋯…
         */
        while (matcher.find()) {
            //小结
            //1. 如果正则表达式有()即分组
            //2. 取出匹配的字符串规则如下
            //3. group（0） 表示匹配到的子字符串
            //4. group （1） 表示匹配到的子字符串的第1组字串
            //5. group（2） 表示匹配到的子字符串的第2组字串
            //6. ...但是分组的数不能越界.
            System.out.println("找到了: " + matcher.group(0));
            System.out.println("第一组: " + matcher.group(1));
            System.out.println("第二组: " + matcher.group(2));
        }
    }
}
```

# 正则表达式语法

> •基本介绍
>
> 如果要想灵活的运用正则表达式，必须了解其中各种元字符的功能，元字符从功能上大致分为：
>
> 1. 限定符
>2. 选择匹配符
> 3. 分组组合和反向引用符
> 
> 4. 特殊字符
>
> 5. 字符匹配符
>
> 6. 定位符

> • 元字符（Metacharacter）-转义号 \\\
> \\\符号说明：在我们使用正则表达式去检索某些特殊字符的时候，需要用到转义符号，否则检索不到结果，甚至会报错的。案例：用＄去匹配 “abc\$（” 会怎样？
> 用（去匹配 “abc$（"会怎样？
>
> 再次提示：
> 在Java 的正则表达式中，两个 \\\ 代表其他语言中的一个 \
>
> 元字符-转义符 \\\
>
> 需要用到转移符号的字符有以下: ==} ] / 不是元字符==
>
> .  *  +  (  )  $  | \  ?  [  ^  {  

# 元字符-字符匹配符

> | 符号  | 符号             | 示例   | 解释                                          |
>| ----- | ---------------- | ------ | --------------------------------------------- |
> | [ ]   | 可接收的字符列表 | [efgh] | e,f,g,h中的任意1个字符                        |
> | [ ^ ] | 不接收的字符列表 | [^abc] | 除 a b c 之外的任意1个字符,包括数字和特殊字符 |
> | -     | 连字符           | A-Z    | 任意单个大写字母                              |

![image-20240222下午12533777](/Users/yannlau/Library/Application Support/typora-user-images/image-20240222下午12533777.png)

==\w也可以匹配到下划线，请注意。==

> • 元字符-字符匹配符
> 应用实例：
>
> 1. ［a-z］说明：
>      ［a-z］表示可以匹配a-z中任意一个字符，比如［a-z］、［A-Z］ 去匹配 a11c8 会得到什么结果？
> 2. java正则表达式默认是区分字母大小写的，如何实现不区分大小写
>      • (?i)abc 表示abc都不区分大小写
>        • a(?i)bc 表示bc不区分大小写
>        • a((?i)b)c表示只有b不区分大小写
>        • Pattern pat = Pattern.compile(regEx, Pattern.CASE_INSENSITIVE);
> 3. ［A-Z］表示可以匹配A-Z中任意一个字符。
>      ［0-9］ 表示可以匹配0-9中任意一个字符。
>        这个就不举例说明了.
> 4. [^a-z]说明：
>      \[^a-z\] 表示可以匹配不是a-z中的任意一个字符，比如
>        我们看看［^a-z］ 去匹配 a11c8 会得到什么结果？用［^a-z]{2} 又会得到什么结果呢？
>        [^A-Z\] 表示可以匹配不是A-Z中的任意一个字符。
>        [^0-9\] 表示可以匹配不是0-9中的任意一个字符。
>        这个就不举例说明了.
> 5. ［abcd］ 表示可以匹配abcd中的任意一个字符。
> 6. ［^abcd］ 表示可以匹配不是abcd中的任意一个字符。
>      当然上面的abcd 你可以根据实际情况修改，以适应你的需求
> 7. \\\d表示可以匹配0-9的任意一个数字，相当于［0-9］。
> 8. \\\D 表示可以匹配不是0-9中的任意一个数字，相当于［^0-9］
> 9. \\\w匹配任意大小写英文字符、数字和下划线，相当于［a-zA-20-9_］
> 10. \\\W 相当于［^a-zA-Z0-9_］是\w刚好相反.
> 11. \\\s匹配任何空白字符（空格，制表符、换行\n、回车\r等）
>       1. \n 打印出来是真的换行
>       2. \r打印出来是把光标置到本行的行首
> 12. \\\S 匹配任何非空白字符(不包括\n)，和\\\s刚好相反
> 13. .  匹配除了 `\n (\r  \r\n)`之外的所有字符，如果要匹配 . 本身则需要使用`\\.`

# 元字符-选择匹配符

> 在匹配某个字符串的时候是选择性的，即：既可以匹配这个，又可以匹配那个，这时你需
>要用到 选择匹配符号    |
> 
> | 符号 | 作用                       | 示例   | 解释     |
>| ---- | -------------------------- | ------ | -------- |
> | \|   | 匹配“\|”之前或之后的表达式 | ab\|cd | ab或者cd |

`(#a)|(@b)`==类似这种的也会有分组，不过总是有一个分组打印时会是null。==

# 元字符-限定符

> 用于指定其前面的字符和组合项连续出现多少次![image-20240222下午84158807](/Users/yannlau/Library/Application Support/typora-user-images/image-20240222下午84158807.png)
>
> <u>***细节：java匹配默认贪婪匹配，即尽可能匹配多的***</u>
>
> ![image-20240222下午83349657](/Users/yannlau/Library/Application Support/typora-user-images/image-20240222下午83349657.png)

# 元字符-定位符

> 定位符，规定要匹配的字符串出现的位置，比如在字符串的开始还是在结束的位置，这个也是相当有用的，必须掌握 
>
> 这里的起始字符指的是整个字符串,而不是子字符串
>
> 下图中的 - 不需要转义
>
> \b边界 指的是 `字符串的边界 \t 空格 \r  \n \b \f \s` 
>
> ![image-20240222下午85455864](/Users/yannlau/Library/Application Support/typora-user-images/image-20240222下午85455864.png)

# 元字符-分组

> 捕获分组
>
> ![image-20240222下午100100529](/Users/yannlau/Library/Application Support/typora-user-images/image-20240222下午100100529.png)

```java
package com.hspedu.regexp;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
/**
* @author 韩顺平
* @version 1.0
* 分组:
*/
public class RegExp07 {
  public static void main(String[] args) {
    String content = "hanshunping s7789 nn1189han";
    //下面就是非命名分组
    //说明
    // 1. matcher.group(0) 得到匹配到的字符串
    // 2. matcher.group(1) 得到匹配到的字符串的第 1 个分组内容
    // 3. matcher.group(2) 得到匹配到的字符串的第 2 个分组内容
    //String regStr = "(\\d\\d)(\\d\\d)";//匹配 4 个数字的字符串
    //命名分组： 即可以给分组取名
    String regStr = "(?<g1>\\d\\d)(?<g2>\\d\\d)";//匹配 4 个数字的字符串
    Pattern pattern = Pattern.compile(regStr);
    Matcher matcher = pattern.matcher(content);
    while (matcher.find()) {
      System.out.println("找到=" + matcher.group(0));
      System.out.println("第 1 个分组内容=" + matcher.group(1));
      System.out.println("第 1 个分组内容[通过组名]=" + matcher.group("g1"));
      System.out.println("第 2 个分组内容=" + matcher.group(2));
      System.out.println("第 2 个分组内容[通过组名]=" + matcher.group("g2"));
    }
  }
}
```



> 非捕获分组
>
> ![image-20240222下午101337528](/Users/yannlau/Library/Application Support/typora-user-images/image-20240222下午101337528.png)

```java
package com.hspedu.regexp;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
/**
* @author 韩顺平
* @version 1.0
* 演示非捕获分组, 语法比较奇怪
*/
public class RegExp08 {
  public static void main(String[] args) {
    String content = "hello 韩顺平教育 jack 韩顺平老师 韩顺平同学 hello 韩顺平学生";
    // 找到 韩顺平教育 、韩顺平老师、韩顺平同学 子字符串
    //String regStr = "韩顺平教育|韩顺平老师|韩顺平同学";
    //上面的写法可以等价非捕获分组, 注意：不能 matcher.group(1)
    //String regStr = "韩顺平(?:教育|老师|同学)";
    //找到 韩顺平 这个关键字,但是要求只是查找韩顺平教育和 韩顺平老师 中包含有的韩顺平
    //下面也是非捕获分组，不能使用 matcher.group(1)
    //String regStr = "韩顺平(?=教育|老师)";
    //找到 韩顺平 这个关键字,但是要求只是查找 不是 (韩顺平教育 和 韩顺平老师) 中包含有的韩顺平
    //下面也是非捕获分组，不能使用 matcher.group(1)
    String regStr = "韩顺平(?!教育|老师)";
    Pattern pattern = Pattern.compile(regStr);
    Matcher matcher = pattern.matcher(content);
    while (matcher.find()) {
      System.out.println("找到: " + matcher.group(0));
    }
  }
}
```



> 非贪婪匹配
>
> ![image-20240222下午101832524](/Users/yannlau/Library/Application Support/typora-user-images/image-20240222下午101832524.png)

# 应用实例

> ==汉字(Unicode)==
>
> ```java
> String regStr = "^[\u0391-\uffe5]+$";
> ```
>
> 邮政编码
>
> ```java
> String regStr = "^[1-9]\\d{5}$";
> ```
>
> QQ号码
>
> ```java
> String regStr = "^[1-9]\\d{4,9}$";
> ```
>
> 手机号码
>
> ```java
> String regStr = "^1(?:3|4|5|8)\\d{9}$";
> regStr = "^1[3|4|5|8]\\d{9}$";
> ```
>
> url
>
> ```java
> String s = "https://www.bilibili.com/video/BV1fh411y7R8?p=894&vd_source=b77bfd0640ba59cb7c1bb8b3ad795165";
> 
> String regStr = "((http|https)://)?([\\w-]+\\.)+[\\w-]+(\\/[\\w-?=/%.]*)?$";
> //注意 [.?]在[]内的符号表示的就是符号本身
> ```

```java
package com.hspedu.regexp;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
/**
* @author 韩顺平
* @version 1.0
* 正则表达式的应用实例
*/
public class RegExp10 {
  public static void main(String[] args) {
    String content = "13588889999";
    // 汉字
    //String regStr = "^[\u0391-\uffe5]+$";
    // 邮政编码
    // 要求：1.是 1-9 开头的一个六位数. 比如：123890
    // 2. // 3. //String regStr = "^[1-9]\\d{5}$";
    // QQ 号码
    // 要求: 是 1-9 开头的一个(5 位数-10 位数) 比如: 12389 , 1345687 , 187698765
    //String regStr = "^[1-9]\\d{4,9}$";
    // 手机号码
    // 要求: 必须以 13,14,15,18 开头的 11 位数 , 比如 13588889999
    String regStr = "^1[3|4|5|8]\\d{9}$";
    Pattern pattern = Pattern.compile(regStr);
    Matcher matcher = pattern.matcher(content);
    if(matcher.find()) {
      System.out.println("满足格式");
    } else {
      System.out.println("不满足格式");
    }
  }
}

package com.hspedu.regexp;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
/**
* @author 韩顺平
* @version 1.0
* 演示正则表达式的使用
*/
public class RegExp11 {
  public static void main(String[] args) {
    //String content = "https://www.bilibili.com/video/BV1fh411y7R8?from=search&seid=1831060912083761326";
    String content =
      "http://edu.3dsmax.tech/yg/bilibili/my6652/pc/qg/05-51/index.html#201211-1?track_id=jMc0jn-hm-yHrNfVad37YdhOUh41XY
      mjlss9zocM26gspY5ArwWuxb4wYWpmh2Q7GzR7doU0wLkViEhUlO1qNtukyAgake2jG1bTd23lR57XzV83E9bAXWkStcAh
      4j9Dz7a87ThGlqgdCZ2zpQy33a0SVNMfmJLSNnDzJ71TU68Rc-3PKE7VA3kYzjk4RrKU";
      /**
* 思路
* 1. 先确定 url 的开始部分 https://
* 2.然后通过 ([\w-]+\.)+[\w-]+ 匹配 www.bilibili.com
* 3. /video/BV1fh411y7R8?from=sear 匹配(\/[\w-?=&/%.#]*)?
*/
      //多写多练，多总结
      String regStr = "^((http|https)://)?([\\w-]+\\.)+[\\w-]+(\\/[\\w-?=&/%.#]*)?$";//注意：[. ? *]表示匹配就是.本身
    Pattern pattern = Pattern.compile(regStr);
    Matcher matcher = pattern.matcher(content);
    if(matcher.find()) {
      System.out.println("满足格式");
    } else {
      System.out.println("不满足格式");
    }
    //这里如果使用 Pattern 的 matches 整体匹配 比较简洁
    System.out.println(Pattern.matches(regStr, content));//
  }
}
```



# 正则表达式三个常用类

> java.util.regex 包主要包括以下三个类 Pattern 类、Matcher 类和
> PatternSyntaxException 
>
> - Pattern 类
>
>   pattern 对象是一个正则表达式对象。
>
>   Pattern 类没有公共构造方法。要创建一个 Pattern 对象，调用其公共静态方法，它返回一个 Pattern 对象。该方法接受一个正则表达式作为它的第一个参数，比如：Pattern r= Pattern.compile（pattern）;
>
> - Matcher 类
>   Matcher 对象是对输入字符串进行解释和匹配的引擎。与Pattern 类一样，Matcher 也没有公共构造方法。你需要调用 Pattern 对象的 matcher 方法来获得一个 Matcher 对象
>
> - PatternSyntaxException
>   PatternSyntaxException 是一个非强制异常类，它表示一个正则表达式模式中的语法错误。

> Pattern类的方法matches
>
> 匹配整体
>
> ```java
> String content = "I am hsp from hspedu.com.";
> String pattern = ".*hspedu.*";
> boolean isMatch = Pattern.matches(pattern, content);
> System.out.println（"是否整体匹配成功“+isMatch）；
> ```

```java
package com.hspedu.regexp;
import java.util.regex.Pattern;
/**
* @author 韩顺平
* @version 1.0
* 演示 matches 方法，用于整体匹配, 在验证输入的字符串是否满足条件使用
*/
public class PatternMethod {
  public static void main(String[] args) {
    String content = "hello abc hello, 韩顺平教育";
    //String regStr = "hello";
    String regStr = "hello.*";
    boolean matches = Pattern.matches(regStr, content);
    System.out.println("整体匹配= " + matches);
  }
}
package com.hspedu.regexp;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
/**
* @author 韩顺平
* @version 1.0
* Matcher 类的常用方法
*/
public class MatcherMethod {
  public static void main(String[] args) {
    String content = "hello edu jack hspedutom hello smith hello hspedu hspedu";
    String regStr = "hello";
    Pattern pattern = Pattern.compile(regStr);
    Matcher matcher = pattern.matcher(content);
    while (matcher.find()) {
      System.out.println("=================");
      System.out.println(matcher.start());
      System.out.println(matcher.end());
      System.out.println("找到: " + content.substring(matcher.start(), matcher.end()));
    }
    //整体匹配方法，常用于，去校验某个字符串是否满足某个规则
    System.out.println("整体匹配=" + matcher.matches());
    //完成如果 content 有 hspedu 替换成 韩顺平教育
    regStr = "hspedu";
    pattern = Pattern.compile(regStr);
    matcher = pattern.matcher(content);
    //注意：返回的字符串才是替换后的字符串 原来的 content 不变化
    String newContent = matcher.replaceAll("韩顺平教育");
    System.out.println("newContent=" + newContent);
    System.out.println("content=" + content);
  }
}
```



> 上面的匹配整体底层用的就是下面的matches()方法 !
>
> matches方法为整体匹配 !
>
> Matcher 类
>
> ![image-20240223下午120446857](/Users/yannlau/Library/Application Support/typora-user-images/image-20240223下午120446857.png)

> ![image-20240223下午10853488](/Users/yannlau/Library/Application Support/typora-user-images/image-20240223下午10853488.png)

# 分组扩展->反向引用

> 需求
>
> 请看下面问题：
> 给你一段文本，请你找出所有四个数字连在一起的子串，并且这四个数字要满足①第1位与第4位相同②第2位与第3位相同，比如 1221, 5775, ...

> • 介绍
> 要解决前面的问题，我们需要了解正则表达式的几个概念：
> 1．分组
> 我们可以用圆括号组成一个比较复杂的匹配模式，那么一个圆括号的部分我们可以看作是一个子表达式/一个分组。
> 2．捕获
> 把正则表达式中子表达式/分组匹配的内容，保存到内存中以数字编号或显式命名的组里，方便后面引用，从左向右，以分组的左括号为标志，第一个出现的分组的组号为1，第二个为2，以此类推。组0代表的是整个正则式
> 3． 反向引用
> 圆括号的内容被捕获后，可以在这个括号后被使用，从而写出一个比较实用的匹配模式，这个我们称为反向引用。
>
> 这种引用既可以是在正则表达式内部，也可以是在正则表达式外部，==内部反向引用\\\分组号==，==外部反向引用$分组号==

> 小案例
>
> 1. 要匹配两个连续的相同数字：`(\\d)\\1`
> 2. 要匹配五个连续的相同数字：`(\\d)\\1{4}`
> 3. 要匹配个位与千位相同，十位与百位相同的数5225，1551
>     `(\\d)(\\d)\\2\\1`

```java
package com.hspedu.regexp;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
/**
* @author 韩顺平
* @version 1.0
*/
public class RegExp13 {
  public static void main(String[] args) {
    String content = "我....我要....学学学学....编程 java!";
    //1. 去掉所有的. 
    Pattern pattern = Pattern.compile("\\.");
    Matcher matcher = pattern.matcher(content);
    content = matcher.replaceAll("");
    // System.out.println("content=" + content);
    //2. 去掉重复的字 我我要学学学学编程 java!
    // 思路
    //(1) 使用 (.)\\1+
    //(2) 使用 反向引用$1 来替换匹配到的内容
    // 注意：因为正则表达式变化，所以需要重置 matcher
    // pattern = Pattern.compile("(.)\\1+");//分组的捕获内容记录到$1
    // matcher = pattern.matcher(content);
    // while (matcher.find()) {
    // System.out.println("找到=" + matcher.group(0));
    // }
    //
    // //使用 反向引用$1 来替换匹配到的内容
    // content = matcher.replaceAll("$1");
    // System.out.println("content=" + content);
    //3. 使用一条语句 去掉重复的字 我我要学学学学编程 java!
    content = Pattern.compile("(.)\\1+").matcher(content).replaceAll("$1");
    System.out.println("content=" + content);
  }
}

```

# String 类中使用正则表达式

> 替换
>
> String content = "2000年5月，JDK1.3、JDK1.4和J2SE1.3相继发布，几周后其"+“获得了Apple公司Mac OS X的工业标准的支持。2001年9月24日，J2EE1.3发"+"布。"+“2002年2月26日，J2SE1.K发布。自此Java的计算能力有了大幅提升“；
> //使用正则表达式方式，将 JDK1.3 和 JDK1.4替换成JDK
> `content = content.replaceAll("JDK1\\.3|JDK1\\.4", "JDK");`
> `System.out.println(content) ;`

> 判断功能
>
> `String public boolean matches(String regex);`

> 分割功能
>
> String 类 public String[] split(String regex)
>
> `String content = "hello#abc-jack12smith~北京"`
> 要求按照＃或者 - 或者 ~ 或者 数字 来分割
>
> `String[] split = content.split("#|-|~|\\d+");`

# 本章作业

> 规定电子邮件规则为
> 1. 只能有一个@
> 2. @前面是用户名，可以是a-zA-Z0-9_-字符
> 3. @后面是域名，并且域名只能是英文字母，比如 sohu.com 或者 tsinghua.org.cn
> 4. 写出对应的正则表达式，验证输入的字符串是否为满足规则
>
> ```java
> ^[\\w-]+@([a-zA-Z]+\\.)+[a-zA-Z]+$
> ```

> 要求验证是不是整数或者小数
>
> 提示: 这个题要考虑正数和负数
>
> 比如: 123  -345   34.89  -87.9 -0.01  0.45 等
>
> `String regStr = "^[-+]?([1-9]\\d*|0)(\\.\\d+)?$"`

> 解析URL
>
> `String regStr = "^([a-zA-Z]+)://([a-zA-Z.]+):(\\d+)[\\w-/]*/([\\w.]+)$";`
>
> ```java
> System.out.println（"整体匹配=" + matcher.group（0））；
> System.out.println("协议: " + matcher.group(1));
> System.out.println("域名: " + matcher.group(2));
> System.out.println("端口: " + matcher.group(3));
> System.out.println("文件: " + matcher.group(4));
> ```
>

# 补充-字符类处理细节

在字符类内部，- 表示一个范围（所有Unicode值落在两个边界范围之内的字符）。

如果你需要的是字面意思的 `.*+?{|()[\^$`,则需要在前面加上一个反斜杠\。

在字符类内部，你只需要转义 [ 和 \ 。例如 []^-] 就是 ] ^ - 的三选一

或者你可以用 \Q 和 \E 把字符串括起来。例如 `\(\$0\.99\)` 和 `\Q($0.99)\E` 都可以匹配字符串`($0.99)`。

如果字符串包含正则表达式语法中众多特殊字符的某些字符，那么你可以直接通过调用 `Pattern.quote(str)`对他们进行转义。这样做会直接用`\Q`和`\E`把这个字符串括起来，而且可以处理好str中包含`\E`的特殊情况。



# 补充-标志

标志flag可以改变正则表达式的行为，可以在编译模式时设置标志：

`Pattern pattern = Pattern.compile(regex,Pattern.CASE_INSENSITIVE | Pattern.UNICODE_CHARACTER_CLASS);`

或者，可以在模式中指定它们：

`String regex = "(?iU:expression)";` 等价于 `String regex = "(?iU)expression"`

i:忽略大小写。