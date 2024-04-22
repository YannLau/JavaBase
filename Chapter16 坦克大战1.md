P569--P579

[toc]

> 用现有知识写出坦克大战1.0版本
>
> 加入线程知识得到2.0版
>
> 加入IO流得到3.0版本

# 坦克大战游戏演示

# java绘图坐标体系

> 坐标体系 - 像素
>
> 1. 绘图还必须要搞清一个非常重要的概念-像素 一个像素等于多少厘米？
> 2. 计算机在屏幕上显示的内容都是由屏幕上的每一个像素组成的。例如，计算机显示器的分辨率是800x600，表示计算机屏幕上的每一行由800个点组成，共有600行，整个计算机屏幕共有480 000个像素。<u>***像素是一个密度单位，而厘米是长度单位，两者无法比较***</u>

# java绘图技术

```java
public class DrawCircle extends JFrame{ //JFrame对应一个窗口,可以理解为是一个画框
  //定义一个面板
  private Mypanel mp = null;
  public static void main(String[] args){
    new DrawCircle();
  }
  public DrawCircle(){ //构造器
    //初始化面板
    mp = new MyPanel();
    //把面板放入到窗口(画框)
    this.add(mp);
    //设置窗口的大小
    this.setSize(400,300);
    //当点击窗口的小×，程序完全退出
    this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    this.setVisible(true);//可以显示
  }
}

//1.先定义一个Mypanel继承Jpanel类,画图形,就在面板上画
class MyPanel extends JPanel {
  
  //说明：
	//1. MyPanel 对象就是一个画板
	//2. Graphics g 把 g 理解成一支画笔
	//3. Graphics 提供了很多绘图的方法
	//Graphics g
  
  //运行发现,paint方法确实被调用了,但是是被谁调用了呢?
  @override
  public void paint(Graphics g) {//绘图方法
    super.paint(g);  //调用父类的方法完成初始化,不能动
    //画一个圆形
    g.drawOval(10,10,100,100);
  }
}
```

> - 绘图原理
>
> 当组件第一次在屏幕显示的时候，程序会自动的调用paint())方法来绘制组件。
> 在以下情况paintO将会被调用：
>
> 1. 窗口最小化，再最大化
> 2. 窗口的大小发生变化
> 3. repaint函数被调用
>
> Component类提供了两个和绘图相关最重要的方法：
> 1. paint（Graphics g）绘制组件的外观
> 2. repaint（刷新组件的外观。)

> Graphics类
>
> ```java
> Graphics类你可以理解就是画笔，为我们提供了各种绘制图形的方法：
> 1.画直线 drawLine(int x1, int y1,int x2,int y2)
> 2.画矩形边框 drawRect(int x, int y, int width, int heigh)
> 3.画椭圆边框 drawOval(int x, int y. int width, int height)
> 4. 填充矩形 fillRect(int x, int y. int width, int height)
>   	//设置画笔的颜色
> 		g.setColor(Color.blue);
> 		g.fillRect(10, 10, 100, 100);
> 5. 填充椭园 fillOval(int x, int y, int width, int height)
> 6. 画图片 drawlmage(Image img, int x, int y,..)
>   1/1.获取图片资源，/bg.png 表示在该项目的根目录去获取bg.png 图片资源
> 	Image image = Toolkit.getDefaultToolkit().getImage(Panel.class.getResource("/bg.png"));
> 	g.drawImage(image, 10, 10, 175, 221, this);
> 7. 画字符串 drawString (String str, int x, int y)
>   //给画笔设置颜色和字体
> 		g.setColor(Color.red)；
> 		g.setFont(new Font("隶书",Font.BOLD, 50));
> 		g.drawString("北京你好"，100,100);
> 8.设置画笔的字体 setFont(Font font)
> 9.设置画笔的颜色 setColor(Color c)
> ```

> 绘制坦克游戏区域
>
> P573

> 绘制坦克
>
> 坦克的坐标位置其实在坦克的左上角



# java事件处理机制

java事件处理是采取"委派事件模型"。

当事件发生时,产生事件的对象,会把此"信息”传递给"事件的监听者”处理，这里所说的"信息"实际上就是 java.awt.event 事件类库里某个类所创建的对象，把它称为"事件的对象"。![image-20240214下午53418930](/Users/yannlau/Library/Application Support/typora-user-images/image-20240214下午53418930.png)

> 时间处理机制深入理解
>
> 1. 前面我们提到几个重要的概念 事件源，事件，事件监听器我们下面来全面的介绍它们。
> 2. 事件源: 事件源是一个产生事件的对象，比如按钮，窗口等。
> 3. 事件:事件就是承载事件源状态改变时的对象，比如当键盘事件、鼠标事件、窗口事件等等，会生成一个事件对象，该对象保存着当前事件很多信息，比如KeyEvent 对象含有被按下键的Code值 . java.awt.event包, 和javax.swing.event包中定义了各种事件类型
> 4. 事件类型:查阅JDK文档
> 5. 事件监听器接口
>    1. 当事件源产生一个事件，可以传送给事件监听者处理
>    2. 事件监听者实际上就是一个类，该类实现了某个事件监听器接口比如前面我们案例中的MyPanle就是一个类，它实现了KeyListener接口，它就可以作为一个事件监听者，对接受到的事件进行处理
>    3. 事件监听器接口有多种，不同的事件监听器接口可以监听不同的事件,一个类可以实现多个监听接口
>    4. 这些接口在java.awt.event包和javax.swing.event包中定义列出常用的事件监听器接口，查看idk 文档聚集了



# 坦克大战游戏（1.0版）