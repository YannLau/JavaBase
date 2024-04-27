[toc]

操作文件

Path和Files类封装了在用户机器上处理文件系统所需要的所有功能。

IO流关注的是文件的内容，而文件类关心的则是文件在磁盘上的存储。

Path和Files类是在Java7中新加入的。用起来比JDK1.0就使用的File类要方便的多。我们认为这两个类在Java程序员中会流行起来的。

# Path和Paths

### Paths.get

可以创建绝对路径：`Paths.get("/home","harry");`得到的是“/home/harry”。

也可以创建相对路径：`Paths.get("myprog","conf","user.properties");`

静态的Paths.get方法接受一个或者多个字符串，并将它们用默认文件系统的路径分隔符连接起来。然后解析连接起来的结果，如果其表示的不是给定文件系统中的合法路径，那么就抛出一个InvalidPathException异常。这个链接起来的结果就是一个Path对象。

Paths.get方法也可以获取包含多个部件的单个字符串。

`Paths.get("/User/you/desktop");`

> 路径不必对应着某个实际存在的文件，它仅仅是一个抽象的名字序列。在接下来的小节中将会看到，当你想要创建文件时，首先要创建一个路径，然后才调用方法去创建对应的文件。

## 组合或解析路径

### Path.of（java11）可以代替Paths.get

### resolve

`p.resolve(q)`:如果q是绝对路径，则结果就是q。否则，根据文件系统的规则，将“p后面跟着q”作为结果。 ==> 翻译为人话，尝试将 p 和 q 拼接 ， 如果 q 很厉害，结果为q。

例如，假设你的应用系统需要查找相对于给定基目录的工作目录，其中基目录是从配置文件中读取的，就像前一个例子一样。
`Path workRelative = Paths.get("work");`
`Path workPath = basePath.resolve(workRelative);`
resolve 方法有一种快捷方式，它接受一个字符串而不是路径：
`Path workPath = basePath.resolve("work");`

### resolveSibling

他通过解析指定路径的父路径产生其兄弟路径。例如workPath是`/opt/myapp/work`,以下调用：

`Path tempPath = workPath.resolveSibling("temp");`

将创建`/opt/myapp/temp`

### relativize

是resolve的对立面。???

`'a/b'.relativize('/a/b/c/d')` = `/c/d`

`p.relativize(p.resolve(q)).equals(q)`

### normalize

将移除所有冗余的.和..部件（或者文件系统认为冗余的所有部件）。

例如规范化`/home/harry/../fred/./input.txt` 将产生 `/home/fred/input.txt`。

其实就是把..操作给执行了。==..表示上一级目录===

### toAbsoulutePath

将产生绝对路径,该绝对路径从根部件开始，例如/usr/bin

如果是相对路径，则拼接上当前工作路径 `System.getProperty("user.dir")`.

### getRoot getFileName getParent

Path p = Paths. get ("/home", "fred", "myprog properties");
Path parent = p. getParent(); // the path /home/fred
Path file = p.getFileName(); // the path myprog. properties
Path root = p. getRoot (); // the path /

如果是p是相对路径，则root=null。

### toFile/toPath

偶尔你可能需要与遗留系统的API交互，他么使用的是File类而不是Path接口。Path接口有一个toFile方法，而File类有一个toPath方法。

# 读写文件

Files类可以使得普通文件操作变得快捷。例如，可以用如下方式很容易地读取文件的所有内容。

## 读入文件的内容

byte[] Files.readAllBytes(Path path);

String Files.readString(path,charset);

List\<String> Files.readAllLines(path,charset);

## 写入文件

static Path Files.write(Path path,byte[] contents,OpenOption... options)

static Path Files.write(Path path,byte[] contents,Charset charset,OpenOption... options)

static Path write(Path path, String contents, Charset charset, OpenOption... options)

static Path write(Path path, Iterable‹? extends CharSequence> contents, OpenOption options)

static Path Files.writeString(path,content,charset);

以上这些简便方法适用于处理中等长度的文本文件，如果要处理的文件长度比较大，或者是二进制文件，那么还是应该使用所熟知的输入/输出流或者读入器/写出器。

以下便捷方法可以将你从处理FileInputStream，FileOutputStream，BufferedReader和BufferedWriter的繁复操作中解脱出来。

static InputStream newInputStream(Path path, OpenOption... options)

static OutputStream newOutputStream (Path path, OpenOption... options)

static BufferedReader newBufferedReader(Path path, Charset charset)

static BufferedWriter newBufferedWriter(Path path, Charset charset, OpenOption... options)

打开一个文件，用于读入或写出。

## 创建文件和目录

