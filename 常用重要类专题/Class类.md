[toc]

# Class.getResource() 和 getClassLoader().getResource() 区别

> 总结
>
> > 目录树
> >
> > （根目录一）
> >
> > - target
> >
> >   - classes
> >
> >     - com
> >
> >       - lyp
> >
> >         - test.txt
> >
> >         - Task.class
> >
> >           ```java
> >           public class Task{
> >             public void printResource(){
> >           
> >               System.out.println( Task.class.getResource("").getPath() );
> >               System.out.println( Task.class.getResource("/").getPath() );
> >               System.out.println( Task.class.getResource("test.txt").getPath() );
> >               System.out.println( Task.class.getResource("/test.txt").getPath() );
> >           
> >               System.out.println( Task.class.getClassLoader().getResource("").getPath() );
> >               System.out.println( Task.class.getClassLoader().getResource("/").getPath() );
> >               System.out.println( Task.class.getClassLoader().getResource("test.txt").getPath() );
> >               System.out.println( Task.class.getClassLoader().getResource("/test.txt").getPath() );
> >             }
> >           }
> >           ```
> >
> >         - test.class
> >
> >           ```java
> >           public class test{
> >             public static void main(String[] args){
> >           		new Task().printResource();
> >             }
> >           }
> >           ```
> >
> >   - test-classes
> >
> >     - com
> >
> >       - lyp
> >
> >         - Test.class
> >
> >           ```java
> >           import com.lyp.Task;
> >                                         
> >           public class Test{
> >             public static void main(String[] args){
> >               new Task().printResource();
> >             }
> >           }w
> >           ```

test.class运行测试会得到结果：

```txt
.../target/classes/com/lyp
.../target/classes
.../target/classes/com/lyp/test.txt
.../target/classes/test.txt (报错，要找的文件不存在为null，对null调用getPath()，故报错，这里为了方便理解，写成路径形式)

.../target/classes
null (报错)
.../target/classes/test.txt (不存在为null，对null调用getPath()，故报错，这里为了方便理解，写成路径形式)
null (报错)
```

Test.class运行测试会得到如下结果

```txt
.../target/classes/com/lyp
.../target/test-classes
.../target/classes/com/lyp/test.txt
.../target/classes/test.txt (报错，要找的文件不存在为null，对null调用getPath()，故报错，这里为了方便理解，写成路径形式)

.../target/test-classes
null (报错)
.../target/classes/test.txt (不存在为null，对null调用getPath()，故报错，这里为了方便理解，写成路径形式)
null (报错)
```

# .class和.class.getClass()的区别

System.out.println(System.class.getClassLoader());为何结果为null

#### ChatGPT 说：

ChatGPT

在Java中，`System.out.println(System.class.getClassLoader());` 这行代码的输出为 `null` 是因为 `System` 类是由引导类加载器（Bootstrap ClassLoader）加载的，而这个类加载器在Java中被表示为 `null`。

### 详细原因：

1. **引导类加载器（Bootstrap ClassLoader）**：
   - Java虚拟机在启动时会首先加载一些核心的类库，如`java.lang.*`，`java.util.*`等，这些类库通常存放在`$JAVA_HOME/lib`目录下。
   - 这些核心类是由引导类加载器加载的。引导类加载器是用本地代码实现的，位于JVM的核心部分，用来加载最基本的类，例如`java.lang.Object`、`java.lang.System`等。
2. **引导类加载器的表示**：
   - 在Java中，JVM使用`null`来表示引导类加载器。因此，当你调用`System.class.getClassLoader()`时，由于`System`类是由引导类加载器加载的，返回值就是 `null`。
3. **代码解释**：
   - `System.class.getClassLoader()` 返回的是加载 `System` 类的类加载器。
   - 因为 `System` 类是由引导类加载器加载的，而引导类加载器在Java中表示为 `null`，所以输出结果是 `null`。

### 示例代码

```
System.out.println(System.class.getClassLoader());  // 输出：null
System.out.println(String.class.getClassLoader());  // 输出：null
System.out.println(getClass().getClassLoader());    // 通常会输出AppClassLoader或其他类加载器
```

在上面的代码中，`System.class.getClassLoader()` 和 `String.class.getClassLoader()` 都会返回 `null`，因为这些类都是由引导类加载器加载的。而 `getClass().getClassLoader()` 返回的是当前类的类加载器，它通常是应用程序类加载器（AppClassLoader）。

### 总结

