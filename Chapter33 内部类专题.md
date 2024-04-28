[toc]

如果定义在局部位置（方法中、代码块）：1.局部内部类 2.匿名内部类

如果定义在成员位置 1.成员内部类 2.静态内部类

# 内部类

Java 程序员水平的分水岭！

一个类的内部又完整的嵌套了另一个类结构。被嵌套的类称为内部类（inner class），嵌套其他类的类称为外部类（outer class）。是我们类的第五大成员。

五大成员：属性、方法、构造器、代码块、内部类。

内部类最大的特点就是可以==直接访问私有属性==，并且可以体现类与类之间的包含关系。

内部类是学习的难点，同时也是重点，后面看底层源码时会遇到大量的内部类！

> 基本语法
>
> ```java
> class Outer{ //外部类
>   class Inner{  //内部类
> }
>   }
> class Other{  //外部其他类
> }
> ```

快速入门案例

```java
package com.hspedu.innerclass;
public class InnerClass01 { //外部其他类
  public static void main(String[] args) {
  }
}
class Outer { //外部类
  private int n1 = 100;//属性
  public Outer(int n1) {//构造器
    this.n1 = n1;
  }
  public void m1() {//方法
    System.out.println("m1()");
  }
  {//代码块
    System.out.println("代码块...");
  }
  class Inner { //内部类, 在 Outer 类的内部
  }
}
```

> 分类
>
> 1. 定义在外部类局部位置上 （比如方法内）：
>    1. 局部内部类（有类名）
>    2. 匿名内部类（没有类名，重点!!!!!!!!!!!!!!!）
> 2. 定义在外部类的成员位置上：
>    1. 成员内部类（没有static修饰）
>    2. 静态内部类（使用static修饰）

# 内部类——局部内部类

> 说明：局部内部类是定义在外部类的局部位置，比如通常在方法中，并且有类名。
>
> 1. 可以直接访问外部类的所有成员，包含==私有==的
> 2. 不能添加访问修饰符，因为它的==地位就是一个局部变量==。局部变量是不能使用修饰符的。但是可以使用final修饰，因为局部变量也可以使用 final
> 3. 作用域：仅仅在定义它的方法或代码块中。
> 4. 局部内部类——访问——>外部类的成员【访问方式：直接访问】
> 5. 外部类——访问——>局部内部类的成员。访问方式：创建对象，再访问（<u>***注意：必须在作用域内。***</u>）
> 6. 外部其他类—--不能访问—->局部内部类（因为局部内部类地位是一个局部变量）
> 7. 如果外部类和局部内部类的成员重名时，默认遵循就近原则，内部类的实例对象如果想访问外部类的成员，则可以使用（==外部类名.this.成员==）去访问。
> 8. （`外部类名.this`）**<u>*本质就是指代外部类的调用本方法以至于创建了内部类实例对象的外部对象实例.*</u>**
> 9. 内部类中直接使用 this 代表的内部类自己的对象本身。

```java
package com.hspedu.innerclass;
/**
* 演示局部内部类的使用
*/
public class LocalInnerClass {//
  public static void main(String[] args) {
    //演示一遍
    Outer02 outer02 = new Outer02();
    outer02.m1();
    System.out.println("outer02 的 hashcode=" + outer02);
  }
}
class Outer02 {//外部类
  private int n1 = 100;
  private void m2() {
    System.out.println("Outer02 m2()");
  }//私有方法
  public void m1() {//方法
    //1.局部内部类是定义在外部类的局部位置,通常在方法
    //3.不能添加访问修饰符,但是可以使用 final 修饰
    //4.作用域 : 仅仅在定义它的方法或代码块中
    final class Inner02 {//局部内部类(本质仍然是一个类)
      //2.可以直接访问外部类的所有成员，包含私有的
      private int n1 = 800;
      public void f1() {
        //5. 局部内部类可以直接访问外部类的成员，比如下面 外部类 n1 和 m2()
        //7. 如果外部类和局部内部类的成员重名时，默认遵循就近原则，如果想访问外部类的成员，
        // 使用 外部类名.this.成员）去访问
        // 老韩解读 Outer02.this 本质就是外部类的对象, 即哪个对象调用了 m1, Outer02.this 就是哪个对象
        System.out.println("n1=" + n1 + " 外部类的 n1=" + Outer02.this.n1);
        System.out.println("Outer02.this hashcode=" + Outer02.this);
        m2();
      }
    }
    //6. 外部类在方法中，可以创建 Inner02 对象，然后调用方法即可
    Inner02 inner02 = new Inner02();
    inner02.f1();
  }
}
```

