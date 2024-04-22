P711---P730

[toc]

# 一个需求引出反射

> • 请看下面的问题
> 1. 根据配置文件 re.properties 指定信息，创建Cat对象并调用方法hi()
>     classfullpath=com.hspedu.Cat
>     method =hi
>     老韩思考：使用现有技术，你能做的吗？
> 2. 这样的需求在学习框架时特别多，即通过外部文件配置，在不修改源码情况下，来控制程序，也符合设计模式的 ocp原则（开闭原则：不修改源码，扩容功能）
> 3. 快速入门 com.hspedu.reflection.question ReflectionQuestion.java
>
> 

```java
//根据配置文件 re.properties 指定信息,创建Cat对象并调用方法hi();

//传统的方式 new 对象 -> 调用方法
// Cat cat = new Cat();
// cat.hi(); ===> cat.cry() 修改源码.
//有了反射机制,就可以通过修改配置文件来调用不同的方法而不是修改源码


//我们尝试做一做 -> 明白反射
//1.使用Properties 类，可以读写配置文件
Properties properties = new Properties();
properties.load(new FileInputStream("src\\re.properties"));
String classfullpath = properties.get("classfullpath").toString();//"com.hspedu.Cat"
String method = properties.get("method").toString();//"hi"
System.out.println("classfullpath=" + classfullpath);
System.out.println("method=" + method);

//2.创建对象 , 传统方法 ,行不通 ==>> 反射机制
//new classfullpath(); × 语法上根本行不通

//3. 反射机制快速入门
//(1) 加载类 , 返回 Class类型的对象
Class cls = Class.forName(classfullpath);
//这里的Class是一个名为Class的类
//(2) 通过cls得到你加载的类 com.hspedu.Cat 的对象实例
Object o = cls.newInstance();
System.out.println(o.getClass()); //运行类型
//这里的转型没什么用!!! Cat cat = (Cat) o;
//因为要调用的方法你并不知道,因为是读取re.properties中的方法名!
//(3) 通过cls得到你加载的类 com.hspedu.Cat 的methodame"hi"  的方法对象
// 即 :在反射中,可以把方法视为对象(万物皆对象)
Method method1 = cls.getMethod(methodName);
//(4) 通过method1调用方法: 即通过方法对象来实现调用方法
method1.invoke(o); 
//传统方法 对象.方法()，反射机制 方法.invoke（对象）
```

# 反射机制

> Java Reflection
> 1. 反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息（比如成员变量，构造器，成员方法等等），并能操作对象的属性及方法。反射在设计模式和框架底层都会用到
> 2. 加载完类之后，在堆中就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象包含了类的完整结构信息。通过这个对象得到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，形象的称之为：反射

Class类对象 加载到堆中后,是以一种可操作的数据结构存在的!

![image-20240225下午62856410](/Users/yannlau/Library/Application Support/typora-user-images/image-20240225下午62856410.png)

> • Java反射机制可以完成
> 1. 在运行时判断任意一个对象所属的类
> 2. 在运行时构造任意一个类的对象
> 3. 在运行时得到任意一个类所具有的成员变量和方法
> 4. 在运行时调用任意一个对象的成员变量和方法
> 5. 生成动态代理

> • 反射相关的主要类：
> 1. java.lang.Class：代表一个类，Class对象表示某个类加载后在堆中的对象
> 2. java.lang.reflect.Method：代表类的方法，Method对象表示某个类的方法
> 3. java.lang.reflect.Field：代表类的成员变量
> 4. java.lang.reflect.Constructor：代表类的构造方法
>     这些类在 java.lang.reflection
> 5. 结合前面案例演示：Reflection01.java com.hspedu.reflecton

```java
//java.Lang.reflect.Field：代表类的成员变量，Field对象表示某个类的成员变量
//得到name字段
//getField不能得到私有的属性
Field nameField = cls.getField("age"); //不能访问私有字段
System.out.println（nameField.get（o））；// 传统写法 对象。成员变量，反射：成员变量对象.get（对象）
  
//java.Lang.reflect.Constructor：代表类的构造方法，Constructor对象表示构造器
Constructor constructor = cls.getConstructor();
//()中可以指定构造器参数类型，返回无参构造器
System.out.println(constructor); //Cat()

Constructor constructor2 = cls.getConstructor(String.class);//这里老师传入的 String.class 就是 String类的类对象
System.out.println(constructor2);// Cat(String name)
```