`System.class.getClassLoader()` 返回 `null` 是因为 `System` 类是由引导类加载器加载的，而引导类加载器在Java中被表示为 `null`。



##### 您说：

为何System.class就可以获得System类的类对象了？



#### ChatGPT 说：

ChatGPT

在Java中，`System.class` 语法是用于获取某个类的类对象（`Class` 对象）。类对象（`Class` 对象）是Java反射机制的核心，它在JVM中表示类的元数据，包括类的名称、父类、方法、字段等信息。

### 为什么可以通过 `System.class` 获得 `System` 类的类对象？

1. **类加载和类对象的生成**：
   - 在Java程序执行期间，当JVM加载某个类时，它会创建一个对应的`Class`对象，这个对象包含了该类的所有元数据。
   - 这个`Class`对象在类加载时由JVM自动创建，并且在整个程序运行期间，它是唯一的。
2. **`Class`对象的唯一性**：
   - 每个类在JVM中只有一个对应的`Class`对象。无论你在程序中多次调用`System.class`，都返回同一个`Class<System>`对象。
   - 通过`Class`对象，你可以访问类的元数据（例如类的方法、字段、构造器），并进行反射操作。
3. **`System.class` 的作用**：
   - `System.class` 表达式是获取 `System` 类的 `Class<System>` 对象的直接方式。
   - 这个 `Class<System>` 对象是在 JVM 加载 `System` 类时自动创建的。
   - 当你调用 `System.class` 时，JVM实际上是返回了已经加载好的 `Class<System>` 对象。

### 代码示例

```
Class<System> systemClass = System.class;  // 获得System类的Class对象
System.out.println(systemClass.getName());  // 输出：java.lang.System
```

在这个例子中，`System.class` 返回的是表示 `System` 类的 `Class` 对象，然后通过 `getName()` 方法可以获取到 `System` 类的全限定名。

### 总结

`System.class` 可以获得 `System` 类的类对象（`Class` 对象）是因为当JVM加载 `System` 类时，会自动创建一个对应的 `Class<System>` 对象。这个对象在程序的整个生命周期中是唯一的，通过 `System.class` 表达式可以直接引用这个 `Class` 对象。





##### 您说：

那么System.class和System.class.getClass()有没有本质区别？



#### ChatGPT 说：

ChatGPT

`System.class` 和 `System.class.getClass()` 都与 Java 的类对象（`Class` 对象）相关，但它们之间有细微的区别：

### `System.class`

- `System.class` 是用于获取 `System` 类的 `Class` 对象的直接方式。它表示的是 `System` 类本身的类对象。

- 这是一个编译时确定的引用，它不涉及任何运行时的方法调用。

- 例子：

  ```
  Class<System> systemClass = System.class;
  ```

  在这个例子中，`systemClass` 代表 `System` 类的 `Class` 对象，包含了 `System` 类的所有元数据。

### `System.class.getClass()`

- `System.class.getClass()` 是在获取 `System` 类的 `Class` 对象后，再调用这个 `Class` 对象的 `getClass()` 方法。

- `getClass()` 方法返回的是 `Class` 类的类对象。也就是说，它返回的是表示 `Class` 类的 `Class` 对象，而不是 `System` 类本身的类对象。

- 例子：

  ```
  Class<?> systemClassClass = System.class.getClass();
  ```

  在这个例子中，`systemClassClass` 实际上表示的是 `Class` 类的类对象，而不是 `System` 类的类对象。换句话说，它是 `Class<Class<?>>` 的实例。

### 区别的具体分析

- `System.class` 表示的是 `System` 类的 `Class<System>` 对象。
- `System.class.getClass()` 表示的是 `Class` 类的 `Class` 对象，即 `Class<Class<?>>`。

**总结**：

- `System.class` 直接引用的是 `System` 类的类对象。
- `System.class.getClass()` 是 `System` 类的 `Class` 对象的类对象，即 `Class<Class<?>>`。因为 `Class` 类本身也是一个类，所以它有自己的 `Class` 对象。

这两者本质上的区别在于：`System.class` 返回的是表示 `System` 类的 `Class` 对象，而 `System.class.getClass()` 返回的是表示 `Class` 类的 `Class` 对象。





##### 您说：

但是从结果来看是否没有区别？



#### ChatGPT 说：

ChatGPT

从表面结果来看，`System.class` 和 `System.class.getClass()` 的确可能会给人一种没有区别的感觉，尤其是在直接打印输出时，它们都会输出类似的内容。但实际上，它们的含义和用途是不同的。让我们深入理解一下：

