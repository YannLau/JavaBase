【Java中的反射 Reflection in Java】 https://www.bilibili.com/video/BV1K4421w7zP/?share_source=copy_web&vd_source=b77bfd0640ba59cb7c1bb8b3ad795165

![image-20240429下午52043092](/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240429下午52043092.png)

# 获取Class对象的三种方式

1. 类字面常量.class

   >`Class<User> userClass = User.class;`
   >
   >==这种方式得到User类的Class类对象不会触发静态初始化块。==
   >
   >只有在你访问类的静态成员或者创建该类实例的时候才会被触发。

2. 对象.getClass()

   > 如果你已经有某个类的实例对象，就可以通过该对象的getClass()方法获取对应Class对象。
   >
   > `User user = new User("YannLau",20);`
   >
   > `Class<?> clazz = user.getClass();`
   >
   > 这里的泛型用通配符，这是因为该Class对象实在运行时从User实例中获取的，而user实例的具体类型只能在运行时创建和确定。我们在编译阶段无法准确地判断Class对象的确切类型，因此我们使用通配符，来表示未知的类型。如果填入User就会报错。

3. Class.forName(String classPath)静态方法，接受一个字符串参数，表示待加载类的完全限定名。

   > 用于在运行时，动态加载指定的类，并返回该类的Class对象实例。
   >
   > 通常用于类名在编译时不可知的场景中。
   >
   > `Class.forName("com.lyp.reflect.User");`
   >
   > 通过这种方法获得类的Class对象时，会立即触发类的初始化。类的初始化块，static块会被执行。

# 框架模拟

## 注解Annotation

好像一个开关，用于选择启用/禁用的功能等。

```java
package com.lyp.reflect;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

public @interface MyAnnotation {
}

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@interface SelectedService {
}

@Target(ElementType.CONSTRUCTOR)
@Retention(RetentionPolicy.RUNTIME)
@interface SelectedConstructor {
}

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@interface printable{
}
```

## Service

用于 实例化 服务。

```json
package com.lyp.reflect;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName Address
 * @ClassPath com.lyp.reflect.Address
 * @create 2024-04-29 15:10
 * @description
 */
public class Address {
    private String street;
    private String postCode;

    public Address(String street, String postCode) {
        this.street = street;
        this.postCode = postCode;
    }

    public String getStreet() {
        return street;
    }

    public String getPostCode() {
        return postCode;
    }

    @printable //表示该service服务功能被启用
    public void printStreet() {
        System.out.println("Address Street: " + street);
    }

    public void printPostCode() {
        System.out.println("Address PostCode: " + postCode);
    }
}
```

```json
package com.lyp.reflect;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName Customer
 * @ClassPath com.lyp.reflect.Customer
 * @create 2024-04-29 15:10
 * @description
 */
public class Customer {
    private String name;
    private String email;

    public Customer(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }

    @printable
    public void printName() {
        System.out.println("Customer name: " + name);
    }

    @printable
    public void printEmail() {
        System.out.println("Customer email: " + email);
    }
}
```

```java
package com.lyp.reflect;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName Message
 * @ClassPath com.lyp.reflect.Message
 * @create 2024-04-29 15:22
 * @description
 */
public class Message {
    private String msg;

    public Message(String msg) {
        this.msg = msg;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    @printable
    public void printMessage() {
        System.out.println(msg);
    }
}
```

## 被挂载服务的对象(也就是去使用服务的对象)

因为要通过反射生成对象实例的同时还要自动挂载需要的服务。因此必须符合规范才可以，要有指向服务实例的字段指向服务实例，才能调用服务。

```java
package com.lyp.reflect;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName Order
 * @ClassPath com.lyp.reflect.Order
 * @create 2024-04-29 15:09
 * @description
 */
public class Order {
    private Customer customer;//用于挂载服务
    private Address address;//用于挂载服务

    public Order() {
    }

    @SelectedConstructor//想要通过反射创建本对象并且自动挂载服务，必须要使用该注解，选择能够传入服务实例的构造器
    public Order(Customer customer, Address address) {
        this.customer = customer;
        this.address = address;
    }

    public Customer getCustomer() {
        return customer;
    }

    public void setCustomer(Customer customer) {
        this.customer = customer;
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }
}
```

```json
package com.lyp.reflect;

import java.util.List;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName User
 * @ClassPath com.lyp.reflect.User
 * @create 2024-04-29 16:52
 * @description User类想要享受和Order类同样的服务, 需要实现相应的声明规则
 */
public class User {
    public String name;
    private final int age;
    private String email;
    private Message message; //用于挂载服务
    private List<String> comments;
    public static int publicStaticField = 1;
    private static int privateStaticField = 10;

    static {
        System.out.println("UserClass is initialized");
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @SelectedConstructor //想要通过反射创建本对象并且自动挂载服务，必须要使用该注解，选择能够传入服务的构造器
    public User(Message message) {
        this.age = 18;
        this.message = message;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getEmail() {
        return email;
    }

    public List<String> getComments() {
        return comments;
    }

    public void myPublicMethod() {
        System.out.println("This is a public method");
    }

    private void myPrivateMethod() {
        System.out.println("This is a private method");
    }

    public void myPrivateMethod(String content, String mark) {
        System.out.println("This is a private method with parameters" + mark);
    }

    public void myPublicStaticMethod() {
        System.out.println("This is a public static method");
    }
}
```

