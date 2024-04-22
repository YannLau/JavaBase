P104——P154

[toc]

# 顺序控制

程序从上到下逐行地执行，中间没有任何判断和跳转。

Java中定义变量时采用合法的*<u>**前向引用**</u>*。

# 分支控制

## if语句分支

`if (){`

`}else if(){`

`}else if(){`

`}else{`

`}`

1. 当条件表达式1成立时，即执行代码块1，
2. 如果表达式1不成立，才去判断表达式2是否成立，
3. 如果表达式2成立，就执行代码块2
4. 以此类推，如果所有的表达式都不成立
5. 则执行 else 的代码块，注意，只能有一个执行入口。
6. 可以不含最后的 else{ } 

注意条件判断语句的 == 不要写成 =     ！ 

在一个分支结构中又完整的嵌套了另一个完整的分支结构，里面的分支的结构称为内层分支外面的分支结构称为外层分支。规范：不要超过3层（可读性不好）。

## switch分支 ：switch语句 和 switch表达式

注意事项：

1. switch()内表达式数据类型，应和case后的常量类型一致，或者是可以自动转成可以相互比较的类型。
2. <u>***case子句中的值必须是常量，而不能是变量***</u>
3. default子句是可选的，当没有匹配的case时执行default
4. 一旦匹配上就会一直执行，直到遇到 break;



- switch 语句 1 （**标准的 `switch`**，有直通行为）

  ```java
  int dayOfWeek = 3;
  switch (dayOfWeek) {
      case 1:
          System.out.println("Monday");
          break;
      case 2:
          System.out.println("Tuesday");
          break;
      // ... other cases ...
      default:
          System.out.println("Invalid day");
  }
  ```

- switch 语句 2 （无直通行为）

  ```java
  switch (seasonName)
  {
      case "Spring" -> 
      {
        System.out.println("spring time!");
        numLetters = 6;
      }
      case "Summer","Winner" ->
        numLetters = 6;
      case "Fall" -> 
        numLetters = 4;
      default ->
        numLetters = -1;
  }
  ```

  `switch 的 case标签`可以是：

  - 类型为 char、byte、short 或 int 的常量表达式（不含变量），可以多个用逗号隔开。
  - 枚举常量
  - 字符串字面量
  - 多个字符串，用逗号分隔

- switch 表达式 1 （`->`无直通行为）

  ```java
  int numLetters = switch(seasonName){
      case "Spring" ->
      {
        System.out.println("Spring time!");//由于要打印，故使用 {}
        yield 6; //由于 switch 表达式必须生成值，故 yield。
      }
      case "Sunmmer","Winter" -> 6;
      case "Fall" -> 4;
      default -> -1;
  };    // ; 说明构成一个句子
  ```

  > switch 表达式的每个分支必须生成一个值。且 { } 后面要加上`;` 构成一个句子。最常见的做法是，各个值跟在一个箭头 -> 后面：
  >
  > `case "Summer","Winter" -> 6;`
  >
  >  如果无法做到，比如还要打印其他内容，则使用 `yield` 语句返回值。因此 yield 是在 多条语句的情况下使用的，单语句不应该出现 `case "Fall" -> yield 3;`这种情况！而是 `case "Fall" -> 3;`

- switch 表达式 2 （Java14 有直通行为）

  ```java
  int numLetters = switch (seasonName){
  	case "Spring":
  		System.out.println("spring time!");
    case "Summer","Winter":
  		yield 6;
  	case "Fall':
  		yield 4;
  	default:
  		yield -1;
  };
  ```

  > 遇到第一个 yield 语句才会停止。

<u>***在有直通行为的形式中，每个 case 以一个冒号结束。***</u>

<u>***如果 case 以箭头 -> 结束，则没有直通行为。***</u>

<u>***不能在一个 switch 语句中混合使用冒号和箭头。***</u>

注意 `switch 表达式`中的 yield 关键字。 与 break 类似，yield 会终止执行。但与 break 不同的是，yield 还会生成一个值，这就是表达式的值。

要在 `switch 表达式`的一个分支中使用其他语句而不想有直通行为，就必须使用大括号和 yield。

switch 表达式的关键是生成一个值（或者产生一个异常而失败）。

不允许“跳出" switch 表达式： 

`default -> { return -1; } // ERROR`

具体来讲，不能在 switch 表达式中使用 return、break 或 continue 语句。

## for 循环

细节：

1） 循环条件是返回一个布尔值的表达式

2） for（循环判断条件 ；；中的初始化和变量迭代可以写到其它地方，但是两边的分号不能省略。

3） 循环初始值可以有多条初始化语句，但要求类型一样，并且中间用逗号隔开，循环变量迭代也可以有多条变量迭代语句，中间用逗号隔开。

4） `for( ; ; ){}`表示一个无限循环/死循环。

## while/do while 循环

循环变量初始化：
`do{`
	`循环体（语句）：`
	`循环变量迭代：`
`｝while(循环条件);`

1. do while是关键字
2. 也有循环四要素，只是位置不一样
3. 先执行，再判断，也就是说，一定会执行一次
4. *<u>**最后 有一个分号；**</u>*

5. while 和 do..while 区别举例：要账  do while = 先打一顿再要账

# Math.random()

`static double random();`

`返回一个带正号的 double 值，该值大于等于 0.0 且小于 1.0`

# break

break语句用于终止某个语句块的执行，一般使用
switch 或者 [for, while, do-while]中。

> Break 使用细节

1. break语句出现在多层嵌套的语句块中时，可以通过标签指明要终止的是哪一层语句块

2. 标签的基本使用

   ```java
   label1:{ ...
     label2:{ ...
       label3:{ ...
       					{  ...
                    break label2;
                    ...
       					}
       				}
     				}
   				}
   ```

   （1）break 语句可以指定退出哪层
   （2）label1 是标签，由程序员指定。
   （3） break 后指定到哪个label 就退出到哪里
   （4）在实际的开发中，尽量不要使用标签.
   （5）如果没有指定 break，默认退出最近的循环体

# continue

1） continue语句用于结束本次循环，继续执行下一次循环。
2） continue语句出现在多层嵌套的循环语句体中时，可以
通过标签指明要跳过的是哪一层循环，这个和前面的标
签的使用的规则一样。

# return