### 打印输出的情况

```
java
复制代码
System.out.println(System.class);           // 输出: class java.lang.System
System.out.println(System.class.getClass()); // 输出: class java.lang.Class
```

- `System.out.println(System.class);` 会输出 `class java.lang.System`，表示 `System` 类的 `Class` 对象。
- `System.out.println(System.class.getClass());` 会输出 `class java.lang.Class`，表示 `Class` 类的 `Class` 对象。

### 差异的具体说明

1. **`System.class`**:
   - 这是 `System` 类的 `Class` 对象，类型是 `Class<System>`。它代表 `System` 类本身。
2. **`System.class.getClass()`**:
   - `System.class.getClass()` 首先获取 `System` 类的 `Class` 对象，然后调用这个对象的 `getClass()` 方法，返回 `Class<Class<?>>` 对象。
   - 因为 `Class` 本身是一个类，`System.class` 的类型是 `Class<System>`，而 `System.class.getClass()` 返回的是 `Class` 类的类对象。

### 结果表现上的相似性

在实际使用中，特别是在打印时，`System.class` 和 `System.class.getClass()` 的输出可能容易混淆：

- 当你直接打印它们时，看到的都是 `Class` 对象的 `toString()` 结果，所以你可能会觉得它们相似。
- 但是它们本质上代表不同的对象：
  - `System.class` 是 `System` 类的类对象。
  - `System.class.getClass()` 是 `Class` 类的类对象。

### 实际应用中的区别

- 如果你试图进一步使用这些对象，如调用方法或检查类型，它们的区别会变得更加明显。
  - 例如，调用 `System.class.getName()` 会返回 `java.lang.System`，而调用 `System.class.getClass().getName()` 会返回 `java.lang.Class`。
  - 这种差异在反射和动态类型检查中尤为重要。

### 总结

尽管从结果的表面上来看，它们的打印输出可能相似，但它们的本质和含义是不同的。`System.class` 表示 `System` 类的 `Class` 对象，而 `System.class.getClass()` 表示 `Class` 类的 `Class` 对象。在具体应用中，理解这种区别非常重要，尤其是在反射或框架开发中。



### 我说：

我想知道的是System.class这条语句的.class定义在哪里？还是说只是一种规定好的语法？

#### ChatGPT 说：

ChatGPT

`.class` 是 Java 中的一种内建语法，它用于获取一个类的 `Class` 对象。它并不是在 Java 库中定义的类或方法，而是 Java 语言规范中定义的语法元素。

### `.class` 的定义

`.class` 是 Java 编程语言的一部分，定义在 [Java Language Specification (JLS)](https://docs.oracle.com/javase/specs/jls/se17/html/jls-15.html#jls-15.8.2) 中。它用于在编译时获取与特定类型关联的 `Class` 对象。

- **语法**：

  ```
  ClassName.class
  ```

  这里 `ClassName` 可以是任何有效的 Java 类型（包括类、接口、数组、原始类型如 `int`、`double` 等），通过这个语法，你可以获取该类型的 `Class` 对象。

- **用途**：

  1. **反射**：通过 `Class` 对象，可以使用 Java 反射机制来动态地检查和操作类的结构。
  2. **类型检查**：可以在运行时检查类型。
  3. **静态引用**：在编译时确定类型信息，避免使用 `Class.forName()` 这样的动态加载。

### 具体实现

`.class` 是直接由编译器解析并生成字节码的一部分。在编译时，`.class` 语法会被编译器转换为对 `Class` 对象的引用，并写入生成的字节码中。JVM在运行时读取这个字节码，直接返回相应的 `Class` 对象。

因为 `.class` 是语言级别的语法，涉及的功能（如返回 `Class` 对象的引用）由编译器和 JVM 联合实现，而不需要在标准 Java API 中有明确的方法定义。

### 示例

```
java
复制代码
Class<System> systemClass = System.class;
System.out.println(systemClass.getName()); // 输出：java.lang.System
```

在这个例子中，`System.class` 在编译时生成一个对 `Class<System>` 的引用，并且这个引用直接存储在编译后的 `.class` 文件中。

### 总结

`.class` 是 Java 语言规范中的一种语法，并不在标准库中定义，而是编译器和 JVM 处理的核心部分。它用于在编译时获取特定类型的 `Class` 对象，并且广泛用于反射、类型检查等场景。