> 记住：
>
> （1）局部内部类定义在方法中/代码块
> （2） 作用域在方法体或者代码块中
> （3） 本质仍然是一个类

## BY YannLau: 一段代码搞透局部内部类的访问问题：

```java
import org.junit.jupiter.api.Test;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName FatherClass
 * @ClassPath PACKAGE_NAME.FatherClass
 * @create 2024-04-23 00:53
 * @description
 */
public class FatherClass {

    private final String strFa = "我是FatherClass的私有String字段";

    private final int a = 999;
    private final int b = 999;
    private final int c = 999;
    private final int d = 999;

    private void f1() {
        System.out.println("我是FatherClass的私有方法!");
    }

    @Test
    public void test() {

        final int a = 6;
        final int b = 6;
        final int c = 6;

        // new InnerClass() == error 此处InnerClass尚未定义,不能使用

        class InnerClass {

            private static final String strInner = "我是局部内部类的静态私有字符串";

            private final int a = 2;
            private final int b = 2;

            private void p1() {

                System.out.println("InnerClass实例的p1方法>>>");

                final int a = 1;

                System.out.println("就近原则: " + (a + b + c + d)); //(1 + 2 + 6 + 999) ==1008

                f1(); //同样就近原则,使用的是内部类的f1方法
                this.f1(); //同上
                FatherClass.this.f1(); //使用的外部类实例对象的f1()方法

                System.out.println("我能访问到父类实例字段的a: " + FatherClass.this.a);
                System.out.println("我能访问到父类实例字段的b: " + FatherClass.this.b);
                System.out.println("我能访问到父类实例字段的c: " + FatherClass.this.c);
                System.out.println("我能访问到父类实例字段的d: " + FatherClass.this.d);

                System.out.println("我能访问到自己内部类的实例字段a: " + this.a);
                System.out.println("我能访问到自己内部类的实例字段b: " + this.b);
                //System.out.println("我不能访问到自己内部类的实例字段c: " + this.c);  因为不存在

                //我该如何访问到和内部类同级别的外部类实例的test方法中定义的局部变量呢
                System.out.println("我能访问到外部局部变量c: " + c); //因为没有就近原则的阻拦,我可以直接访问 c = 6
                //但是外部局部变量a,b因为就近原则被拦截了,有没有this等操作可以使用,因此无解,只有传递参数或者改名了.
                /*
                    在命名重复的情况下，确实无法直接访问外部方法中的同名局部变量，因为局部变量的就近原则将会覆盖同名的外部变量
                    ，从而隐藏外部变量。如果不通过传递参数，在内部类的方法中无法直接访问外部方法中同名的局部变量。
                    解决这个问题的常见方法是重命名局部变量，以避免命名冲突。如果你希望在内部类中访问外部方法中的局部变量，
                    但又不想通过传递参数的方式，你可以修改内部类的方法，使其访问外部方法中的成员变量而不是局部变量。
                    例如，你可以将外部方法中的局部变量提升为外部类的成员变量，然后在内部类的方法中直接访问这些成员变量。
                    这样做的话，内部类的方法就可以访问到外部方法中定义的成员变量了，而不会受到局部变量命名冲突的影响。
                 */

                System.out.println("我能访问到内部类本方法的局部变量a: " + a);


                System.out.println("InnerClass实例的p1方法<<<\n");
            }

            private void f1() {
                System.out.println("我是InnerClass实例的私有方法!");
            }
        }

        InnerClass innerClass = new InnerClass();

        innerClass.p1();//可以调用私有方法
        int num = innerClass.b;//correct! 在局部内部类的作用域内可以直接访问内部类实例的私有变量
        String str = InnerClass.strInner; //同理
        /*
         这里 InnerClass 是局部内部类，但在其作用域内确实可以直接访问 InnerClass 实例的私有变量 a 和 b，
         以及静态私有变量 strInner。这是因为在Java中，内部类实际上是外部类的成员，它们与外部类的实例或静态成员
         具有相同的访问权限。因此，内部类的实例可以直接访问其自身的私有成员变量和静态私有成员变量。
         所以，innerClass.p1() 可以调用内部类的私有方法，innerClass.b 可以访问内部类的私有变量 b，
         InnerClass.strInner 可以访问内部类的静态私有变量 strInner。
         */
    }
}
```

# 内部类——匿名内部类*