> 反射的优点和缺点
>
> 1. 优点：可以动态的创建和使用对象（也是框架底层核心），使用灵活，没有反射机制，框架技术就失去底层支撑。
>
> 2. 缺点：使用反射基本是解释执行，对执行速度有影响.
> 3. 应用实例：Reflection02.java com.hspedu.reflecton

> • 反射调用优化-关闭访问检查
> 1. Method和Field、 Constructor对象都有setAccessible()方法
> 2. setAccessible作用是启动和禁用访问安全检查的开关
> 3. 参数值为true表示 反射的对象在使用时取消访问检查，提高反射的效率。参数值为false则表示反射的对象执行访问检查

```java
public static void m3() throws ClassNotFoundException, IllegalAccessException, InstantiationException {
	Class cls = Class.forName("com.hspedu.Cat");
  Object o = cls.newInstance();
  Method hi = cls.getMethod("hi");
  hi.setAccessible(true);//在反射调用方法时,取消访问检查
  long start = System.currentTimeMillis();
  for ( int i = 0; i<900000000; i++){
  	hi.invoke(o);
  }
  long end = System.currentTimeMillis();
  System.out.println("m3() 耗时= " + (end - start));
}
```

# Class类

> • 基本介绍：
>
> 1. Class也是类，因此也继承Object类［类图］
> 2. Class类对象不是new出来的，而是系统创建的［演示］
> 3. 对于某个类的Class类对象，在内存中只有一份，因为类只加载一次［演示］,类似于模版只需要一份
> 4. 每个类的实例都会记得自己是由哪个 Class 实例所生成
> 5. 通过Class对象可以完整地得到一个类的完整结构，通过一系列API
> 6. Class对象是存放在堆的
> 7. 类的字节码二进制数据，是放在方法区的，有的地方称为类的元数据（包括 方法代码，变量名，方法名，访问权限等等）https://www.zhihu.com/question/38496907
>     【示意图】

![image-20240225下午74924011](/Users/yannlau/Library/Application Support/typora-user-images/image-20240225下午74924011.png)

```java
public class Class01 {
    public static void main(String[] args) throws ClassNotFoundException {
       //看看Class类图
       //1. Class也是类,因此继承Object类
        //Class
        //2. Class类对象不是new出来的,而是系统创建的
        // (1) 传统new对象
        /**
         * CLassLoader类
         *      public Class‹?> loadClass(String name) throws ClassNotFoundException {
         *          return loadClass(name, false);
         */
        Cat cat = new Cat();
        // (2)反射方式,刚才老师没有debug到 ClassLoader类的 loadClass，原因是，我没有注销Cat cat = new Cat();导致类已经加载过了不会重复加载
        /**
         * ClassLoader类，仍然是通过 ClassLoader类加载Cat类的 CLass对象
         *     public Class<?> loadClass(String name) throws ClassNotFoundException {
         *         return loadClass(name, false);
         *     }
         */
        Class cls1 = Class.forName("reflection.Cat");
        //3. 对于某个类的CLass类对象，在内存中只有一份，因为类只加载一次
        Class cls2 = Class. forName ("reflection.Cat");
        System.out.println(cls1.hashCode());
        System.out.println(cls2.hashCode());
    }
}
```

![image-20240225下午81610676](/Users/yannlau/Library/Application Support/typora-user-images/image-20240225下午81610676.png)

方法区:元数据引用 堆区的Class类对象

> Class类的常用方法
>
> ![image-20240225下午81758157](/Users/yannlau/Library/Application Support/typora-user-images/image-20240225下午81758157.png)

```java
package reflection;
import java.lang.reflect.Field;
public class Class0 {
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchFieldException {
        String classAllPath = "reflection.Car";
        //1. 获取到Car类 对应的 Class对象
        //<?> 表示不确定的Java类型
        Class<?> cls = java.lang.Class.forName(classAllPath);
        //2. 输出cls
        System.out.println(cls); //显示cls对象,是哪个类的Class对象 reflection.Car
        System.out.println(cls.getClass());//输出cls运行类型 java.lang.Class
        //3. 获取包名
        System.out.println(cls.getPackage().getName()); //包名
        //4. 得到全类名
        System.out.println(cls.getName());
        //5. 通过cls创建对象实例
        Car car = (Car) cls.newInstance();
        System.out.println(car);//car.toString()
        //6. 通过反射获取非私有属性 brand
        Field brand = cls.getField("brand");
        System.out.println(brand.get(car));
        //7. 通过反射给属性赋值
        brand.set(car, "奔驰");
        System.out.println(brand.get(car));
        //8. 我希望大家可以得到所有的属性(字段)
        System.out.println("===所有的字段属性===");
        Field[] fields = cls.getFields();
        for (Field field : fields) {
            System.out.println(field.getName());//名称
        }
    }
}
```

