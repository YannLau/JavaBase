[toc]

# 操作文件

Path和Files类封装了在用户机器上处理文件系统所需要的所有功能。

IO流关注的是文件的内容，而文件类关心的则是文件在磁盘上的存储。

Path和Files类是在Java7中新加入的。用起来比JDK1.0就使用的File类要方便的多。我们认为这两个类在Java程序员中会流行起来的。

# Path和Paths

可以创建绝对路径：`Paths.get("/home","harry");`得到的是“/home/harry”。

也可以创建相对路径：`Paths.get("myprog","conf","user.properties");`

静态的Paths.get方法接受一个或者多个字符串，并将它们用默认文件系统的路径分隔符连接起来。然后解析连接起来的结果，如果其表示的不是给定文件系统中的合法路径，那么就抛出一个InvalidPathException异常。这个链接起来的结果就是一个Path对象。

Paths.get方法也可以获取包含多个部件的单个字符串。

`Paths.get("/User/you/desktop");`

> 路径不必对应着某个实际存在的文件，它仅仅是一个抽象的名字序列。在接下来的小节中将会看到，当你想要创建文件时，首先要创建一个路径，然后才调用方法去创建对应的文件。