## 服务配置

```java
package com.lyp.reflect;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName Config
 * @ClassPath com.lyp.reflect.Config
 * @create 2024-04-29 15:18
 * @description 定义服务
 */
public class Config {
    @SelectedService //这里相当于一个配置文件，使用该注解的方法都会被放入map中，用于创建服务Service对象
    public Customer customer() {
        return new Customer("YannLau", "YannLau@gmail.com");
    }
    @SelectedService
    public Address address() {
        return new Address("96 ZhongShan Street", "100000");
    }
    @SelectedService
    public Message message(){
        return new Message("Hi there!");
    }
}
```

## 容器

```java
package com.lyp.reflect;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.util.HashMap;
import java.util.Map;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName Container
 * @ClassPath com.lyp.reflect.Container
 * @create 2024-04-29 15:23
 * @description
 */
public class Container {
    private Map<Class<?>, Method> methods;
    private Object config;
    private Map<Class<?>, Object> services;

    public Container() throws ClassNotFoundException, InvocationTargetException, NoSuchMethodException, InstantiationException, IllegalAccessException {
        this.init();
    }

    public void init() throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        //初始化methods
        this.methods = new HashMap<>();
        this.services = new HashMap<>();
        Class<?> clazz = Class.forName("com.lyp.reflect.Config");
        Method[] methods = clazz.getDeclaredMethods();
        for (Method method : methods) {
            if (method.isAnnotationPresent(SelectedService.class)) {
                //或者用 method.getDeclaredAnnotation(SelectedService.class) != null 去判断
                //System.out.println(method.getName());
                this.methods.put(method.getReturnType(), method);
            }
        }
        this.config = clazz.getConstructor().newInstance();
    }

    public Object getServiceInstanceByClass(Class<?> clazz) throws InvocationTargetException, IllegalAccessException {
        if (services.containsKey(clazz)) {
            return services.get(clazz);
        } else if (this.methods.containsKey(clazz)) {
            Method method = this.methods.get(clazz);
            Object obj = method.invoke(this.config);
            services.put(clazz, obj);
            return obj;
        }
        return null;
    }

    public Object createInstance(Class<?> clazz) throws InvocationTargetException, IllegalAccessException, InstantiationException, NoSuchMethodException {
        Constructor<?>[] declaredConstructors = clazz.getDeclaredConstructors();
        for(Constructor<?> constructor : declaredConstructors) {
            if (constructor.isAnnotationPresent(SelectedConstructor.class)) {
                //System.out.println(constructor);
                Class<?>[] parameterTypes = constructor.getParameterTypes();
                Object[] arguments = new Object[parameterTypes.length];
                for (int i = 0; i < parameterTypes.length; i++) {
                    arguments[i] = getServiceInstanceByClass(parameterTypes[i]);
                }
                //自动将服务注入到实例当中
                return constructor.newInstance(arguments);
            }
        }
        //如果没有任何构造器被注解
        return clazz.getConstructor().newInstance();
    }
}
```

## 测试

```java
package com.lyp.reflect;

import org.junit.jupiter.api.Test;

import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

/**
 * @author YannLau
 * @version 1.0
 * @program untitled
 * @ClassName Main
 * @ClassPath com.lyp.reflect.Main
 * @create 2024-04-29 15:28
 * @description
 */
public class Main {

    @Test
    public void test1() throws Exception {
      
        Container container = new Container();//创建容器并自动

        String className = "com.lyp.reflect.User";
        String fieldName = "message";

        Class<?> clazz = Class.forName(className);
        Object obj = container.createInstance(clazz);
        Field field = clazz.getDeclaredField(fieldName);
        field.setAccessible(true);
        Object fieldValue = field.get(obj);
        //System.out.println(fieldValue); //我们得到了message的服务对象了,证明注入服务成功了
        Method[] methods = fieldValue.getClass().getDeclaredMethods();
        //通过得到的服务,服务方法,使用服务
        for (Method method : methods) {
            if (method.isAnnotationPresent(printable.class)) {
                method.invoke(fieldValue);
            }
        }

    }
    @Test
    public void test2() throws Exception {
        Container container = new Container();

        String className = "com.lyp.reflect.Order";
        String fieldName = "address";
      	//String fieldName = "customer";

        Class<?> clazz = Class.forName(className);
        Object obj = container.createInstance(clazz);
        Field field = clazz.getDeclaredField(fieldName);
        field.setAccessible(true);
        Object fieldValue = field.get(obj);
        //System.out.println(fieldValue); //我们得到了Customer的服务对象了,证明注入服务成功了
        Method[] methods = fieldValue.getClass().getDeclaredMethods();
        //通过得到的服务,服务方法,使用服务
        for (Method method : methods) {
            if (method.isAnnotationPresent(printable.class)) {
                method.invoke(fieldValue);
            }
        }

    }
}
```