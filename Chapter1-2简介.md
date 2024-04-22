[toc]

# JDK的构成

JDK = JRE（Java Runtime Environment） + 开发工具包(javac、java、javadoc etc)

JRE = JVM + Java SE标准（Java 核心类库）

`.java`文件的编译需要 Java 开发工具。

用户使用的话只需要运行`.class`文件所以只需要 JRE。

# 运行.class文件无需带后缀

`.class`字节码文件，JVM 可以识别的字节码文件。

运行.class文件，只需要 `java classname` 不需要携带 `.class` 后缀，是因为实际 JVM 寻找并执行的是当前文件夹下的 `类classname` 而不是该 `classname.class` 文件。

修改源代码后要记得重新编译生成新的.class文件。

源文件经过编译后，每一个类都会生成对应的`.class`文件，即使这些类都在一个源文件中。 Java 编译器识别的是`.java`文件，生成的是文件内的各个类的`.class`文件。而运行时不需要带后缀`.class`说明： JVM 识别的是类而不是`.class`文件。

一个源文件中最多只能有一个public类。<u>**其它类的个数不限，也可以将main方法写在非public类中，然后指定运行非 public 类，这样入口方法就是非public的main方法。**</u>

如果源文件中有 public 类，则与源文件命名必须和类名一致。

# 技术学习路线

> 以下六步是一个循环螺旋上升的过程！

需求：1.工作需要 2.跳槽 3.技术控

看看传统技术是否能解决：1.能解决，但是不完美 2.解决不了

引出我们学习的新技术和知识点

学习新技术或者知识点的基本原理和基本语法（不要考虑细节！）

快速入门（基本程序，CRUD 增删改查）

开始研究技术的注意事项，使用细节，使用规范，如何优化=>没有止境，技术魅力所在 1.2.3.4.5.6.7.8.9.。。。能看到多少个关键点，往后看多少步决定了你的薪资上限

# 转义字符

`\r`：\r 只是代表回车并不代表换行。会将光标放置在输出行的最前方。

```java
jshell> System.out.println("刘云鹏是只小狗\r王美佳");
王美佳是只小狗
```

`\t`：制表符的对齐效果之迷思

```java
jshell> System.out.println("书名\t作者\t价格\t销量\n三国\t罗贯中不中\t120\t1000")
书名	作者	价格	销量
三国	罗贯中不中	120	1000

jshell> System.out.println("书名\t作者\t价格\t销量\n三国\t罗贯中\t120\t1000")
书名	作者	价格	销量
三国	罗贯中	120	1000
```

ChatGPT 解释

是的，制表符 `\t` 是在输出时用于在文本中创建水平制表符的特殊字符。它在控制台输出时通常会产生对齐效果。在你的例子中，由于作者的长度不同，导致了对齐效果的不同。

如果内容过长而导致对齐不良好，你可以考虑使用固定宽度的格式化字符串来更精确地控制输出的格式。例如，可以使用`printf`方法：

```java
javaCopy code
System.out.printf("%-15s%-15s%-10s%-10s\n", "书名", "作者", "价格", "销量");
System.out.printf("%-15s%-15s%-10s%-10s\n", "三国", "罗贯中不中", "120", "1000");
System.out.printf("%-15s%-15s%-10s%-10s\n", "水浒", "施耐庵", "90", "800");
```

在这个例子中，`%-15s` 表示左对齐，总宽度为15的字符串。你可以根据你的需要调整宽度和对齐方式。这样可以确保即使内容长度不同，输出仍然保持对齐。

# 常见错误

最难发现和修改的是业务逻辑错误和环境错误。这种错误运行时不会报错，很难发现和解决。

# 注释

1. 单行注释 `//`

2. 多行注释 `/*    */`

​	**<u>*多行注释中不可以嵌套多行注释！*</u>**

3. 文档注释（<u>***更多内容见notion wiki笔记-javadoc，这一块内容需要在后期继续精进***</u>）

```java
/**
*@author YannLau
*@version 1.0
*@date 2024.2.1
* xxxxxxxx
*/
```

​	文档注释的注释内容可以被JDK提供的工具javadoc 所解析，生成一套以网页文件形式体现的该程序的说明文档，一般写在类中。

`javadoc -d 文件夹名 -xx -yy Demo3.java`

`-d 文件夹名` 表明把生成的 Javadoc 放到该文件夹内。

`-xx -yy`：表示你要生成哪些标签的 javadoc。

```shell
javadoc -d D:\\java_sources\\temp -author -version Comments.java
```

# Java 代码简单规范

**<u>*该内容也需要后期继续精进*</u>**

1. 类、方法的注释，要以javadoc的方式来写。
2. 非Java Doc的注释，往往是给代码的维护者看的，着重告述读者为什么这样写，如何修改，注意什么问题等。
3. 使用tab操作：实现缩进，默认整体向右边移动，时候用shift+tab整体向左移
4. 运算符 和 = 两边习惯性各加一个空格。比如：2 + 4 * 5 + 345 - 89
5. 源文件使用 UTF-8 编码

6. 行宽度不要超过80字符
7. 代码编写次行风格和行尾风格

# 绝对路径和相对路径

相对路径：从当前目录开始定位，形成的一个路径。
绝对路径：从顶级目录c/d，开始定位，形成的路径。

`cd`命令可以使用相对路径来进行切换目录。

`cd ../Music` 该命令会跳转至当前目录的上级目录的 Music 文件夹下。其实就是说 `..`代表了当前目录上级的目录。`../..`代表了上级目录的上级目录。`cd /`直接跳转至根目录。也就是说 `/`代表根目录。

```
  ~ ❯ ls                                                        base  11:03:45 上午  80%
(安装ohmyzsh之前的配置文件)zshrc          IdeaProjects                              Spider
Applications                              Library                                   Zotero
CLionProjects                             Movies                                    anaconda3
Desktop                                   Music                                     courses
Documents                                 Pictures                                  vim-test.txt
Downloads                                 Public
IDEA_Hacked_FILE                          Python_spider_code
  ~ ❯ cd Music                                                  base  11:03:49 上午  80%
  ~/Music ❯ cd ../Public                                        base  11:03:59 上午  80%
  ~/Public ❯
```

# Windows中的用户变量和系统变量

系统变量：对于该计算机上所有的用户起作用的环境变量。

用户变量：进队当前用户起作用的环境变量。

为什么要创建一个变量 `$JAVA_HOME$`来存放 Java 的路径`C:\Program\JDK17`，然后再添加环境变量为`$JAVA_HOME$\bin`，因为这样一些 IDE 默认会寻找系统中的`$JAVA_HOME$`来进行使用，这样设置，当你更改了 JDK 的路径只需要修改一次`$JAVA_HOME$`即可。

