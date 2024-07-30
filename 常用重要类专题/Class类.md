## Class.getResource() 和 getClassLoader().getResource() 区别

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
> >           }
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

