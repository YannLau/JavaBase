P611---P644

[toc]

# 文件

> 文件流
>
> 文件在程序中是以流的形式来操作的
>
> java程序(内存)  输出流---> 文件(磁盘)
>
> Java程序(内存) 输入流 <--- 文件(磁盘)
>
> 当然，在类Unix系统中所有的设备都是文件。

流：数据在数据源（文件）和程序（内存）之间经历的路径
输入流：数据从数据源（文件）到程序（内存）的路径
输出流：数据从程序（内存）到数据源（文件）的路径

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240217下午83711410.png" alt="image-20240217下午83711410" style="zoom: 33%;" />

# 文件的常用操作

> 创建文件对象相关构造器和方法
>
> new File（Strinq pathname） //根据路径构建一个File对象
> new File（File parent,String child）//根据父目录文件＋子路径构建
> new File（String parent , String child） //根据父目录＋子路径构建

`e:\\（Windows写法，因为转义所以两个\） 可以 写成 e:/（类Unix文件路径写法）`

```java
		//方式1 new File（String pathname）
public void create01() {
	String filePath = "e:\\news1.txt";
	File file = new File(filePath);
	try{
		file.createNewFile();
		System.out.printLn("文件创建成功");
	} catch(IOException e){
		e.printStackTrace);
  }
}
//  e:\\ 可以 写成 e:/
//方式 2 new File(File parent,String child) //根据父目录文件+子路径构建
//e:\\news2.txt
public void creat02(){
  File parentFile = new File("e:\\");
  String fileName = "new2.txt";
//这里的 file 对象，在 java 程序中，只是一个对象
//只有执行了 createNewFile 方法，才会真正的，在磁盘创建该文件
  File file = new File(parentFile, fileName);
  try {
  	file.createNewFile();
		System.out.println("创建成功~");
  } catch ( IOException e) {
    e.printStackTrace();
  }
}
//方式 3 new File(String parent,String child) //根据父目录+子路径构建
public void creat03(){
	String parentPath = "e:\\";
  String fileName = "new3.txt";
  File file = new File(parentPath, fileName);
  try {
  	file.createNewFile();
		System.out.println("创建成功~");
  } catch ( IOException e) {
    e.printStackTrace();
  }
}
```

- 下面四个都是抽象类
  - InputStream
  - OutputStream
  - Reader
  - Writer

> 获取文件信息的常用方法
>
>  <img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240418下午25553799.png" alt="image-20240418下午25553799" style="zoom:50%;" />
>
> getName getAbsolutePat getParent length exists isFile isDirectory
>
> 演示如何获取到文件的大小，文件名，路径，父file，是文件还是目录（目录的本质也是文件，一种特殊的文件）是否存在。。。
>
> ```java
> File file = new File("e:/new1.txt");
> 
> //调用相应的方法,得到对应的信息
> System.out.println("文件名字 = " + file.getName());
> System.out.println("文件绝对路径 = " + file.getAbsoluteath());
> System.out.println("文件父级目录 = " + file.getParent());
> System.out.println("文件大小(按字节计算) = " + file.length());
> System.out.println("文件是否存在 = " + file.exists());
> System.out.println("是不是一个文件 = " + file.isFile());
> System.out.println("是不是一个目录 = " + file.isDirectory());
> ```

> 文件目录操作
>
> mkdir创建一级目录 mkdirs创建多级目录 delete删除空目录或者文件
>
> ```java
> //判断文件是否存在,存在就删除,否则提示不存在
> String filePath = "E:\\news1.txt";
> File file = new File(filePath);
> if (file.exists()) {
>   if(file.delete()) {
>     System.out.println（filePath + "删除成功"）；
>   } else {
>     System.out.println（filePath + "删除失败"）；
>   }else {
>     System.out.println（"该文件不存在。.");
>   }
> }
> 
> //判断目录是否存在,存在就删除,否则提示不存在
> //这里我们需要体会到，在java编程中，目录也被当做文件
> String filePath = "E:\\demo2";
> File file = new File(filePath);
> if (file.exists()) {
>   if(file.delete()) {
>     System.out.println（filePath + "删除成功"）；
>   } else {
>     System.out.println（filePath + "删除失败"）；
>   }else {
>     System.out.println（"该目录不存在。.");
>   }
> }
> //判断 D:\demo\a\b\c 目录是否存在，如果存在就提示已经存在，否则就创建
> String directoryPath = "D:\\demo\\a\\b\\c";
> File file = new File(directoryPath);
> if (file.exists()) {
>   System.out.println（directorypath + "该路径存在。.");
> } else {
>   if(file.mkdirs()){
>     //mkdir只能创建一级目录
>     System.out.println（directorypath + "创建成功。.");
>   } else {
>     System.out.println（directorypath + "创建失败。.");
>   }
> }
> ```

# IO流原理以及流的分类

> Java IO流原理
>
> 1. I/O是Input/Output的缩写，I/O技术是非常实用的技术，用于处理数据传输。如读/写文件，网络通讯等。
> 2. Java程序中，对于数据的输入/输出操作以”流（stream）”的方式进行。
> 3. java.io包下提供了各种“流”类和接口，用以获取不同种类的数据，井通过方法输入或输出数据
> 4. 输入input：读取外部数据（磁盘、光盘等存储设备的数据）到程序（内存）中。
> 5. 输出output： 将程序（内存）数据输出到磁盘、光盘等存储设备中.