> 获取Class类对象的六种不同方式
>
> 1. 代码编译阶段___Class.forName()
> 2. Class类阶段(加载阶段)___类加载器ClassLoader
> 3. 加载完成后______类.class
> 4. Runtime阶段  对象,getClass();

> 1. 前提：已知一个类的全类名，且该类在类路径下，可通过Class类的静态方法
>     forName()获取，可能抛出ClassNotFoundException，实例：Class cls1 =
>     Class.forName（ "java.lang.Cat” ）；
>     应用场景：多用于配置文件，读取类全路径，加载类.
> 2. 前提：若已知具体的类，通过类的class 获取，该方式 最为安全可靠，程序性能
>     最高实例：Class cls2 = Cat.class：
>     应用场景：多用于参数传递，比如通过反射得到对应构造器对象.
> 3. 前提：已知某个类的实例，调用该实例的getClass（方法获取Class对象，实例：
>     Class clazz = 对象.getClassO）：//运行类型
>     应用场景：通过创建好的对象，获取Class对象.
> 4. ClassLoader cl = 对象.getClass.getClassLoader();
>     Class clazz4 = cl.loadClass（“类的全类名"）：
> 5. 基本数据（int, char,boolean,float,double, byte,long,short）按如下方式得到Class类
>     对象
>     Class cls = 基本数据类型.class
> 6. 基本数据类型对应的包装类，可以通过.TYPE 得到Class类对象
>     Class cls = 包装类.TYPE

```java
package reflection;

public class GetClass_ {
    public static void main(String[] args) throws ClassNotFoundException {
        //1. Class.forName
        String classAllPath = "reflection.Car";//通过读取配置文件获取
        Class<?>  cls1 = Class.forName(classAllPath);
        System.out.println(cls1);
        //2. 类名.class  应用场景:多用于参数传递
        Class cls2 = Car.class;
        System.out.println(cls2);
        //3. 对象.getClass() 应用场景,有对象实例
        Car car = new Car();
        Class cls3 = car.getClass();
        System.out.println(cls3);
        //4. 通过类加载器[4种]来获取到类的Class对象
        //(1)先得到类加载器car
        ClassLoader classLoader = car.getClass().getClassLoader();
        //(2)通过类加载器得到Class对象
        Class cls4 = classLoader.loadClass(classAllPath);
        System.out.println(cls4);
        // cls1 cls2 cls3 cls4其实是同一个对象
        System.out.println(cls1.hashCode());
        System.out.println(cls2.hashCode());
        System.out.println(cls3.hashCode());
        System.out.println(cls4.hashCode());

        //5. 基本数据（int,char,boolean,float,double,byte,long,short）按如下方式得到CLass类对象
        Class<Integer> integerClass = int.class;
        Class<Character> characterClass = char.class;
        System.out.println(integerClass);//int
        //6. 基本数据类型对应的包装类，可以通过 .TYPE 得到CLass类对象
        Class<Integer> type1 = Integer.TYPE;
        Class<Character> type2 = Character.TYPE;
        System.out.println(type1);//int
        System.out.println(integerClass.hashCode());
        System.out.println(type1.hashCode());
        //相等!
    }
}

```

> 哪些类型有Class对象
>
> • 如下类型有Class对象
>
> 1. 外部类，成员内部类，静态内部类，局部内部类，匿名内部类
> 1. interface：接口
>
> 3. 数组
> 4. enum：枚举
> 5. annotation：注解
> 6. 基本数据类型
> 7. void

```java
package reflection;

import java.io.Serializable;

public class HasClass {
    public static void main(String[] args) {
        Class<String> cls1 = String.class;//外部类
        Class<Serializable> cls2 = Serializable.class;//接口
        Class<Integer[]> cls3 = Integer[].class;//数组
        Class<float[][]> cls4 = float[][].class;//二维数组
        Class<Deprecated> cls5 = Deprecated.class;//注解
        //枚举
        Class<Thread.State> cls6 = Thread.State.class;
        Class<Long> cls7 = long.class;//基本数据类型
        Class<Void> cls8 = void.class;//void
        Class<Class> cls9 = Class.class;//Class
        System.out.println(cls1);
        System.out.println(cls2);
        System.out.println(cls3);
        System.out.println(cls4);
        System.out.println(cls5);
        System.out.println(cls6);
        System.out.println(cls7);
        System.out.println(cls8);
        System.out.println(cls9);

    }
}
```