> 本质是一个类! 是内部类! 该类没有名字! 同时还是一个对象!
>
> 说明：
>
> 匿名内部类是定义在外部类的局部位置，比如方法中，并且没有类名。
>
> 也可以定义在类的实例字段中。

> 1. 匿名内部类的基本语法
>
>    ```java
>    类/接口名 变量名 = new 类或接口(参数列表){
>      类体
>    };
>    //会生成一个匿名内部类并且继承了等号左边类或者实现了等号左边的接口.
>    //当是继承一个类的匿名内部类时会使用到参数列表,直接传递给被继承的类的对象构造器.
>    
>    //案例演示
>    package com.hspedu.innerclass;
>    /**
>    * 演示匿名内部类的使用
>    */
>    public class AnonymousInnerClass {
>      public static void main(String[] args) {
>        Outer04 outer04 = new Outer04();
>        outer04.method();
>      }
>    }
>    class Outer04 { //外部类
>      private int n1 = 10;//属性
>      public void method() {//方法
>        //基于接口的匿名内部类
>        //老韩解读
>        //1.需求： 想使用 IA 接口,并创建对象
>        //2.传统方式，是写一个类，实现该接口，并创建对象
>        //3.老韩需求是 Tiger/Dog 类只是使用一次，后面再不使用
>        //4. 可以使用匿名内部类来简化开发
>        //5. tiger 的编译类型 ? IA
>        //6. tiger 的运行类型 ? 就是匿名内部类 Outer04$1
>        /*
>    我们看底层 会分配 类名 Outer04$1
>    class Outer04$1 implements IA {
>    @Override
>    public void cry() {
>    System.out.println("老虎叫唤...");
>    }
>    }
>    */
>        //7. jdk 底层在创建匿名内部类 Outer04$1,立即马上就创建了 Outer04$1 实例，并且把地址
>        // 返回给 tiger
>        //8. 匿名内部类使用一次，就不能再使用
>        IA tiger = new IA() {
>          @Override
>          public void cry() {
>            System.out.println("老虎叫唤...");
>          }
>        };
>        System.out.println("tiger 的运行类型=" + tiger.getClass());
>        tiger.cry();
>        tiger.cry();
>        tiger.cry();
>        // IA tiger = new Tiger();
>        // tiger.cry();
>        //演示基于类的匿名内部类
>        //分析
>        //1. father 编译类型 Father
>        //2. father 运行类型 Outer04$2
>        //3. 底层会创建匿名内部类
>        /*
>    class Outer04$2 extends Father{
>    @Override
>    public void test() {
>    System.out.println("匿名内部类重写了 test 方法");
>    }
>    }
>    */
>        //4. 同时也直接返回了 匿名内部类 Outer04$2 的对象
>        //5. 注意("jack") 参数列表会传递给 构造器
>        Father father = new Father("jack"){
>          @Override
>          public void test() {
>            System.out.println("匿名内部类重写了 test 方法");
>          }
>        };
>        System.out.println("father 对象的运行类型=" + father.getClass());//Outer04$2
>        father.test();
>        //基于抽象类的匿名内部类
>        Animal animal = new Animal(){
>          @Override
>          void eat() {
>            System.out.println("小狗吃骨头...");
>          }
>        };
>        animal.eat();
>      }
>    }
>    interface IA {//接口
>      public void cry();
>    }
>    //class Tiger implements IA {
>    //
>    // @Override
>    // public void cry() {
>    // System.out.println("老虎叫唤...");
>    // }
>    //}
>    //class Dog implements IA{
>    // @Override
>    // public void cry() {
>    // System.out.println("小狗汪汪...");
>    // }
>    //}
>    class Father {//类
>      public Father(String name) {//构造器
>        System.out.println("接收到 name=" + name);
>      }
>      public void test() {//方法
>      }
>    }
>    abstract class Animal { //抽象类
>      abstract void eat();
>    }
>    ```
>
> 2. 底层其实发生了创建类的过程
>
> 3. 匿名内部类会被系统分配一个名字  `外部类名 $ 1/2/3`
>
> 4. JDK底层在创建了匿名内部类后,立即马上就创建了其对应的实例对象,并且把地址返回给  `对象引用 或者 作为实参传入调用函数.`
>
> 5. 匿名内部类使用一次就不能再使用了,但接收其返回对象的引用还可以一直用.

