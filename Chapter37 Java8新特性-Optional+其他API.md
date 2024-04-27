# Optional类

- JDK8 新特性

- 到目前为止，臭名昭著的空指针异常是导致Java应用程序失败的最常见原因。
  以前，为了解决空指针异常，Google公司著名的Guava项目引入了Optional类，
  Guava通过使用检查空值的方式来防止代码污染，它鼓励程序员写更干净的代
  码。受到Google Guava的启发，Optional类已经成为Java 8类库的一部分。

- Optional<T>类（java.util.Optional）是一个容器类，它可以保存类型T的值，仅代表这个值存在。或者仅仅保存null，表示这个值不存在。如果值存在，则isPresent()方法会返回true，调用get()方法会返回该对象。
- 原来用 null 表示一个值不存在，现在 Optional 可以更好的表达这个概念。并且可以避免空指针异常。
- Optional类的Javadoc描述如下：这是一个可以为null的容器对象。如果值存在
  则isPresent()方法会返回true，调用get()方法会返回该对象。

> Optional提供很多有用的方法，这样我们就不用显式进行空值检测。
>
> > - 创建Optional类对象的方法：
> >
> >   `Optional.of(T t)` : 创建一个 Optional 实例，t 必须非空；
> >   `Optional.empty()`：创建一个空的 Optional 实例
> >   `static <T> Optional.ofNullable(T t)`：t 用来创建一个optional实例，==value可能是空，也可能非空==。
> >
> > - 判断Optional容器中是否包含对象：
> >   boolean isPresent()：判断是否包含对象
> >   void ifPresent(Consumer<？ super T> consumer)：如果有值，就执行Consumer接口的实现代码，并且该值会作为参数传给它。
> >
> > - 获取Optional容器的对象：
> >   T get()：如果调用对象包含值，返回该值，否则抛异常
> >   `T orElse(T other)`：如果有值则将其返回，否则返回指定的other对象。
> >   T orElseGet(Supplier<？ extends T> other)：如果有值则将其返回，否则返回由Supplier接口实现提供的对象。
> >   T orElseThrow(Supplier<？ extends X> exceptionSupplier)：
> >   如果有值则将其返回，否则抛出由Supplier接口实现提供的异常。



```java
import org.junit.jupiter.api.Test;

import java.util.Optional;

/**
 * @author YannLau
 * @version 1.0
 * @program java8_NewProperties
 * @ClassName OptionalTest
 * @ClassPath com.lyp.java8.optional_class.OptionalTest
 * @create 2024-03-10 17:01
 * @description Optional 的使用学习
 * Optional类的作用:
 * 为了避免在程序中出现空指针异常而创建的
 */
public class OptionalTest {
    /*
        Optional.of(T t)  创建一个Optional实例  t必须为非空
        Optional.empty()  创建一个空的Optional实例
        Optional.ofNullable(T t) t可以为null
     */
    @Test
    public void test1() {
        Girl girl = new Girl();
        //of(T t)方法要求非空
        Optional<Girl> girl1 = Optional.of(girl);
        System.out.println(girl1);
    }

    @Test
    public void test2() {
        Girl girl = new Girl();
        girl = null;
        //ofNullable(T t) t可以为null
        Optional<Girl> girl1 = Optional.ofNullable(girl);
        System.out.println(girl1);
    }

    public String getGirlName(Boy boy) {
        //传统的方法容易导致空指针异常
        return boy.getGirl().getName();
    }

    @Test
    public void test3() {
        Boy boy = new Boy();
        //传统的方法容易导致空指针异常
        String girlName = getGirlName(boy);
        System.out.println(girlName);
    }

    //优化以后的getGirlName方法
    public String getGirlNamePlus(Boy boy) {
        if (boy != null) {
            Girl girl = boy.getGirl();
            if (girl != null) {
                return girl.getName();
            }
        }
        return null;
    }

    @Test
    public void test4() {
        Boy boy = new Boy();
        //传统的方法优化方法比较繁琐
        String girlName = getGirlNamePlus(boy);
        System.out.println(girlName);
    }

    //使用Optional类以后的getGirlName方法
    public String getGirlNamePro(Boy boy) {
        Optional<Boy> boyOptional = Optional.ofNullable(boy);
        Boy boy1 = boyOptional.orElse(new Boy(new Girl("迪丽热巴")));
        //此时boy1一定是非空的,但是boy1的girl不一定是不是null
        Girl girl = boy1.getGirl();
        Optional<Girl> girl1 = Optional.ofNullable(girl);
        //此时girl2一定非空
        Girl girl2 = girl1.orElse(new Girl("古力娜扎"));
        return girl2.getName();
    }

    @Test
    public void test5() {
        Boy boy = new Boy();
        System.out.println(getGirlNamePro(boy));
    }
}
```

# 其它结构变化
- 8.1 JDK9:UnderScore（下划线）使用的限制

在java8中，标识符可以独立使用“\_”来命名：

但是，在java 9中规定“_”不再可以单独命名标识符了，如果使用，会报错。

- 8.2 JDK11：更简化的编译运行程序

我们的认知里，要运行一个 Java 源代码必须先编译，再运行。而在 Java 11版本中，通过一个java 命令就直接搞定了，如下所示：
`java JavaStack. java`

注意点：执行源文件中的第一个类，第一个类必须包含主方法。