# 类加载

> 反射机制是java实现动态语言的关键，也就是通过反射实现类动态加载。
>
> 1. 静态加载：编译时加载相关的类，如果没有则报错，依赖性太强
>
> 2. 动态加载：运行时加载需要的类，如果运行时不用该类，及时不存在该类 , 也不报错，降低了依赖性
> 3. 举例说明

> 类加载的时机
>
> 1. 当创建对象时(new)  //静态加载
> 2. 当子类被加载,父类也被加载 //静态加载
> 3. 调用类中的静态成员时 //静态加载
> 4. 通过反射 //动态加载

```java
package reflection;


import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.Scanner;


public class Dynamicload {
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException {
        Scanner scanner = new Scanner(System.in);
        String key = scanner.next();
        switch (key) {
            case "1" -> {
                Dog dog = new Dog(); //静态加载,依赖性很强
                dog.cry();
            }
            case "2" -> {
                //可以通过编译2
                //反射->动态加载
                Class cls = Class.forName("reflection.Person");//加载Person类[动态加载]
                Object o = cls.newInstance();
                Method m = cls.getMethod("hi");
                m.invoke(o);
                System.out.println("ok");
            }
            case "3" -> {
                System.out.println("do nothing...");
            }
        }
    }
}
//因为new Dog()是静态加载,因此必须编写Dog类
//Person类是动态加载,所以,没有编写Person类也不会报错,只有当动态加载该类时,才会报错
class Dog{
    public void cry(){
        System.out.println("小狗汪汪叫~~~");
    }
}
class Person{
    public void hi(){
        System.out.println("hi!");
    }
}
```

# 类加载流程

![image-20240226上午122721291](/Users/yannlau/Library/Application Support/typora-user-images/image-20240226上午122721291.png)

![image-20240226上午123027544](/Users/yannlau/Library/Application Support/typora-user-images/image-20240226上午123027544.png)

> 加载阶段 Loading
>
> JVM 在该阶段的主要目的是将字节码从不同的数据源（可能是 class 文件、也可能是jar 包，甚至网络）转化为二进制字节流加载到内存中，并生成一个代表该类的java.lang.Class 对象

> 链接阶段-验证
>
> 1. 目的是为了确保 Glass 文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。
> 2. 包括：文件格式验证（是否以魔数 oxcafebabe开头--cafe babe）、元数据验证、字节码验证和符号引用验证［举例说明］
> 3. 可以考虑使用-Xverifyinone 参数来关闭大部分的类验证措施，缩短虚拟机类加载的时间。

> 链接阶段--准备
>
> 1.JVM 会在该阶段对静态变量，分配内存并默认初始化（对应数据类型的默认初始值，如0、0L、null、false 等）。这些变量所使用的内存都将在方法区中进行分配

```java
class A {
    //属性-成员变量-字段
    //老韩分析类加载的链接阶段-准备 属性是如何处理
    //1.n1 是实例属性，不是静态变量，因此在准备阶段，是不会分配内存
    //2．n2是静态变量，分配内存 n2 是默认初始化0，而不是20
    //3.n3 是static final 是常量，他和静态变量不一样，因为一旦赋值就不变 n3 = 30
    public int n1 = 10;
    public static int n2 = 20;
    public static
    final int n3 = 30;
}

```

> 链接阶段-解析
>
> 1. 虚拟机将常量池内的符号引用替换为直接引用的过程。

> • Initialization（初始化）
> 1. 到初始化阶段，才真正开始执行类中定义的Java 程序代码，此阶段是执行<clinit＞()方法的过程。
> 2. <clinit>()方法是由编译器按语包在源文件中出现的顺序，依次自动收集类中的所有静态变量的赋值动作和静态代码块中的语句，并进行合并。［举例说明
>   ClassLoad03.javal
> 3. 虚拟机会保证一个类的<clinit>()方法在多线程环境中被正确地加锁、同步，如果多个线程同时去初始化一个类，那么只会有一个线程去执行这个类的<clinit>()方法，其他线程都需要阻塞等待，直到活动线程执行<clinit>()方法完毕