> 流的分类
>
> 1. 按操作数据单位不同分为：字节流（8 bit）二进制文件，字符流（按字符） 文本文件
> 2. 按数据流的流向不同分为：输入流，输出流
> 3. 按流的角色的不同分为：节点流，处理流/包装流

| (抽象基类) |    字节流    | 字符流 |
| :--------: | :----------: | :----: |
|   输入流   | InputStream  | Reader |
|   输出流   | OutputStream | Writer |

使用时要创建以上四个类的实现子类

> 1.  Java的IO流共涉及40多个类，实际上非常规则，都是从如上4个抽象基类派生的。
> 2.  由这四个类派生出来的子类名称都是以其父类名作为子类名后缀。

# IO流体系图

## IO流体系图 -- 常用的类

> ![image-20240217下午94841197](/Users/yannlau/Library/Application Support/typora-user-images/image-20240217下午94841197.png)

## InputStream: 字节输入流

> - InputStream: 字节输入流
>
> - InputStream 抽象类是所有字节输入流类的超类
> -  InputStream 常用的子类
>   1. FilelnputStream：文件输入流
>   2. BufferedInputStream： 缓冲字节输入流
>   3. ObjectlnputStream：对象字节输入流

![image-20240217下午95254970](/Users/yannlau/Library/Application Support/typora-user-images/image-20240217下午95254970.png)

### FilelnputStream：文件输入流

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.FileNotFoundException;

public class FileInputStream_ {
    public static void main(String[] args) {
        FileInputStream_ iotest = new iotest();
        iotest.readFile01();
        iotest.readFile02();
    }

