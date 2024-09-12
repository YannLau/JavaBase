[toc]

# Advanced Enum

这个是上一个笔记中展示的用法。本笔记在此基础上扩展，而且展示高级技巧。

```java
package org.yannlau;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public enum Season {
  SPRING,
  SUMMER,
  AUTUMN,
  WINTER(4, "寒冷");

  private int value;
  private String description;
  private static final Season[] seasons = values();

  Season() {
  }

  Season(int value, String description) {
    this.value = value;
    this.description = description;
  }

  public int getValue() {
    return value;
  }

  public String getDescription() {
    return description;
  }

  public static void printAll() {
    //测试values()的返回值
    System.out.println("=========测试values()的返回值===========");
    Arrays.stream(seasons).forEach(System.out::println);
    System.out.println("&&&&&&&&&&&&&&");
    Stream<Season> stream = Arrays.stream(seasons);
    List<Season> collect = stream.collect(Collectors.toList());
    System.out.println(collect);
    System.out.println("=========测试name()的返回值========");
    String name = SPRING.name();
    System.out.println(name);
    System.out.println("=========测试valueOf(String)的返回值========");
    Season awd = Season.valueOf("AUTUMN");
    System.out.println(awd);
    System.out.println("========展示valueOf(Class<T> enumType,String name)的用法========");
    Season summer = Enum.valueOf(Season.class, "SUMMER");
    System.out.println(summer);
    System.out.println("========展示compareTo(E o)的用法========");
    int i = Season.SPRING.compareTo(Season.AUTUMN);
    System.out.println("compareTo()的结果为 -> " + i);
    System.out.println("========展示ordinal()的用法========");
    System.out.println(awd.ordinal());
  }
}

```



# 抽象方法测试

```java
package org.yannlau.aa;

import java.lang.reflect.Modifier;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public enum Season {
  SPRING {
    @Override
    public int hi() {
      return 0;
    }
    @Override
    protected int ih() {
      return 3;
    }
  },
  SUMMER {
    @Override
    public int hi() {
      return 1;
    }

    @Override
    protected int ih() {
      return 2;
    }
  },
  AUTUMN {
    @Override
    public int hi() {
      return 2;
    }

    @Override
    protected int ih() {
      return 1;
    }
  },
  WINTER(4, "寒冷") {
    @Override
    public int hi() {
      return 3;
    }

    @Override
    protected int ih() {
      return 0;
    }
  };

  private int value;
  private String description;
  private static final Season[] seasons = values();

  Season() {
  }

  Season(int value, String description) {
    this.value = value;
    this.description = description;
  }

  public int getValue() {
    return value;
  }

  public String getDescription() {
    return description;
  }


  public static void printAll() {
    //测试values()的返回值
    System.out.println("=========测试values()的返回值===========");
    Arrays.stream(seasons).forEach(System.out::println);
    System.out.println("&&&&&&&&&&&&&&");
    Stream<Season> stream = Arrays.stream(seasons);
    List<Season> collect = stream.collect(Collectors.toList());
    System.out.println(collect);
    System.out.println("=========测试name()的返回值========");
    String name = SPRING.name();
    System.out.println(name);
    System.out.println("=========测试valueOf(String)的返回值========");
    Season autumn = Season.valueOf("AUTUMN");
    System.out.println(autumn);
    System.out.println("========展示valueOf(Class<T> enumType,String name)的用法========");
    Season summer = Enum.valueOf(Season.class, "SUMMER");
    System.out.println(summer);
    System.out.println("========展示compareTo(E o)的用法========");
    int i = Season.SPRING.compareTo(Season.AUTUMN);
    System.out.println("compareTo()的结果为 -> " + i);
    System.out.println("========展示ordinal()的用法========");
    System.out.println(autumn.ordinal());
    System.out.println("========展示有了抽象方法后的枚举类是否是抽象类的用法========");
    int modifiers = Season.class.getModifiers();
    if (Modifier.isAbstract(modifiers)) {
      System.out.println("It is a 抽象类");
    } else {
      System.out.println("ta不是抽象类");
    }
    System.out.println("========展示枚举类实现抽象方法的并使用========");
    System.out.println("public方法autumn.hi()的结果:"+autumn.hi());
    System.out.println("protected方法autumn.ih()的结果:"+autumn.ih());
  }

  public abstract int hi();
  protected abstract int ih();
}
```