```java
public class ClassLink {
    public static void main(String[] args) {
        //1.加载B类，并生成 B的class对象
        //2.链接num = 0
        //3. 初始化阶段
        //      依次自动收集类中的所有静态变量的赋值动作和静态代码块中的语句,并合并
        /**
         *  clinic(){
         *     System.out.println("B 静态代码块被执行");
         *     //num = 300;
         *     num = 100;
         *  }
         *  合并 num = 100;
         */
        System.out.println(B.num);//100，如果直接使用类的静态属性，也会导致类的加载

        //看看加载类的时候，是有同步机制控制
        B b = new B();
        /*
            protected Class‹?> loadClass(String name, boolean resolve) throws ClassNotFoundException{
             //正因为有这个机制，才能保证某个类在内存中，只有一份Class对象
                synchronized (getClassLoadingLock(name)) {
                    //....
                }
            }
        */
    }
}
class B {
    static {
        System.out.println("B 静态代码块被执行");
        num = 300;
    }
    static int num = 100;
  //静态成员按照顺序执行,因此这里num最终被赋值为100
    public B() {
        System.out.println("B()构造器被调用");
    }
}

```

# 通过反射获取类的结构信息

> 第一组
>
> 1. getName：获取全类名
> 2. getSimpleName：获取简单类名
> 3. getFields：获取所有public修饰的属性，包含本类以及父类的
> 4. getDeclaredFields：获取本类中所有属性
> 5. getMlethods：获取所有public修饰的方法，包含本类以及父类的
> 6. getDeclaredMethods：获取本类中所有方法
> 7. getConstructors：获取所有public修饰的构造器，包含本类以及父类的
> 8. getDeclaredConstructors：获取本类中所有构造器
> 9. getPackage：以Package形式返回 包信息
> 10. getSuperClass：以Class形式返回父类信息
> 11. getlnterfaces：以Class［形式返回接口信息
> 12. getAnnotations：以Annotation[] 形式返回注解信息

> 第二组 Field
>
> 1. getMlodifiers：以 int 形式返回修饰符
>   ［说明：默认修饰符 是0，public 是1, private 是2，protected 是4，static 是8, final 是 16］， public（1） + static （8） = 9
> 2. getType：以Class形式返回类型
> 3. getName：返回属性名

> 第三组 Method
>
> 1. getMlodifiers：以int形式返回修饰符
> ［说明：默认修饰符 是0，public是1，private 是2, protected 是4 , static 是 8, final 是 16
> 2. getReturnType：以Class形式获取 返回类型
> 3. getName：返回方法名
> 4. getParameterTypes：以Class[]返回参数类型数组

> 第四组 Constructor
>
> 1. getMlodifiers：以int形式返回修饰符
> 2. getName：返回构造器名（全类名）
> 3. getParameterTypes：以Class[]返回参数类型数组

