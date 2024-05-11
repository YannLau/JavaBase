[toc]

# 基础概念

## Java Char类型（特殊的\u）

char类型原本用于表示单个字符。不过，现在情况已经有所变化。如今，有些Unicode字符可以用一个char值描述，另外一些Unicode字符则需要两个char值。

char类型的字面量要用单引号括起来。char类型的值可以表示为十六进制值，范围从`\u0000 ~ \uffff`。

除了转义字符`\u`之外，还有一些用于表示特殊字符的转义序列，如 `\n`。可以在加引号的字符字面量或者字符串中使用这些转义序列。e.g. `'\u2131'`或者`"hello\n"`。转义序列`\u`还可以在加引号字符常量或者字符串之外使用（而其它的所有转义字符不可以）。

`public static void main(String\u005B\u005D args)`完全合法，\u005B和\u005D分别是’`[和]`‘的编码。

## 码点

码点是指与一个编码表中的某个字符对应的代码值。在Unicode标准中，码点采用十六位进制书写，并加上前缀`U+`，例如 U+0041就是拉丁字母 A 的码点。Unicode的码点可以分成17个代码平面（code plane）。第一个代码平面称为 ==基本多语言平面（base multilingual plane）==，包括码点从 `\u+0000 到 \u+ffff` 的 “经典” Unicode代码；其余的16个平面的代码从 `U+10000`到`U+10FFFF`，包含各种辅助字符（supplementary character）。

## 代码单元（码元）

UTF-16编码采用不同的代码表示所有Unicode码点。在基本的多语言平面中，每个字符用16位表示，通常称为==代码单元==，而辅助字符编码为一对连续的代码单元。采用这种`编码对`表示的每个值，都属于 基本多语言平面`\u+0000 到 \u+ffff` 中未用的2048个值范围，通常称为==替代区域（surrogate area）==(U+D800~U+DBFF 共计 0X3FF+1 = 1023+1=1024 用于第一个代码单元，U+DC00~UDFFF 共计0X3FF+1 = 1023+1 =1024 用于第二个代码单元)。

这样的设计很巧妙，因为我们很快可以知道一个代码单元是一个字符的编码还是一个辅助字符的第一或者第二部分。例如，𝕆是八元数集的数学符号，码点为U+1D546,编码为两个代码单元U+D835和U+DD46。

关于编码的具体算法描述见 https://tools.ietf.org/html/rfc2781

Java中，char类型描述了采用UTF-16编码的一个代码单元。

# UTF-16编码方式

看我搞定UTF-16的编码！

> 首先要知道以下的背景知识：
>
> - Unicode代码的码点范围目前为 ==U+000000 ~ U+10FFFF==,其实就是 ==0b0 ~ 0b100001111111111111111== 最多21个比特位，所以四字节轻松足够存下。
> - BMP对应的码点范围是 ==U+0000 ~ U+FFFF==，其实就是 ==0b0 ~ 0b1111111111111111==，刚好两个字节，共$2^{16}$个。
> - 那么超过U+FFFF的辅助字符集便是==U+10000 ~ U+10FFFF== 共有 0xFFFFF + 1 = 0x100000 = 1048576 个 = 0b 1 0000 0000 0000 0000 0000 个 = $2^{20}$ 需要 20个bit位 来存储。
> - 由于一个代码单元是16位，两字节，正好可以覆盖BMP，但为了映射到辅助字符集，需要用两个代码单元来表示。
> - 如何映射？ 因为辅助字符集需要20位来存储，分摊到两个码元，一个代码单元要存十个bit位。也因此，大于 U+FFFF 的Unicode码点要先减去 U+10000（减去后得到0——FFFFF范围内的值），这样换算成二进制刚好能够被20bit覆盖。然后分成高10位和低10位两部分。
> - 为了迁就与辅助字符集，BMP必须要做出牺牲，必须要留出2048个码点的空位来存放这两个10bit。不然会造成歧义。
> - 于是规定了：0b 110110 0000000000 ~ 0b 110110 1111111111 共1024个来存储 高10位。其实就是 0xD800 ~ 0xDBFF
> - 0b 110111 0000000000 ~ 0b 110111 1111111111 共1024个来存储 低10位。其实就是 0xDC00 ~ 0x DFFF
> - 学习过霍夫曼编码的人应该知道前缀码，这里正是因为前缀码的原理因此不会造成任何歧义了，110110是高位代理的前缀，110111是低位代理的前缀。
> - 也因此以上范围BMP就不对应任何字符了，被称为 surrogate替代区域。
> - 当解码时发现是以上范围的，也就知道要读取下一个码元才能确定到底是哪一个字符（确定Unicode）。