> 匿名内部类的使用
>
> 1. 语法见上
>
> 2. 匿名内部类的语法比较奇特，请大家注意，因为匿名内部类既是一个类的定义,同时它本身也是一个对象，因此从语法上看，它既有定义类的特征，也有创建对象的特证，对前面代码分析可以看出这个特点，因此可以调用匿名内部类方法.
> 3. 可以直接访问外部类的所有成员，包含私有的
> 4. 不能添加访问修饰符，因为它的地位就是一个局部变量。
> 5. 作用域：仅仅在定义它的方法或代码块中.
> 6. 匿名内部类---访问---->外部类成员［访问方式：直接访问］
> 7. 外部其他类—--不能访问----->匿名内部类（因为 匿名内部类地位是一个局部变量）
> 8. 如果外部类和内部类的成员重名时，内部类访问的话，默认遵循就近原则，如果想访问外部类的成员，则可以使用（`外部类名.this.成员`）去访问
> 9. 对匿名内部类对象的方法调用仍然有动态绑定机制的存在!

```java
package com.hspedu.innerclass;
public class AnonymousInnerClassDetail {
  public static void main(String[] args) {
    Outer05 outer05 = new Outer05();
    outer05.f1();
    //外部其他类---不能访问----->匿名内部类
    System.out.println("main outer05 hashcode=" + outer05);
  }
}
class Outer05 {
  private int n1 = 99;
  public void f1() {
    //创建一个基于类的匿名内部类
    //不能添加访问修饰符,因为它的地位就是一个局部变量
    //作用域 : 仅仅在定义它的方法或代码块中
    Person p = new Person(){
      private int n1 = 88;
      @Override
      public void hi() {
        //可以直接访问外部类的所有成员，包含私有的
        //如果外部类和匿名内部类的成员重名时，匿名内部类访问的话，
        //默认遵循就近原则，如果想访问外部类的成员，则可以使用 （外部类名.this.成员）去访问
        System.out.println("匿名内部类重写了 hi 方法 n1=" + n1 +
                           " 外部内的 n1=" + Outer05.this.n1 );
        //Outer05.this 就是调用 f1 的 对象
        System.out.println("Outer05.this hashcode=" + Outer05.this);
      }
    };
    p.hi();//动态绑定, 运行类型是 Outer05$1
    //也可以直接调用, 匿名内部类本身也是返回对象
    // class 匿名内部类 extends Person {}
    // new Person(){
    // @Override
    // public void hi() {
    // System.out.println("匿名内部类重写了 hi 方法,哈哈...");
    // }
    // @Override
    // public void ok(String str) {
    // super.ok(str);
    // }
    // }.ok("jack");
  }
}
class Person {//类
  public void hi() {
    System.out.println("Person hi()");
  }
  public void ok(String str) {
    System.out.println("Person ok() " + str);
  }
}
//抽象类/接口...
```

### 匿名内部类的最佳实践

当做实参直接传递，简洁高效。

```java
package com.hspedu.innerclass;
import com.hspedu.abstract_.AA;
public class InnerClassExercise01 {
  public static void main(String[] args) {
    //当做实参直接传递，简洁高效
    f1(new IL() {
      @Override
      public void show() {
        System.out.println("这是一副名画~~...");
      }
    });
    //传统方法
    f1(new Picture());
  }
  //静态方法,形参是接口类型
  public static void f1(IL il) {
    il.show();
  }
}
//接口
interface IL {
  void show();
}
//类->实现 IL => 编程领域 (硬编码)
class Picture implements IL {
  @Override
  public void show() {
    System.out.println("这是一副名画 XX...");
  }
}
```

### 内部类练习

 <img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240423下午15708487.png" alt="image-20240423下午15708487" style="zoom:50%;" />

```java
package com.hspedu.innerclass;
public class InnerClassExercise02 {
  public static void main(String[] args) {
    /*
1.有一个铃声接口 Bell，里面有个 ring 方法。(右图)
2.有一个手机类 Cellphone，具有闹钟功能 alarmClock，参数是 Bell 类型(右图)
3.测试手机类的闹钟功能，通过匿名内部类(对象)作为参数，打印：懒猪起床了
4.再传入另一个匿名内部类(对象)，打印：小伙伴上课了
*/
    CellPhone cellPhone = new CellPhone();
    //老韩解读
    //1. 传递的是实现了 Bell 接口的匿名内部类 InnerClassExercise02$1
    //2. 重写了 ring
    //3. Bell bell = new Bell() {
    // @Override
    // public void ring() {
    // System.out.println("懒猪起床了");
    // }
    // }
    cellPhone.alarmClock(new Bell() {
      @Override
      public void ring() {
        System.out.println("懒猪起床了");
      }
    });
    cellPhone.alarmClock(new Bell() {
      @Override
      public void ring() {
        System.out.println("小伙伴上课了");
      }
    });
  }
}
interface Bell{ //接口
  void ring();//方法
}
class CellPhone{//类
  public void alarmClock(Bell bell){//形参是 Bell 接口类型
    System.out.println(bell.getClass());
    bell.ring();//动态绑定
  }
}
```