```java
package reflection;

import java.lang.annotation.Annotation;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class ClassMethod {
    public static void main(String[] args) throws ClassNotFoundException {
        //ClassMethod.api_01();
        ClassMethod.api_02();
    }
    public static void api_02() throws ClassNotFoundException {
        //得到Class对象
        Class<?> peopleCls = Class.forName("reflection.People");
        //getDeclaredFields：获取本类中所有属性
        Field[] declaredFields = peopleCls.getDeclaredFields();
        for (Field declaredField : declaredFields) {
            System.out.println("本类所有属性 = " + declaredField.getName()
                    + "该属性的修饰符值 = " + declaredField.getModifiers() +
                    " 该属性的类型 = " + declaredField.getType());
        }
        //getMethods：获取本类的所有方法
        Method[] methods = peopleCls.getDeclaredMethods();
        for (Method method : methods) {
            System.out.println("=======");
            System.out.println("本类及父类的public方法 = " + method.getName()
                    + " 该方法的访问修饰符的值 = " + method.getModifiers()
                    + " 该方法的返回类型 = " + method.getReturnType());
            //输出当前这个方法的形参数组情况
            Class<?>[] parameterTypes = method.getParameterTypes();
            for (Class<?> parameterType : parameterTypes) {
                System.out.println("该方法" + method.getName() + "的形参类型为= " + parameterType);
            }
        }
        Constructor<?>[] constructors = peopleCls.getDeclaredConstructors();
        for (Constructor<?> constructor : constructors) {
            System.out.println("=======");
            System.out.println("本类的所有构造器 = " + constructor.getName());
            Class<?>[] parameterTypes = constructor.getParameterTypes();
            for (Class<?> parameterType : parameterTypes) {
                System.out.println(constructor.getName() + "该构造器的形参类型 = " +
                parameterType);
            }
        }
    }
    public static void api_01() throws ClassNotFoundException {
        //得到Class对象
        Class<?> peopleCls = Class.forName("reflection.People");
//        1. getName：获取全类名
        System.out.println(peopleCls.getName());
//        2. getSimpleName：获取简单类名
        System.out.println(peopleCls.getSimpleName());
//        3. getFields：获取所有public修饰的属性，包含本类以及父类的
        for (Field field : peopleCls.getFields()) {
            System.out.println("本类及父类的public属性 = " + field.getName());
        }
//        4. getDeclaredFields：获取本类中所有属性
        for (Field declaredField : peopleCls.getDeclaredFields()) {
            System.out.println("本类所有属性 = " + declaredField.getName());
        }
//        5. getMethods：获取所有public修饰的方法，包含本类以及父类(包括Object)的
        for (Method method : peopleCls.getMethods()) {
            System.out.println("本类及父类的public方法 = " + method.getName());
        }
//        6. getDeclaredMethods：获取本类中所有方法
        for (Method declaredMethod : peopleCls.getDeclaredMethods()) {
            System.out.println("本类所有方法 = " + declaredMethod.getName());
        }
//        7. getConstructors：获取所有public修饰的构造器，只包含本类的,父类的不行
        for (Constructor<?> constructor : peopleCls.getConstructors()) {
            System.out.println("本类的public构造器 = " + constructor.getName());
        }
//        8. getDeclaredConstructors：获取本类中所有构造器
        for (Constructor<?> declaredConstructor : peopleCls.getDeclaredConstructors()) {
            System.out.println("本类所有的构造器 = " + declaredConstructor.getName());//这里只是输出名字,不带形参
        }
//        9. getPackage：以Package形式返回 包信息
        System.out.println(peopleCls.getPackage()); //package reflection
//        10. getSuperClass：以Class形式返回父类信息
        Class<?> superclass = peopleCls.getSuperclass();
        System.out.println("父类的class对象 = " + superclass);//class reflection.ABC
//        11. getInterfaces：以Class[]形式返回接口信息
        Class<?>[] interfaces = peopleCls.getInterfaces();
        for (Class<?> anInterface : interfaces) {
            System.out.println("本类实现的接口为 = " + anInterface.getName());
        }
//        12. getAnnotations：以Annotation[] 形式返回注解信息
        for (Annotation annotation : peopleCls.getAnnotations()) {
            System.out.println("本类的注解信息 = " + annotation);
        }
    }
}
class ABC {
    public String hobby;
    private String lover = "wmj";

    public ABC() {
    }

    public void hi() {
    }
}
interface IA {
}
interface IB {
}
@Deprecated
class People extends ABC implements IA, IB {
    //属性
    public String name;
    protected static int age;
    String job;
    private double sal;
    //构造器
    public People() {
    }
    public People(String name) {
    }
    private People(int age) {
    }
    //方法
    public void m1(String name, int age, double sal) {
    }
    protected String m2() {
        return null;
    }
    void m3() {
    }
    private void m4() {
    }
}
```

# 通过反射创建对象

> 1. 方式一：调用类中的public修饰的无参构造器
>
> 2. 方式二：调用类中的指定构造器
> 3. Class类相关方法
> • newlnstance：调用类中的无参构造器，获取对应类的对象
> • getConstructor（Class...clazz）：根据参数列表，获取<u>***对应的public构造器***</u>对象
> • getDecalaredConstructor（Class...clazz）：根据参数列表，获取对应的所有构造器对象
> 4. Constructor类相关方法
> • setAccessible：暴破
> •newlnstance（Object...obj）：调用构造器