    /**
     * 演示读取文件
     * 使用 read() 方法读取单个字节
		 * 单个字节的读取，效率比较低
		*/
    public void readFile01() {
        FileInputStream fileInputStream = null;
        String filePath = "/Users/yannlau/Downloads/file1.txt";
        int readData;
        try {
            //创建 FileInputStream 对象,用于读取文件
            fileInputStream = new FileInputStream(filePath);
            //从该输入流读取 一个字节（字节流） 的数据。如果没有输入可用，此方法将阻止。
            //如果返回-1，表示读取完毕
            while ((readData = fileInputStream.read()) != -1) {
                System.out.println((char) readData);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            //关闭文件流 , 释放资源
            if (fileInputStream != null)
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
        }
    }
    /**
     * 使用 read(byte[] b) 方法读取文件,提高效率
     */
    public void readFile02(){
        FileInputStream fileInputStream = null;
        String filePath = "/Users/yannlau/Downloads/file1.txt";
        byte[] buf = new byte[20];
        int readLen = 0;
        try {
            //创建 FileInputStream 对象,用于读取文件
            fileInputStream = new FileInputStream(filePath);
            //从该输入流读取一个字节的数据。如果没有输入可用，此方法将阻止。
            //如果返回-1，表示读取完毕
            //如果读取正常，返回实际读取的字节数
            while ((readLen=fileInputStream.read(buf)) != -1) {
                System.out.println(new String(buf,0,readLen));
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            //关闭文件流 , 释放资源
            if (fileInputStream != null)
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
        }
    }
}
```

### FileOutPutStream 文件输出流

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240218上午103013668.png" alt="image-20240218上午103013668" style="zoom:50%;" />

要求: 请使用 FileOutputStream 在 a.txt 文件，中写入 “hello，world”. [老师代码演示], 如果文件不存在，会创建文件(注意：前提是目录已经存在.）

```java
class FileOutputStream01 {
  public static void main(String[] args) {
    FileOutputStream01 fileOutputStream01 = new FileOutputStream01();
    fileOutputStream01.writeFile();
  }
  /**
* 演示使用 FileOutputStream 将数据写到文件中, * 如果该文件不存在，则创建该文件
*/
  public void writeFile() {
    //创建FileOutputStream对象
    String filePath = "/Users/yannlau/Downloads/file2.txt";
    FileOutputStream fileOutputStream = null;
    try {
      //得到了相应的对象
      //老师说明
      //1. new FileOutputStream(filePath) 创建方式，当写入内容是，会覆盖原来的内容
      //2. new FileOutputStream(filePath, true) 创建方式，当写入内容是，是追加到文件后面
      fileOutputStream = new FileOutputStream(filePath);
      // 以下三种方法都会覆盖原来的文件内容
      //new FileOutputStream(filePath)
      //创建方式，当写入内容是，会覆盖原来的内容
      //new FileOutputstream（filePath,true）创建方式，
      // 当写入内容时，是追加到文件后面

      //1.写入一个字节
      //fileOutputStream.write('x'); //char -> int
      //2.写入字符串
      String str = " fuck! world! ";
      //str.getBytes() 可以把 字符串->字节数组
      //fileOutputStream.write(str.getBytes());
      //write(bytel[] b,int off, int Len） 将 Len字节从位于偏移量off的指定字节数
      fileOutputStream.write(str.getBytes(),0,str.length());
    } catch ( IOException e) {
      e.printStackTrace();
    } finally {
      try {
        fileOutputStream.close();
      } catch (IOException e) {
        throw new RuntimeException(e);
      }
    }
  }
}
```

### 文件拷贝

要求: 编程完成图片/音乐 的拷贝. [老师代码演示

```java
class FileCopy {
    public static void main(String[] args) {
        //完成 文件拷贝，将e：\Koala.jpg 拷贝到 c:\
        //思路分析
        //1.创建文件的输入流，将文件读入到程序
        //2.创建文件的输出流，将读取到的文件数据，写入到指定的文件。
        FileInputStream fileInputStream = null;
        FileOutputStream fileOutputStream = null;
        String srcFilePath = "/Users/yannlau/Downloads/墙纸/wallpaper.jpg";
        String destFilePath = "/Users/yannlau/file3.jpg";
        try {
            int read_size = 0;
            byte[] buf = new byte[1024];
            fileInputStream = new FileInputStream(srcFilePath);
            fileOutputStream = new FileOutputStream(destFilePath);
            while ((read_size = fileInputStream.read(buf)) != -1) {
                fileOutputStream.write(buf, 0, read_size);
            }
          	System.out.println("拷贝成功~");
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fileInputStream != null) {
                    fileInputStream.close();
                }
                if (fileOutputStream != null) {
                    fileOutputStream.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

### FileReader 字符输入流

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240218上午113926948.png" alt="image-20240218上午113926948" style="zoom:33%;" />

> FileReader相关方法：
>
> 1. new FileReader(File/String)
> 2. read()：每次读取单个字符，返回该字符，如果到文件末尾返回-1
> 3. read(char[])：批量读取多个字符到数组，返回读取到的字符数，如果到文件末尾返回-1
> 4. 相关API：
>    1） new String(char[]) ：将char[]转换成String
>    2） new String(char[]，off , len)：将char[]的指定部分转换成String

```java
class FileReader_ {
    public static void main(String[] args) {
        FileReader_ fileReader = new FileReader_();
        fileReader.readFile01();
        fileReader.readFile02();
    }
    //单个字符读取文件
    public void readFile01() {
        String filePath = "/Users/yannlau/Downloads/file1.txt";
        //1.创建Filereader对象
        FileReader fileReader = null;
        int data = 0;
        try {
            fileReader = new FileReader(filePath);
            //循环读取 使用read , 单个字符读取
            while ((data = fileReader.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fileReader != null) {
                try {
                    fileReader.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
    //字符数组读取文件
    public void readFile02() {
        String filePath = "/Users/yannlau/Downloads/file1.txt";
        //1.创建Filereader对象
        FileReader fileReader = null;
        int readLen = 0;
        char[] buf = new char[8];
        try {
            fileReader = new FileReader(filePath);
            //1循环读取 使用read（buf），返回的是实际读取到的字符数
            //如果返回-1，说明到文件结束
            while ((readLen = fileReader.read(buf)) != -1) {
                System.out.print(new String(buf,0,readLen));
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fileReader != null) {
                try {
                    fileReader.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
}
```



### FileWriter 字符输出流

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240218下午120713158.png" alt="image-20240218下午120713158" style="zoom:33%;" />

> FileWriter 相关方法
>
> • FileWriter常用方法
> 1） new FileWriter（File/String）：覆盖模式，相当于流的指针在首端
> 2） new FileWriter（File/String,true）：追加模式，相当于流的指针在尾端
> 3） write（int）：写入单个字符
> 4） write（char[]）：写入指定数组
> 5） write（char[] , off , len）：写入指定数组的指定部分
> 6） write （string）：写入整个字符串
> 7） write（string , off , len）：写入字符串的指定部分
> 相关API:String类：toCharArray：将String转换成char[]

```java
class FileWriter_{
    public static void main(String[] args) {
        String filePath = "/Users/yannlau/Downloads/file2.txt";
        //创建FileWriter对象
        FileWriter fileWriter = null;
        char[] chars = {'H','e','l','l','o'};
        try {
            fileWriter = new FileWriter(filePath);// true 开启追加模式
            //1. fileWriter.write(int i); 写入单个字符
            fileWriter.write('H');
            //2. write(char[]): 写入指定数组
            fileWriter.write(chars);
            //3. write(char[],offset,len);
            fileWriter.write("我爱刘云鹏".toCharArray(),3,2);
            //4. write(String);
            fileWriter.write("你好北京!");
            //5. write(String,off,len);
            fileWriter.write("上海天津",2,2);

        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            //FileWriter使用后，必须要关
            // 闭（close）或刷新（flush），否则写入不到指定的文件！
            //但是会把原来的内容清空
            //源码说明了这个问题所在
            try {
                //fileWriter.flush();
                //关闭文件流，等价 flush() + 关闭
                //fileWriter.flush();
                fileWriter.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

        }
    }
}
```

FileWriter使用后，必须要关闭（close）或刷新（flush），否则写入不到指定的文件！

```java
//对应 FileWriter , 一定要关闭流，或者 flush 才能真正的把数据写入到磁盘中的文件
//老韩看源码就知道原因. 
/*看看代码
  private void writeBytes() throws IOException {
  this.bb.flip();
  int var1 = this.bb.limit();
  int var2 = this.bb.position();
  assert var2 <= var1;
  int var3 = var2 <= var1 ? var1 - var2 : 0;
  if (var3 > 0) {
    if (this.ch != null) {
      assert this.ch.write(this.bb) == var3 : var3;
    } else {
      this.out.write(this.bb.array(), this.bb.arrayOffset() + var2, var3);
    }
  }
  this.bb.clear();
}
*/
```



# 节点流和处理流

![image-20240218下午31747460](/Users/yannlau/Library/Application Support/typora-user-images/image-20240218下午31747460.png)

> 基本介绍
>
> 1. 节点流可以从一个特定的数据源**读写数据**，如FileReader、FileWriter［源码］
> 2. 处理流（也叫包装流）是“连接“在已存在的流（节点流或处理流）之上，为程序提供更为强大的读写功能，也更加灵活, 如BufferedReader、BufferedWriter［源码］
> 3. 包装流运用了一种设计模式->修饰器模式

![image-20240218下午31945367](/Users/yannlau/Library/Application Support/typora-user-images/image-20240218下午31945367.png)

> 节点流和处理流的区别和联系
>
> 1. 节点流是底层流/低级流，直接跟数据源相接。
> 2. 处理流 (包装流) 包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出。［源码理解］
> 3. 处理流（也叫包装流）对节点流进行包装，使用了修饰器设计模式，不会直接与数据源相连［模拟修饰器设计模式］

> 处理流的功能主要体现在以下两个方面：
>
> 1. 性能的提高：主要以增加缓冲的方式来提高输入输出的效率。
> 2. 操作的便捷：处理流可能提供了一系列便捷的方法来一次输入输出大批量的数据，使用更加灵活方便

![image-20240218下午32641586](/Users/yannlau/Library/Application Support/typora-user-images/image-20240218下午32641586.png)

## 设计模式-修饰器模式介绍

```java
abstract class Reader_ { //抽象类
    public void readFile() {
    }
    public void readString() {
    }
  	//在Reader_ 抽象类，使用read方法统一管理.
		//后面在调用时，利于对象动态绑定机制，绑定到对应的实现子类即可。
		//public abstract void read();
  
    public static void main(String[] args) {
        BufferedReader_ bufferedReader = new BufferedReader_(new FileReader_());
        bufferedReader.readFiles(10);

        //这次希望通过 BufferedReader_ 多次读取字符串
        BufferedReader_ bufferedReader1 = new BufferedReader_(new StringReader_());
        bufferedReader1.readStrings(10);
        bufferedReader1.readFiles(10);
    }
}

//节点流
class StringReader_ extends Reader_ {
    public void readString() {
        System.out.println("读取字符串...");
    }
}
//节点流
class FileReader_ extends Reader_ {
    public void readFile() {
        System.out.println("对文件进行读取...");
    }
}
//做成处理流(包装流)
class BufferedReader_ extends Reader_ {
    private Reader_ reader_;//属性是Reader_类型
    public BufferedReader_(Reader_ reader) {
        this.reader_ = reader;
    }
    //封装一层
    public void readFile(){
        reader_.readFile();
    }
    //让方法更加灵活,扩展方法,多次读取文件
    public void readFiles(int num) {
        for (int i = 0; i < num; i++) {
            reader_.readFile();
        }
    }
    //扩展readString,批量处理字符串数据
    public void readStrings(int num) {
        for (int i = 0; i < num; i++) {
            reader_.readString();
        }
    }
}
```

## BufferedReader 和 BufferedWriter

1. BufferedReader 和 BufferedWriter 属于字符流，是按照字符来读取数据的
2. <u>***关闭处理流时，只需要关闭外层流即可***</u>［后面看源码］,内层节点流会被自动关闭.

> BufferedReader应用案例
>
> 1. 使用BufferedReader读取文本文件，并显示在控制台
>
> ```java
> class BufferedReader_test {
>     public static void main(String[] args) throws Exception {
>         String filePath = "/Users/yannlau/Downloads/file1.txt";
>         //创建BufferedReader
>         BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
>         //读取
>         String line;//按行读取,效率高
>         //说明
>         //1. bufferedReader.readLine （） 是按行读取文件
>         //2.当返回nULl 时，表示文件读取完毕
>         while((line = bufferedReader.readLine())!=null){
>             System.out.println(line);
>         }
>         //关闭流，这里注意，只需要关闭 BufferedReader ，因为底层会自动的去关闭节点流
>         //FileReader.
>         bufferedReader.close();
>       /**
>       * public void close() throws IOException {
>         synchronized (lock) {
>             if (in == null)
>                 return;
>             try {
>                 in.close();
>             } finally {
>                 in = null;
>                 cb = null;
>             }
>         }
>     }
>       */
>     }
> }
> ```
>
> 2. 使用BufferedWriter 将 "hello，韩顺平教育"，写入到文件中 
>
> ```java
> class BufferedWriter_test{
>     public static void main(String[] args) throws Exception{
>         String filePath = "/Users/yannlau/Downloads/file2.txt";
>         //创建BufferedWriter
>         //说明
>         // 1.new FileWriter（filePath,true） 表示以追加的方式写入
>         // 2. new FileWriter（filePath），表示以覆盖的方式写入
>         //想去追加就在new FileWriter(filePath,true) 加上true
>         BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath));
>         bufferedWriter.write("hello,lyp!呼呼!");
>         //插入一个和系统相关的换行符
>         bufferedWriter.newLine();
>         bufferedWriter.write("扭扭捏捏袅袅娜娜");
>         //关闭外层流即可
>         //传入的new FileWriter（filePath），会在底层关闭
>         bufferedWriter.close();
>     }
> }
> ```
>
> 3. 综合使用 BufferedReader 和 BufferedWriter 完成 文本文件拷贝 <u>***注意文件编码***</u>
>
> ```java
> class copy_buffered {
>     public static void main(String[] args) {
> 
>         //1. BufferedReader 和 BufferedWriter 是安装字符操作
>         //2.不要去操作 二进制文件［声音，视频，doc，pdf etc.]，可能造成文件损坏
>         String srcFilePath = "/Users/yannlau/Downloads/file1.txt";
>         String destFilepath = "/Users/yannlau/Downloads/copyfile1.txt";
>         BufferedReader br = null;
>         BufferedWriter bw = null;
>         String line;
> 
>         try {
>             br = new BufferedReader(new FileReader(srcFilePath));
>             bw = new BufferedWriter(new FileWriter(destFilepath));
>             //说明：readLine 读取一行内容，但是没有换行
>             while ((line = br.readLine()) != null) {
>                 //每读取一行就写入
>                 bw.write(line);
>                 bw.newLine();//插入一个系统相关的换行符
>             }
>           	System.out.println("拷贝完毕~");
>         } catch (IOException e) {
>             throw new RuntimeException(e);
>         } finally {
>             //关闭文件
>             if (br != null) {
>                 try {
>                     br.close();
>                 } catch (IOException e) {
>                     throw new RuntimeException(e);
>                 }
>             }
>             if (bw != null) {
>                 try {
>                     bw.close();
>                 } catch (IOException e) {
>                     throw new RuntimeException(e);
>                 }
>             }
>         }
>     }
> }
> ```

## BufferedInputStream 和 BufferedOutputStream

BufferedInputStream 和 BufferedOutputStream 的 in和out 是放在其直接父类FilterInputStream和FilterOutputStream中的.

> BufferedInputStream是字节流 在创建 BufferedlnputStream时，会创建一个内部缓冲区数组.
>
> <img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240418下午44642529.png" alt="image-20240418下午44642529" style="zoom:50%;" />
>
> BufferedOutputStream 是字节流，实现缓冲的输出流，可以将多个字节写入底层输出流中，而不必对每次字节写入调用底层系统
>
> <img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240418下午44617949.png" alt="image-20240418下午44617949" style="zoom:50%;" />

```java
//使用他们，可以完成二进制文件考贝。
//思考：字节流可以操作二进制文件，可以操作文本文件吗？当然可以
class bufferedIOStream {
    public static void main(String[] args) {
        String srcFilePath = "/Users/yannlau/Downloads/Linux 系列汇总.pdf";
        String destFilepath = "/Users/yannlau/Downloads/copyPDF.pdf";

        BufferedInputStream bIS = null;
        BufferedOutputStream bOS = null;

        try {
            byte[] bytes = new byte[1024];
            int readLen = 0;
            bIS = new BufferedInputStream(new FileInputStream(srcFilePath));
            bOS = new BufferedOutputStream(new FileOutputStream(destFilepath));
            while ((readLen = bIS.read(bytes)) != -1) {
                bOS.write(bytes, 0, readLen);
            }

        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            if (bIS != null) {
                try {
                    bIS.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
            if (bOS != null) {
                try {
                    bOS.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
}
```



## 对象流 ObjectInputStream和ObjectOutputStream`

![image-20240218下午90446119](/Users/yannlau/Library/Application Support/typora-user-images/image-20240218下午90446119.png)

> 看一个需求
>
> 1. 将int num = 100这个 int 数据保存到文件中，注意不是100 数字，而是 int 100，并且，能够从文件中直接恢复 int 100
> 2. 将Dog dog = new Dog("小黄",3)  这个 dog对象 保存到 文件中，并且能够从文件恢复.
> 3. 上面的要求，就是 能够将 基本数据类型 或者 对象 进行 序列化 和 反序列化操作

> 序列化和反序列化
>
> 1. 序列化就是在保存数据时，保存数据的值和数据类型
> 2. 反序列化就是在恢复数据时，恢复数据的值和数据类型
> 3. 需要让某个对象支持序列化机制，则必须让其类是可序列化的，为了让某个类是可序列化的，该类必须实现如下两个接口之一：
>
> >Serializable // 这是一个标记接口,没有方法
> >Externalizable //该接口有方法需要实现，因此我们一般实现上面的

#### 对象流介绍

功能：提供了对基本类型或对象类型的序列化和反序列化的方法 

ObjectOutputStream 提供 序列化功能 

ObjectInputStream 提供 反序列化功能

![image-20240218下午91228111](/Users/yannlau/Library/Application Support/typora-user-images/image-20240218下午91228111.png)

![image-20240218下午91319262](/Users/yannlau/Library/Application Support/typora-user-images/image-20240218下午91319262.png)

> 序列化可能涉及到版本的问题,之前的保存的一个类的对象,在你对类进行修改后 , 哪怕是很小的改动比如增加一个方法 ,也会导致无法读取该类的对象!

> 而且读取对象和存入对象时,对象的包要是同一个例如 com.animal.Dog,
>
> 不能是一个是 com.animal.Dog 另一个是 com.live.dog , 哪怕这两个Dog类的内容完全一样.

<img src="/Users/yannlau/Documents/JavaSet/Java韩顺平/第1阶段_Java900P_韩顺平 + 个人理解积累补充/assets/image-20240418下午45725789.png" alt="image-20240418下午45725789" style="zoom:50%;" />

```java
//演示ObjectOutputStream的使用，完成数据的序列化
class ObjectOutputStream_Test {
    public static void main(String[] args) throws IOException {
        //序列化后，保存的文件格式，不是存文本，而是按照他的格式来保存
        String filePath = "/Users/yannlau/Downloads/data.txt";
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath));
        //序列化数据
        oos.writeInt(100); // int -> Integer(实现了Serializable);
        oos.writeBoolean(true); //boolean -> Boolean (实现了 Serializable)
        oos.writeChar('a'); // char -> Character (实现了 Serializable)
        oos.writeDouble(9.5);//double -> Double (实现了Serializable)
        oos.writeUTF("刘云鹏大帅比"); //String 实现了Serializable
        //保存一个Dog对象
        oos.writeObject(new Dog("大黄", 3,"China","yellow"));

        oos.close();
        System.out.println("数据保存完毕(序列化形式)");
    }
}

class ObjectInputStream_test {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        String filepath = "/Users/yannlau/Downloads/data.txt";
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filepath));
        //读取
        //1.读取（反序列化〕的顺序需要和你保存数据（序列化〕的顺序一致
        //2.否则汇报异常
        System.out.println(ois.readInt());
        System.out.println(ois.readBoolean());
        System.out.println(ois.readChar());
        System.out.println(ois.readDouble());
        System.out.println(ois.readUTF());
        Object dog = ois.readObject();
        System.out.println("运行类型是 " + dog.getClass());
        System.out.println("dog的信息 " + dog); //底层 Object -> Dog
        //这里是特别重要的细节：
        //1.如果我们希望调用Dog的方法，需要向下转型
        //2.需要我们将Dog类的定义，拷贝到可以引用的位置
        Dog doge = (Dog) dog;
        System.out.println(doge.getAge());

        //关闭外层流即可
        ois.close();
    }
}

class Dog implements Serializable {
    private String name;
    private int age;
    private static String nation;
    private transient String color;

    public Dog(String name, int age , String nation,String color) {
        this.name = name;
        this.age = age;
        this.color = color;
        this.nation = nation;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", color='" + color + '\'' +
                '}'+ nation;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

> 节点流和处理流
>
> ＞ 注意事项和细节说明
> 1）读写顺序要一致
> 2）要求实现序列化或反序列化对象，需要 实现 Serializable
> 3）序列化的类中建议添加SerialVersionUID，为了提高版本的兼容性
>
> `private static final long serialVersionID = 1L;`
>
> 4）序列化对象时，默认将里面所有属性都进行序列化，但除了static或transient（临时）修饰的成员
> 5）序列化对象时，要求里面属性的类型也需要实现序列化接口
> 6）序列化具备可继承性，也就是如果某类已经实现了序列化，则它的所有子类也已经默认实现了序列化

## 标准输入输出流

> |            | 类型        | 默认设备 |
> | ---------- | ----------- | -------- |
> | System.in  | InputStream | 键盘     |
> | System.out | PrintStream | 显示器   |

```java
System.in
	//System类的public final static InputStream in = null;
  //System.in 编译类型 InputStream
	//System.in 运行类型 BufferedInputStream
  //表示标准输入,就是键盘

System.out
  //1. System.out是public final static PrintStream out = null;
  //2．编译类型 PrintStream
  //3. 运行类型 PrintStream
  //4. 表示标准输出,就是显示器

```

## 转换流 InputStreamReader 和 OutputStreamWriter

> 文件乱码问题引入
>
> ```java
> package com.hspedu.transformation;
> import java.io.*;
> /**
> * @author 韩顺平
> * @version 1.0
> * 看一个中文乱码问题
> */
> public class CodeQuestion {
>   public static void main(String[] args) throws IOException {
>     //读取 e:\\a.txt 文件到程序
>     //思路
>     //1. 创建字符输入流 BufferedReader [处理流]
>     //2. 使用 BufferedReader 对象读取 a.txt
>     //3. 默认情况下，读取文件是按照 utf-8 编码
>     String filePath = "e:\\a.txt";
>     BufferedReader br = new BufferedReader(new FileReader(filePath));
>     String s = br.readLine();
>     System.out.println("读取到的内容: " + s);
>     br.close();
>     //InputStreamReader
>     //OutputStreamWriter
>   }
> }
> ```
>
> 

> >介绍
> 1. InputStreamReader: Reader的子类，可以将InputStream（字节流）包装成Reader（字符流）
> 2. OutputStreamWriter:Writer的子类，实现将OutputStream（字节流）包装成Writer（字符流）
> 3. 当处理纯文本数据时，如果使用字符流效率更高，并且可以有效解决中文问题，所以建议将字节流转换成字符流
> 4. 可以在使用时指定编码格式（比如utf-8,gbk,gb2312, ISO8859-1等）

![image-20240219上午100424422](/Users/yannlau/Library/Application Support/typora-user-images/image-20240219上午100424422.png)

![image-20240219上午100558030](/Users/yannlau/Library/Application Support/typora-user-images/image-20240219上午100558030.png)

> 1. 编程将 字节流FileInputStream包装成（转換成）字符流Inputstream Reader，对文件进行读取（按照 utf-8/gbk 格式），进而再包装成 BufferedReader

```java
class InputStreamReader_test {
    public static void main(String[] args) throws IOException {
        String filePath = "/Users/yannlau/Downloads/file3.txt";
        //1. new FileInputStream（filePath）转成 InputStreamReader
        //2. 指定编码格式 , 不指定默认utf-8
        InputStreamReader isr = new InputStreamReader(new FileInputStream(filePath),"gb18030");
        //3. 把InputStreamReader 传入 BufferedReader
        BufferedReader br = new BufferedReader(isr);
        //2和3可以合为一步
        //BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream(filePath),"gb18030"));
        //4.读取
        String s = br.readLine();
        System.out.println("读取内容为 " + s);
        br.close();
    }
}
```

> 编程将 字节流 FileOutputStream 包装成（转换成）字符流OutputStreamWriter，对文件进行写入（按照gbk格式，可以指定其他，比如utf-8）

```java
class OutputStreamWriter_test{
    public static void main(String[] args) throws IOException {
        String filePath = "/Users/yannlau/Downloads/lyp.txt";
        //1. new FileOutputStream（filePath）转成 OutputStreamWriter
        //2. 指定编码格式
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(filePath),"gb18030");
        //3. 把OutputStreamWriter 传入 BufferedWriter
        BufferedWriter bw = new BufferedWriter(osw);
        //2和3可以合为一步
        //BufferedWriter br = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(filePath),"gb18030"));
        //4.读取
        String s = "hi!刘云鹏是个大帅哥!";
        bw.write(s);
        bw.close();
    }
}
```

## 打印流-PrintStream 和 PrintWriter
> 应用实例，打印流只有输出流，没有输入流.

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240219上午112630979.png" alt="image-20240219上午112630979" style="zoom:50%;" />

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240219上午112705176.png" alt="image-20240219上午112705176" style="zoom:50%;" />

```java
class PrintStream_test{
    public static void main(String[] args) throws IOException {
        PrintStream out = System.out;
        //在默认情况下，PrintStream 输出数据的位置是 标准输出，即显示器
        out.println("john,hello");
        //因为print底层使用的是write，所以我们可以直接调用write进行打印/输出
        out.write("刘云鹏你好啊".getBytes());
        out.close();

        //我们可以去修改打印流的输出位置/设备
//        public static void setOut(PrintStream out) {
//           checkI0();
//           setOuto(out); // native 方法,修改了out
        System.setOut(new PrintStream("/Users/yannlau/Downloads/lyp123.txt"));
        System.out.println("我想自由飞翔!");
    }
}
```



<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240219上午112821767.png" alt="image-20240219上午112821767" style="zoom:50%;" />

<img src="/Users/yannlau/Library/Application Support/typora-user-images/image-20240219上午112802014.png" alt="image-20240219上午112802014" style="zoom:50%;" />

```java
class PrintWriter_test{
    public static void main(String[] args) throws IOException {
        PrintWriter printWriter = new PrintWriter(System.out);
        printWriter.println("john,hello");
        PrintWriter printWriter1 = new PrintWriter(new FileWriter("/Users/yannlau/Downloads/lyp123.txt"));
        printWriter1.print("hi,宝贝你好!~");
        printWriter.close();
        printWriter1.close();
    }
}
```

# Properties类

> 如下一个配置文件 mysql.properties
> ip=192.168.0.13
> user=root
> pwd=12345
> 请问编程读取 ip、user 和 pwd 的值是多少

> 传统方法
>
> ```java
> import java.io.BufferedReader;
> import java.io.FileReader;
> import java.io.IOException;
> 
> public class Properties_test {
>     public static void main(String[] args) throws IOException {
>         //读物mysql.properties文件,获取 ip user pwd
>         BufferedReader br = new BufferedReader(new FileReader("/Users/yannlau/IdeaProjects/busuan_HW01/Test/src/mysql.properties"));
>         String line = null;
>         while((line = br.readLine())!=null){
>             String[] split = line.split("=");
>             System.out.println(split[0] + "的值是: "+ split[1]);
>         }
>         br.close();
>     }
> }
> ```

> Properties 基本介绍
>
> 1） 专门用于读写配置文件的集合类
> 配置文件的格式：
> 键=值
> 键=值
> 2） 注意：键值对不需要有空格，值不需要用引号一起来。默认类型是String

> Properties 类的方法
> • load：加载配置文件的键值对到Properties对象
> • list：将数据显示到指定设备
> • getProperty（key）：根据键获取值
>
>    get(key);  得到object对象 (编译类型) ,  可以通过和 ""空字符串相加得到字符串. 因为其运行类型就是字符串.
>
> • setProperty（key,value）：设置键值对到Properties对象
> • store：将Properties中的键值对存储到配置文件，在idea 中，保存信息到配置文件，如果
> ***<u>含有中文，会存储为unicode码***</u>
>
> ```java
> class Properties_02{
>     public static void main(String[] args) throws IOException {
>         //1. 创建Properties类对象
>         Properties properties = new Properties();
>         //2. 加载指定的配置文件
>         String filePath = "/Users/yannlau/IdeaProjects/busuan_HW01/Test/src/mysql.properties";
>         properties.load(new FileReader(filePath));
>         //3. 把k-v显示控制台
>         properties.list(System.out);
>         //4. 根据key获取对应的值
>         System.out.println(properties.getProperty("user"));
>         System.out.println(properties.getProperty("pwd"));
>     }
> }
> 
> class Properties_03{
>     public static void main(String[] args) throws IOException {
>         //使用Properties 类来创建配置文件
>         Properties properties = new Properties();
>         //创建
>         //'1.如果该文件没有key 就是创建
>         //'2.如果该文件有key，就是修改
>         //Properties 父类是 Hashtable，底层就是Hashtable
>         properties.setProperty("charset","utf-8");
>         properties.setProperty("user","汤姆");
>         properties.setProperty("pwd","abc111");
>         //将k-v存储在文件中,后面的null可以替换为说明类似于git的commit -m
>         properties.store(new FileOutputStream("/Users/yannlau/IdeaProjects/busuan_HW01/Test/src/mysql.properties"),null);
>         System.out.println("保存配置文件成功!");
>     }
> }
> ```





# 为什么java 用字符流去读取复制二进制文件会导致文件损坏？

在Java中，字符流主要用于处理文本数据，而不是二进制数据。字符流在读取或写入数据时，会进行字符编码和解码，这适用于文本文件，但不适用于二进制文件。

当你使用字符流来读取二进制文件时，可能会导致一些字节的丢失或错误的解释，因为字符编码通常是基于文本字符集的。二进制文件中的某些字节可能不是有效的文本字符，因此在字符流中读取时可能会导致解码错误。

为了正确处理二进制文件，应该使用字节流而不是字符流。字节流不进行字符编码和解码，它们直接操作原始的字节数据。对于文件复制等操作，使用`FileInputStream`和`FileOutputStream`等字节流类是更合适的选择。

示例代码：

```java
javaCopy code
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileCopyExample {
    public static void main(String[] args) {
        String sourceFilePath = "source.bin";
        String destinationFilePath = "destination.bin";

        try (FileInputStream fis = new FileInputStream(sourceFilePath);
             FileOutputStream fos = new FileOutputStream(destinationFilePath)) {

            byte[] buffer = new byte[4096];
            int bytesRead;

            while ((bytesRead = fis.read(buffer)) != -1) {
                fos.write(buffer, 0, bytesRead);
            }

            System.out.println("File copied successfully.");

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

此示例使用`FileInputStream`和`FileOutputStream`来进行字节流的文件复制，避免了字符编码和解码的问题，适用于二进制文件的正确处理。

# Java编码UTF-16问题

```java
public class test1 {
    public static void main(String[] args) throws IOException {
        File file = new File("/Users/yannlau/Desktop/test.txt");
        String hello = "你好Hello😁你好"; //笑脸对应四个字节
        FileWriter fileWriter = null;
        try {
            fileWriter = new FileWriter(file);
            char[] chars = hello.toCharArray(); //笑脸其实对应的是char数组中的[7]和[8]
            fileWriter.write(chars, 7, 2); //test.txt中会出现笑脸
          	fileWriter.write(chars,7,1);//test.txt中会出现？乱码
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            if (fileWriter != null) {
                fileWriter.close();
            }
        }
    }
}
```

# FileWrite、FileReader如何确定文件的字符编码格式？

默认是utf-8，如果出现乱码可以通过转换流 InputStreamReader(inputstream,charaacter-set)转换。

# 系统相关的换行符的影响

不同的操作系统使用不同的换行符来表示文本中的换行。主要的区别在于：

1. Windows：使用回车符（Carriage Return，CR，\r）和换行符（Line Feed，LF，\n），组合在一起表示换行（\r\n）。
2. Unix/Linux：使用换行符（LF，\n）表示换行。
3. macOS（早期版本）：使用回车符（CR，\r）表示换行。

在跨平台的文件处理中，有时候这些换行符的差异会导致问题，因此在处理文本时需要注意。

那么java的BufferedReader、BufferedWriter如何处理不同系统文件的换行符呢？

ChatGPT

在Java中，`BufferedReader`类会根据当前系统的换行符自动处理不同系统文件的换行符。它会将读取的文本文件中的换行符转换为Java中的标准换行符表示形式，即 `\n`。

这意味着，无论文件是在Windows、Unix/Linux还是macOS中创建的，`BufferedReader`都会将其视为使用 `\n` 来表示换行。因此，你在使用 `BufferedReader` 读取文件时，不需要担心不同系统换行符的差异。

`BufferedWriter`在写入文本时，会根据当前系统的换行符来处理换行符。它会自动将Java中的标准换行符 `\n` 转换为当前系统的换行符表示形式，并写入文件。

例如，在Windows系统上，`BufferedWriter`会将 `\n` 转换为回车符（`\r`）和换行符（`\n`）的组合（`\r\n`），而在Unix/Linux系统上，它会将 `\n` 转换为单独的换行符（`\n`）。

这样，通过 `BufferedWriter` 写入的文本文件在不同系统之间也会得到正确的换行符表示形式，从而保持跨平台的兼容性。