## By YannLau 匿名内部类补充

> java 创建匿名内部类一定要去继承一个父类或者实现一个接口吗？
>
> ChatGPT
>
> 在Java中，匿名内部类通常是通过继承一个父类或者实现一个接口来创建的，但并不是一定要这样做。匿名内部类的创建方式可以根据需要选择，可以是继承一个类，也可以是实现一个接口。
>
> 如果你希望创建一个匿名内部类并实现某个接口的方法，可以这样做：
>
> ```java
>javaCopy code
> MyInterface myObject = new MyInterface() {
> @Override
> public void myMethod() {
>    // 实现接口方法的具体逻辑
>  }
>    };
>  ```
> 
> 或者，如果你希望创建一个匿名内部类并继承自某个类，可以这样做：
>
> ```java
>javaCopy code
> MyClass myObject = new MyClass() {
> @Override
> public void myMethod() {
>    // 继承类的方法并提供具体实现
>  }
>    };
>  ```
> 
> 但是，如果你只是想创建一个对象而不需要扩展某个类或实现某个接口，也是可以的。例如：
>
> ```java
>javaCopy code
> Object myObject = new Object() {
> // 匿名内部类的代码块
> };
>  ```
> 
> 在这种情况下，匿名内部类就是一个匿名的子类，它继承自`Object`类，但没有增加额外的方法或属性。
>
> 总体而言，匿名内部类的创建方式取决于你的需求，它可以继承类、实现接口，也可以仅作为一个对象使用。

### 搞懂匿名内部类访问权限

从图中可以看出，匿名内部类其实是一个final类！！！

==下面的测试我发现自己犯蠢了，因为runnable的编译类型是Runnable，编译器检查会发现根本没有这些字段和方法，所以全部红了。==

匿名内部类通常是隐式final的，这意味着你不能在其内部创建子类。当你定义一个匿名内部类时，它实际上是一个继承了某个类或实现了某个接口的子类的实例。由于匿名内部类没有名称，因此你无法在其外部创建它的子类。因此，虽然没有明确声明为final，但它们的行为与final类相似，无法被继承。

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240423下午61618997.png" alt="image-20240423下午61618997" style="zoom:50%;" />

==换一个测试方法==

对于匿名内部类内的字段具体能不能访问 其实就是看其实基层的接口和子类的访问权限。

```java
public class TestInnerClas {
    private final int i = 123;

    public void test() {
        baseClass b = new baseClass() {
            @Override
            void f() {
                System.out.println("private的方法是不能被访问到的,无法重写");
            }
        };
        b.private_f();//Error ! 对于匿名内部类具体能不能访问？其实就是看其实基层的接口和子类的访问权限。
    }
}

class baseClass {
    private int i = 456;

    void f() {
    }
    private void private_f() {
        
    }
}
```



# 内部类——成员内部类

YanLau说明：类的字段可以是匿名内部类的写法，但是其实因为返回的是一个匿名内部类的实例对象，也就不算内部类了，所以成员内部类指的就是普通类定义的方法。

说明：成员内部类是定义在外部类的成员位置，并且没有static修饰。

1. 可以直接访问外部类的所有成员，包含私有的

   ```java
   class Outer01{//外部类
     private int n1 = 10;
     public String name = "张三";
     class Inter01{
       public void say(){
         System.out.printin("Outer01 #J n1 = " + n1 + " outer01 #J name = " + name);
       }
     }
   }
   ```

   

2. 可以添加任意访问修饰符（public、protected、默认。Private），因为它的地位就是一个成员。
3. 作用域  :  和外部类的其他成员一样，为整个类体 .
4. 成员内部类—--访问—-->外部类（比如：属性）
   ［访问方式：直接访问］（说明）
5. 外部类—-访问—---->内部类（说明）
   访问方式：创建对象，再访问
6. 外部其他类—>访问-->成员内部类
7. 如果外部类和内部类的成员重名时，内部类访问的话，默认遵循就近原则，如果想访问外部类的成员，则可以使用（外部类名.this.成员）去访问