```java
package reflection;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

public class ReflectionCreateInstance {
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException {
        //1. 先获取到User类的Class对象
        Class<?> userCls = Class.forName("reflection.User");
        //2. 通过public的无参构造器创建实例
        Object o = userCls.newInstance();
        System.out.println(o);
        //3. 通过public的有参构造器创建实例
        /*
            constructor对象就是
            public User(String name){
                this.name = name;
            }
         */
        Constructor<?> constructor = userCls.getConstructor(String.class);
        Object o1 = constructor.newInstance("wmj");
        System.out.println(o1);
        //4. 通过非公有的有参构造器创建实例
        //4.1 得到私有的构造器对象
        Constructor<?> declaredConstructor = userCls.getDeclaredConstructor(int.class, String.class);
        //4.2 创建实例
        //暴破[暴力破解],使用反射可以访问private构造器/方法/属性___反射面前,都是纸老虎
        declaredConstructor.setAccessible(true);
        Object o2 = declaredConstructor.newInstance(88, "就发");
        System.out.println(o2);
    }
}

class User {
    private int age = 10;
    private String name = "lyp";

    public User() {
    }
    public User(String name){
        this.name = name;
    }
    private User(int age, String name) {
        this.age = age;
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}

```

# 通过反射访问类中的成员

> • 访问属性
> ReflecAccessProperty.java
> 1. 根据属性名获取Field对象
>   Field f = clazz对象.getDeclaredField（属性名）：
> 2. AiR: f.setAccessible(true); //f #Field
> 3. 访问
>   f.set(o,值)：// o表示对象
>   syso(f.get(o))：//o 表示对象
> 4. 注意：如果是静态属性，则set和get中的参数o，可以写成null

```java
package reflection;

import java.lang.reflect.Field;

public class ReflectionAccessProperty {
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchFieldException {
       //1. 得到Student类对应的Class类对象
        Class<?> studentCls = Class.forName("reflection.Student");
        //2.创建对象
        Object o = studentCls.newInstance();//o的运行类型就是Student
        System.out.println(o.getClass());//Student
        //3. 使用反射得到age属性对象
        Field age = studentCls.getField("age");
        age.set(o,88);
        System.out.println(o);
        System.out.println(age.get(o));
        //4. 使用反射操作name属性
        Field name = studentCls.getDeclaredField("name");
        //对name进行暴破,可以操作private属性
        name.setAccessible(true);
        //name.set(o,"lyp");
        name.set(null,"lyp~");//因为name是static属性,因此o也可以写成null
        System.out.println(o);
        System.out.println(name.get(o));
        System.out.println(name.get(null));
    }
}
class Student{
    public int age;
    private static String name;
    public Student(){ //构造器
    }

    @Override
    public String toString() {
        return "Student{" +
                "age=" + age +
                ", name=" + name +
                '}';
    }
}
```

# 通过反射访问类中的方法

> 1. 根据方法名和参数列表获取Method方法对象：Method m = clazz.getDeclaredMethod(方法名, XX.class); //Declared可以得到本类的所有的方法
> 2. 获取对象：Object o=clazz.newlnstance（）：
> 3. 暴破：m.setAccessible（true）：
> 4. 访问：Object returnValue = m.invoke(o,实参列表);//o就是对象
> 5. 注意：如果是静态方法，则invoke的参数o，可以写成null！

```java
package reflection;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class ReflectionAccessMethod {
    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, InvocationTargetException {
        //1.得到Boss类的对应的 Class对象
        Class<?> bossCls = Class.forName("reflection.Boss");
        //2. 创建对象
        Object o = bossCls.newInstance();
        //3. 调用public的hi方法
        //3.1 得到hi方法对象
        //Method hi = bossCls.getMethod("hi",String.class);//ok
        Method hi = bossCls.getDeclaredMethod("hi", String.class);//ok
        //3.2 调用
        hi.invoke(o, "lypnb");
        //4. 调用private static方法
        //4.1 获取say方法对象
        Method say = bossCls.getDeclaredMethod("say", int.class, String.class, char.class);
        //private需要暴破
        say.setAccessible(true);
        //4.2 调用
        System.out.println(say.invoke(o, 100, "wmj", '女'));
        //4.3 因为say方法是static,还可以这样用,传入null
        System.out.println(say.invoke(null,22,"wmj",'女'));
        //5.在反射中，如果方法有返回值，统一返回Object，但是他运行类型和方法定义的返回类型一致
        Object reVal = say.invoke(null,300,"王五",'男');
        System.out.println("reVal 的运行类型=" + reVal.getClass());//String
    }
}

class Boss { //类
    public int age;
    private static String name;

    public Boss() {
    }

    private static String say(int n, String s, char c) {
        return n + " " + s + " " + c;
    }

    public void hi(String s) {
        System.out.println("hi " + s);
    }
}
```

