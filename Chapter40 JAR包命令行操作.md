[toc]

# 文件结构

test
├── com
│   └── lyp
│       └── jar.class
├── ajar.jar
├── jar.java
├── test.class
└── test.java

jar.java的文件内容

```java
package com.lyp;
public class jar{
  public static void main(){
    System.out.println("Hello YannLau!");
  }
  public void hello(){
	System.out.println("我是jar包里的公用方法~~~");
  }
}
```

在test文件路径下使用命令 `javac -d ./ jar.java` 进行编译，按照package的结构在当前目录下生成对应结构的类文件——com/lyp/jar.class。

然后使用命令 `jar --create --file ajar.jar com/lyp/jar.class` 得到ajar.jar的jar包。

编写test.java

```java
import com.lyp.jar;
public class test{
	public static void main(String[] args){
		jar j = new jar();
		j.hello();
	}
}
```

使用命令 `javac -classpath ~/Desktop/test/classes.jar test.java`, 这里可以使用 `~` 得到test.class

使用  `java -classpath :/Users/yannlau/Desktop/test/classes.jar test` 运行。得到正确的输出。



这里有几个坑：

java命令运行class文件时不可以使用 ~代替home文件路径需要写全路径，javac可以

多路径写法：

1. path1:path2:path3  这样写不包含当前目录 先搜索path1，在搜索path2，最后搜索path3
2. :path1:path2:path3 先搜索当前路径，在按顺序搜索
3. path1:path2:path3: 最后搜索当前路径