```java
package com.hspedu.innerclass;
public class MemberInnerClass01 {
  public static void main(String[] args) {
    Outer08 outer08 = new Outer08();
    outer08.t1();
    //外部其他类，使用成员内部类的三种方式
    //老韩解读
    // 第一种方式
    // outer08.new Inner08(); 相当于把 new Inner08()当做是 outer08 成员
    // 这就是一个语法，不要特别的纠结. Outer08.Inner08 inner08 = outer08.new Inner08();
    inner08.say();
    // 第二方式 在外部类中，编写一个方法，可以返回 Inner08 对象
    Outer08.Inner08 inner08Instance = outer08.getInner08Instance();
    inner08Instance.say();
  }
}
class Outer08 { //外部类
  private int n1 = 10;
  public String name = "张三";
  private void hi() {
    System.out.println("hi()方法...");
  }
  //1.注意: 成员内部类，是定义在外部内的成员位置上
  //2.可以添加任意访问修饰符(public、protected 、默认、private),因为它的地位就是一个成员
  public class Inner08 {//成员内部类
    private double sal = 99.8;
    private int n1 = 66;
    public void say() {
      //可以直接访问外部类的所有成员，包含私有的
      //如果成员内部类的成员和外部类的成员重名，会遵守就近原则. //，可以通过 外部类名.this.属性 来访问外部类的成员
      System.out.println("n1 = " + n1 + " name = " + name + " 外部类的 n1=" + Outer08.this.n1);
      hi();
    }
  }
  //方法，返回一个 Inner08 实例
  public Inner08 getInner08Instance(){
    return new Inner08();
  }
  //写方法
  public void t1() {
    //使用成员内部类
    //创建成员内部类的对象，然后使用相关的方法
    Inner08 inner08 = new Inner08();
    inner08.say();
    System.out.println(inner08.sal);
  }
}
```



# 内部类——静态内部类

说明：静态内部类是定义在外部类的成员位置，并且有static修饰

1. 可以直接访问外部类的所有静态成员，包含私有的，`但不能直接访问非静态成员`
2. 可以添加任意访问修饰符（public. protected、默认 和private），因为它的地位就是一个成员。
3. 作用域：同其他的成员，为整个类体
4. 静态内部类---访问---->外部类（比如：静态属性）［访问方式：直接访问所有静态成员]
5. 外部类---访问-—--＞静态内部类 访问方式：创建对象，再访问
6. 外部其他类—访问—----＞静态内部类
7. 如果外部类和静态内部类的成员重名时，静态内部类访问的时，默认遵循就近原则，如果想访问外部类的成员，则可以使用`（外部类名.成员）`去访问.

```java
package com.hspedu.innerclass;
public class StaticInnerClass01 {
  public static void main(String[] args) {
    Outer10 outer10 = new Outer10();
    outer10.m1();
    //外部其他类 使用静态内部类
    //方式 1
    //因为静态内部类，是可以通过类名直接访问(前提是满足访问权限)
    Outer10.Inner10 inner10 = new Outer10.Inner10();
    inner10.say();
    //方式 2
    //编写一个方法，可以返回静态内部类的对象实例. Outer10.Inner10 inner101 = outer10.getInner10();
    System.out.println("============");
    inner101.say();
    Outer10.Inner10 inner10_ = Outer10.getInner10_();
    System.out.println("************");
    inner10_.say();
  }
}
class Outer10 { //外部类
  private int n1 = 10;
  private static String name = "张三";
  private static void cry() {}
  //Inner10 就是静态内部类
  //1. 放在外部类的成员位置
  //2. 使用 static 修饰
  //3. 可以直接访问外部类的所有静态成员，包含私有的，但不能直接访问非静态成员
  //4. 可以添加任意访问修饰符(public、protected 、默认、private),因为它的地位就是一个成员
  //5. 作用域 ：同其他的成员，为整个类体
  static class Inner10 {
    private static String name = "韩顺平教育";
    public void say() {
      //如果外部类和静态内部类的成员重名时，静态内部类访问的时，
      //默认遵循就近原则，如果想访问外部类的成员，则可以使用 （外部类名.成员）
      System.out.println(name + " 外部类 name= " + Outer10.name);
      cry();
    }
  }
  public void m1() { //外部类---访问------>静态内部类 访问方式：创建对象，再访问
    Inner10 inner10 = new Inner10();
    inner10.say();
  }
  public Inner10 getInner10() {
    return new Inner10();
  }
  public static Inner10 getInner10_() {
    return new Inner10();
  }
}
```