有了以上背景知识，解码编码水到渠成。

解码

1. 给定Unicode编码码点值。如果小于 0xFFFF，并且不在 surrogate替代区域 内，那么就是BMP，没什么好说的。
2. 如果落在高位替代区域，那么 (当前码元减去 0xD800 后 左移10位） 加上 （下一个码元 减去 0xDC00），这不就拼成一个20位的bit串了。 $0 —— 2^{20}-1$。
3. 然后加上 0x10000 就得到其对应的 Unicode码点值了。
4. 如果上来就落在低位替代区，那么说明有问题啊，具体怎么处理，是跳过它读下一个码元还是直接开始乱码了我也不清楚。

编码就是上面的逆过程。

# UTF-8

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240424下午32552394.png" alt="image-20240424下午32552394" style="zoom:50%;" />

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240424下午32605521.png" alt="image-20240424下午32605521" style="zoom:50%;" />

# 源码编码问题与机制-GPT

You：不考虑java源代码对于人类的可读性问题，源码完全可以用`\u????`形式的转义字符来编写，对吗？

ChatGPT：是的，Java源代码可以使用Unicode转义字符`\u????`来表示字符。这种形式的转义字符允许你在Java源代码中表示任何Unicode字符，即使它们不可打印或不可见。这在处理多语言字符集或特殊符号时可能很有用。不过，这种方式可能会降低源代码的可读性，因为它会使代码变得更加晦涩和难以理解。因此，通常建议在代码中使用Unicode转义字符时，应该保持适度，以确保代码的可读性和易维护性。

You

这涉及到的底层机制是什么，请详细讲解

ChatGPT

在Java中，源代码中的Unicode转义字符`\u????`是一种表示Unicode字符的方式。这个机制涉及到Java编译器和Java虚拟机（JVM）两个层面的处理。

1. **编译器层面**： 当你在Java源代码中使用`\u????`表示Unicode字符时，编译器会将这些转义字符替换为相应的Unicode字符。这个过程发生在源代码被编译成字节码之前。编译器会扫描源代码，找到所有的`\u????`转义字符，然后将它们替换为对应的Unicode字符。这样，最终生成的字节码就包含了实际的Unicode字符，而不是转义字符。
2. **虚拟机层面**： JVM在执行Java程序时会处理包含Unicode字符的字节码。它会根据Unicode字符的编码来正确地处理字符串和其他数据。这意味着，即使源代码中使用了`\u????`形式的转义字符，最终在程序运行时，JVM都会正确地处理Unicode字符，使得程序能够按预期工作。

总的来说，Unicode转义字符`\u????`在Java中是一种方便的方式来表示Unicode字符，但在编译器和虚拟机层面都需要相应的处理来确保源代码正确地转换为最终执行的Unicode字符。

You

如果\u后面跟的十六进制数值可以超过ffff吗？

ChatGPT

不可以。在Java中，`\u`转义序列后面必须紧跟着四个十六进制数字（即一个16位的十六进制数），范围从`\u0000`到`\uffff`（即从0到65535），用来表示Unicode字符的代码点。这个范围涵盖了基本的多语言平面（Basic Multilingual Plane，BMP）中的所有Unicode字符。

如果要表示超出BMP范围的Unicode字符（例如，辅助平面字符），则需要使用代理对（surrogate pairs），这需要两个`\u`转义序列来表示一个字符。例如，一个辅助平面字符的代码点可以通过两个16位的十六进制数（即两个`\u`转义序列）来表示。

You

也就是说java把这些\u转义字符当做utf-16的编码方式来进行解读？

ChatGPT

是的，你说得对。Java源代码中的`\u`转义字符是按照UTF-16编码方式进行解读的。UTF-16是一种Unicode字符编码方式，它使用16位编码单元来表示大多数常见的Unicode字符。因此，Java编译器会将源代码中的`\u`转义序列解释为UTF-16编码中相应的Unicode字符。

在Java中，字符串（String）是以UTF-16编码形式存储的。这意味着，当Java程序在运行时创建字符串时，其中的Unicode字符会被存储为UTF-16编码的形式。这样，在Java程序中使用`\u`转义字符表示Unicode字符时，它们会被编译器解释为UTF-16编码，并在程序执行时以相应的方式处理。

You

我想具体了解，我编写java源代码的文件格式为utf-8，这也代表了源代码中出现的'\u????'的每一个字符在文件中其实是存储为utf-8的编码的，包括我在源代码中显示声明的字符串。所以java具体是如何处理源代码的？

ChatGPT

当你编写Java源代码文件时，如果使用UTF-8编码（这是一种常见的Unicode字符编码方式），那么源代码中的所有字符都会以UTF-8编码形式存储在文件中，包括`\u????`转义序列和任何明确声明的字符串。

Java编译器在处理源代码文件时会按照指定的字符编码方式（通常是UTF-8）读取文件内容。当编译器遇到`\u????`转义序列时，它会解释这些序列并将它们转换为相应的Unicode字符。这个转换过程发生在编译器将源代码转换为抽象语法树（Abstract Syntax Tree，AST）时。

对于字符串字面量，编译器也会按照指定的字符编码方式（例如UTF-8）解释字符串中的每个字符，并将其转换为相应的Unicode字符。这意味着在源代码中显式声明的字符串中的字符也会被正确地解释和处理。

总的来说，Java编译器会根据指定的字符编码方式（例如UTF-8）来处理源代码文件，包括解释`\u????`转义序列和字符串字面量中的字符，并将它们转换为相应的Unicode字符。

You

"相应的Unicode字符" 这里的unicode的具体存在形式是什么？是原本的unicode值，还是经过编码的utf-16？

ChatGPT

Java中源代码中的Unicode字符存在于Java字符串的形式中，而Java字符串使用UTF-16编码来表示Unicode字符。当编译器解释源代码中的`\u????`转义序列时，它会将这些序列转换为相应的Unicode字符，并将它们存储为UTF-16编码的形式。因此，在Java程序中，源代码中的Unicode字符实际上以UTF-16编码的形式存在。

当源代码中的字符串字面量明确声明时，其中的字符也会以UTF-16编码的形式存储。这意味着，在Java程序执行时，所有的字符串都会以UTF-16编码的形式表示，包括源代码中的字符串字面量和通过`\u????`转义序列表示的Unicode字符。

You

那么其他的语句呢？比如关键字while、for、return等语句，它们在编译的过程中需要转换编码格式吗？

ChatGPT

关键字（例如`while`、`for`、`return`等）和其他Java保留字在编译过程中不需要转换编码格式。它们在源代码文件中以普通的文本形式存在，并不包含Unicode转义字符。这些关键字是Java编译器的一部分，它们在编译器中已经被识别为特殊的语法标记。因此，在编译过程中，Java编译器会直接解析这些关键字而不需要进行编码格式的转换。

编译器会根据Java语法规则识别和解释关键字，并将它们转换为相应的语法结构，以构建抽象语法树（AST）。在这个过程中，并不涉及关键字的编码格式转换，因为它们已经以普通的文本形式存在于源代码中，并被编译器所识别和处理。

You

那么那些括号呢？

ChatGPT

括号（例如圆括号 `()`、花括号 `{}`、方括号 `[]` 等）在Java源代码中也是以普通的文本形式存在的，并不包含Unicode转义字符。它们用于标识代码块、方法参数、数组等不同的语法结构。

在编译过程中，Java编译器会直接解析这些括号，并根据语法规则将它们转换为相应的语法结构，如方法调用、控制流语句等。与关键字类似，这些括号不需要进行编码格式的转换，因为它们已经以普通的文本形式存在于源代码中，并被编译器所识别和处理。

因此，括号在编译过程中被视为代码的一部分，用于标识语法结构，而不需要额外的编码格式转换。

# Java源码编码问题与机制-UP主

见Notion笔记-更多内容请见计算机编码专题

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240425下午33900929.png" alt="image-20240425下午33900929" style="zoom:50%;" />

源代码中的字符串字面量（string literals）——> 在编译后，以“魔改的UTF-8（Modified UTF-8）"的编码方式存储在字节码中。

![image-20240425下午33953873](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240425下午33953873.png)

## Modified UTF-8

![image-20240425下午34030547](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240425下午34030547.png)

## 万”码”归宗

![image-20240425下午34052182](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240425下午34052182.png)

- 源文件编码

开发人员指定的，强烈建议使用UTF8

- 编译器编码

使用哪种字符编码来读取解析源代码文件 通过 javac 命令中的参数 -encoding 来指定

- 字节码编码

不变的 Modified UTF-8

- JVM内存编码

UTF-16

- 输出流的编码

• System.out：使用的是JVM的默认编码 

• HttpServletResponse：由开发人员指定 

• HttpServletResponse. setCharacterEncoding(••) 

• HttpServletResponse. setContentType (••)

- 显示编码

  - 控制台：通常是系统的默认编码
  - 浏览器：自动检测 V Http响应中的 Content-type 头 V HTML文档中的〈meta charset>标签

  如果出现乱码： • 要么是上图1和2的编码不兼容 • 要么是上图5和6的编码不兼容

> 我发现了一个问题，嗯，如果你用的是UTF-8去写源码，然后用的是GBK的方式去编译的话是不会报错的，因为原文件中的关键字等控制语句都是ASCII可以包含的，并且UTF-8、GBK在那一部分都是兼容的， 因此解析不会出现问题，唯一会出现问题的就是你的字符串的字面量会解析成错误的，当然了，如果你的字符串字面全都是ASCII范围内的，因为互相兼容也不会出错，但是如果有中文的话就会导致乱码。



# 处理包含辅助字符的字符串的常用方法

从概念上讲，Java字符串就是Unicode字符串序列。从本质上讲我感觉是一个代码单元串（因此也相当于char串）。我们说的字符串的length也是指的有几个代码单元。

Java字符串中的代码单元和码点是从0开始计数的。

如果理解上面所说的内容，你就会明白代码单元和码点的区别和联系了。

Java虚拟机不一定把字符串实现为代码单元（2字节的）序列。在Java9中是用来一个更紧凑的表示。只包含单字节代码单元的字符串使用byte数组实现，所有其他字符串使用char数组。

Java String API + Character API

| Method                                                       | 说明                                                         | 注释                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| int length()                                                 | 返回字符串<u>***代码单元***</u>的个数                        | 所以说用该方法来获取包含辅助字符的字符串的字符个数会让你很迷惑 |
| String substring(int beginIndex)<br/>String substring(int beginIndex,int endIndex) | 返回从beginInt开始到末尾或者endIndex-1的所有代码单元。       | 左闭右开，index是代码单元的index                             |
| String.join("separator",String...)<br />String.join(CharSequence delimiter,CharSequence... elements) | 返回一个新的字符串，用给定的定界符连接所有元素。             |                                                              |
| String repeat(int count)                                     | 返回一个字符串，将当前字符串重复count次                      |                                                              |
| boolean equals(Object other)                                 | 如果字符串与other相等，返回true                              |                                                              |
| boolean equalsIgnoreCase(String other)                       | 如果字符串与other相等（忽略大小写），返回true                |                                                              |
| int compareTo(String other)                                  | 按照字典顺序，如果字符串位于other之前，返回一个负数；反之返回正数，完全相等返回0； | 其实就是按照代码单元”\Uxxxx“来比较的。<br />因此辅助字符\uD835\uDD46的码点是 U+1D546<br />也比不过\uFFFF 码点是 U+FFFF |
| int codePointCount(int startIndex,int endIndex)              | 返回startIndex到endIndex-1之间的码点个数                     | 这里填写的index是码元的indedx                                |
| char charAt(int index)                                       | 返回给定位置的代码单元                                       | 代码单元的index                                              |
| int codePointAt(int index)                                   | 返回给定位置的码点值（高码位会给出unicode码，低码位没法给，就返回原码元值） | 代码单元的index                                              |
| int offsetByCodePoints(int startIndex,int cpCount)           | 返回从startIndex开始，cpCount个码点后的码点索引              | 代码单元的index                                              |
| int Character.charCount(int codepoint)                       | 返回给定码点值需要几个码元来表示                             | 用来遍历Unicode字符串时对index累加                           |
| boolean Character.isSurrogate(char c)                        | 返回给的char是否是一个替代区域                               |                                                              |
| InputStream codePoints()                                     | 将这个字符串的码点作为一个流返回。代用 toArray 将它们放在一个int数组中。 |                                                              |
| new String(int[] codePoints,int offset,int count)            | 用数组中从offset开始的count个码点构造一个字符串。            |                                                              |
| String Character.toString(int codepoint)                     | 将单个码点转化为一个字符串。                                 |                                                              |
| boolean isEmpty() / isBlank()                                | 如果字符串为空或者空白符(`\u3000,\u0020,\u00A0`)组成，返回true. |                                                              |
| boolean startWith(String prefix)<br />boolean endWith(String suffix) | 如果字符串以prefix开头或者以suffix结尾则返回true             |                                                              |
| String toLowerCase() toUpperCase()                           | 返回一个新字符串，这个字符串中包含原始字符串中的所有字符，不过将原始字符串中的大写字母改为小写，或者相反。 |                                                              |
| String replace(CharSequence oldString,CharSequence newString) | 返回一个新的字符串，这是因newString代替原始字符串中与oldString匹配的所有的子串得到的。 | 可以用String和StringBuilder对象作为CharSequence参数。        |
| int indexOf(String str)<br />int indexOf(String str,int fromIndex)<br />int indexOf(int cp)<br />int indexOf(int cp,int frmIndex) | 返回与字符串str或者码点cp相等的第一个子串的开始位置。从索引0或者fromIndex开始匹配。 | 如果没有返回-1                                               |
| int lastIndexOf(String str)<br />int lastIndexOf(String str,int fromIndex)<br />int lastIndexOf(int cp)<br />int lastIndexOf(int cp,int frmIndex) | 返回与字符串str或者码点cp相等的最后一个子串的开始位置。从字符串末尾或者fromIndex开始匹配。 | 如果没有返回-1                                               |
| String strip()<br />String stripLeading()<br />String stripTrailing | 返回一个新的字符串，这个字符串要删除原始字符串头部和尾部或者只是头部或者尾部的空白符。 | 要使用这些方法，而不要用远古的trim方法删除小于等于U+0020的字符 |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |
|                                                              |                                                              |                                                              |

# Java核心技术卷12ed P48 中遍历字符串码点的方法



如果想要遍历一个字符串，并且一次查看每一个码点，可以使用以下语句：

```java
int cp = sentence.codePointAt(i);
i += Character.charCount(cp);
```

也可以使用一些语句进行反向遍历：

```java
i--;
if(Character.isSurrogate(sentence.charAt(i)))
  i--;
int cp = sentence.codePointAt(i);
```

显然，这很麻烦。更容易的方法是使用codePoints方法。他会生成int值的一个“流”，每个int值对应一个码点。可以将流转换为一个数组，再完成遍历。

# StringBUffer & StringBuilder

StringBuffer类的效率不如StringBuilder，不过StringBuffer允许采用多线程的方式去添加或删除字符串。

单线程时应该用StringBuilder。这两个类的API是一样的。

| Method                                            | 说明                                                   | 注释 |
| ------------------------------------------------- | ------------------------------------------------------ | ---- |
| StringBuilder()                                   | 构造一个空的字符串构建器                               |      |
| int length()                                      | 返回构建器或者缓冲区中的代码单元的个数                 |      |
| StringBuilder append(String str)                  | 追加一个字符串并返回this                               |      |
| StringBuilder append(char c)                      | 追加一个代码单元并返回this                             |      |
| StringBuilder append(int codePoint)               | 追加一个码点，将它转换为一个或两个代码单元并返回this   |      |
| void setCharAt(int i , char c)                    | 将第i个代码单元设置为c                                 |      |
| StringBuilder insert(int offset,String str)       | 在offset位置插入一个字符串并返回this                   |      |
| StringBuilder insert(int offset,char str)         | 在offset位置插入一个代码单元并返回this                 |      |
| StringBuilder delete(int startIndex,int endIndex) | 删除从startIndex开始，到endIndex-1的代码单元并返回this |      |
| String toString()                                 | 返回一个字符串，其数据与构建器或缓冲器内容相同         |      |

