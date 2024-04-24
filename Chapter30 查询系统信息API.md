System.getProperty("user.dir"); //获取执行 java 命令的工作目录的绝对路径路径.

警告：由于反斜杠字符在Java字符串中是转义字符，因此要确保在 Windows风格的路径名中使用`\\`（例如，`C:\\Windows\\win.ini`）。在 Windows 中，还可以使用单斜杠字符（C:/windous/win.ini），因为大部分 Windows文件处理的系统调用都会将斜杠解释成文件分隔符。但是，并不推荐这样做，因为 Windows 系统函数的行为会因与时俱进而发生变化。因此，对于可移植的程序来说，应该使用程序所运行平台的文件分隔符，我们可以通过常量字符串 `java.io.File.separator` 获得它.

macos中管理 java版本的命令行工具 `/usr/libexec/java_home -V`

		Runtime runtime = Runtime.getRuntime();
		//获取当前电脑的cpu数量
		int cpuNums = runtime.availableProcessors();
		System.out.println("当前有cpu 个数=" + cpuNums);

`System.lineSeparator() == System.getProperty("line.separator")` 获取系统对应的换行符 ==> "\n"

Toolkit.getDefaultToolkit().beep();发出蜂鸣声。

javax.swing.Timer定时器，配合ActionListener接口使用。