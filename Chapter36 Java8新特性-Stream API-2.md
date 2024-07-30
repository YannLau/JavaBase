[toc]

本小节来源于Up主-AlbertShen

【Java中的流、并行流 - Java Stream API | Parallel Streams】 https://www.bilibili.com/video/BV1Vi421C73n/?share_source=copy_web&vd_source=b77bfd0640ba59cb7c1bb8b3ad795165

# Stream

通过声明来declarative实现





## 动态创建流

```java
public class Main{
  public static void main(){
    Stream.Builder<String> streamBuilder = Stream.builder();//静态工厂方法
    streamBuilder.add("a");
    streamBuilder.add("b");
    if(Math.random() > 0.5){
      streamBuilder.add("c");
    }
    Sream<String> stream = streamBuilder.build();
    streamBuilder.add("d");//在流已经build的情况下，再次添加元素，会抛出illegalStateException异常
    stream.forEach(System.out::println);
  }
}
```

## IntSream

```json
public class Main{
  public static void main(){
    IntStream intStream = IntStream.of(1,2,3);
    IntStream intStream = IntStream.range(1,4); //不包含4
    IntStream intStream = IntStream.rangeClosed(1,4);//包含4
    new Random.ints(5).forEach(System.out::println);
    
    //转换为对象流
    Stream<Integer> integerStream = intStream.boxed();
  }
}
```

16:38 对各种流进行总结