上面必要说明的两点：

1. 没有抽象方法的时候，测试表明不是抽象类。有了抽象方法，枚举常量实现后仍然是抽象类。
2. 有了抽象方法每个枚举常量都要实现。
3. 受保护方法仍然是不同包且非子类无法直接访问。
4. 枚举类仍然可以有自己的实例方法、静态方法。而且根据org.springframework.http.HttpStatus可以看出，枚举类内部还可以在创建内部枚举类。
5. 枚举类可以实现接口，和实现抽象方法一样，都是每个枚举常量分别实现的，而且都要实现。

# 枚举getClass、getDeclaringClass区别

```java
package org.yannlau;

import org.yannlau.enums.Season;

import java.io.PrintStream;

public class Main {
  public static void main(String[] args) {
    Season spring = Season.SPRING;
    Season autumn = Season.AUTUMN;
    System.out.println(spring.getDeclaringClass()==autumn.getDeclaringClass());
    System.out.println(spring.getClass()==autumn.getClass());
  }
}
输出：
  true
  false
```

## 原因

在使用枚举类的时候，建议用`getDeclaringClass`返回枚举类。但是为什么不用`getClass`呢？

```kotlin
public enum FruitEnum{
  BANANA,APPLE;

  public static void main(String[] args) {
    System.out.println(BANANA.getDeclaringClass());
    System.out.println(BANANA.getClass());
  }
}

# 运行结果
class FruitEnum
class FruitEnum
```

有人说结果不是一样吗？不急，看下面这种情况。

```kotlin
public enum FruitEnum{
  BANANA{
    String getName() {
      return "香蕉";
    }
  },APPLE{
    String getName() {
      return "苹果";
    }
  };

  abstract String getName();

  public static void main(String[] args) {
    System.out.println(BANANA.getDeclaringClass());
    System.out.println(BANANA.getClass());
  }
}

# 运行结果
class FruitEnum
class FruitEnum$1
```

这种情况下就不同了。因为此时`BANANA`和`APPLE`相当于`FruitEnum`的内部类。下面来看看`Enum`的源码：

```kotlin
public final Class<E> getDeclaringClass() {
  Class var1 = this.getClass();
  Class var2 = var1.getSuperclass(); // 获取上一级的父类
  return var2 == Enum.class ? var1 : var2;
}
```

当上一级的父类不是`Enum`，则返回上一级的`class`。因此对枚举类进行比较的时候，使用`getDeclaringClass`是万无一失的。

# [枚举getClass、getDeclaringClass区别](https://www.cnblogs.com/thaipine/p/9390210.html)

枚举getClass、getDeclaringClass区别

1）：“不含抽象方法”，所有枚举常量未重写方法 的 class
getClass与getDeclaringClass方法输出结果相同
class反编译文件：public final class NoteBook extends Enum

2）：“不含抽象方法”，`部分枚举常量重写了方法`，的class
枚举常量重写了方法：getClass与getDeclaringClass方法输出结果不同
枚举常量未重写方法：getClass与getDeclaringClass方法输出结果相同

```java
//枚举类实例可以随意重写方法。和匿名内部类好像。
SPRING {
      @Override
      public int hi() {
          return 0;
      }
      @Override
      protected int ih() {
          return 3;
      }
      @Override
      public String toString(){
          return "wan";
      }
  },
```

class反编译文件：public class Cellphone extends Enum

3 ）：“不含抽象方法”，所有枚举常量重写了方法，的class
getClass与getDeclaringClass方法输出结果不同
class反编译文件：public class Week extends Enum

4）：“含有抽象方法”，枚举常量实现了方法，的class
getClass与getDeclaringClass方法输出结果不同
class反编译文件：public abstract class Weather extends Enum