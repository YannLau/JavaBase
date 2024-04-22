P499---P553

[toc]

# 集合的理解和作用

前面我们保存多个数据使用的是数组，那么数组有不足的地方，我们分析一下。

> 数组
> 1） 长度开始时必须指定，而且一旦指定，不能更改
> 2） 保存的必须为同一类型的元素
> 3）使用数组进行增加元素的示意代码 - 比较麻烦
>
> 写出Person数组扩容示意代码。
>
> Person[] pers = new Person[1];
> pers[0] = new Person;
> //增加新的Person对象？
>
> Person[] pers2 = new Person[pers.length+1]：//新创建数组
> for(){}  //拷贝pers数组的元素到pers2
> pers2[pers2.length-1]=new Person（）：//添加新的对象

> 集合
>
> 1）可以<u>***动态保存***</u>任意多个对象，使用比较方便！
> 2）提供了一系列方便的操作对象的方法：add、remove、set、get等
> 3） 使用集合添加，删除新元素的示意代码- 简洁了

# 集合的框架体系

- Collection -- 单列集合<单个值>
  - List
    - ArrayList
    - LinkedList
    - Vector
  - Set
    - HashSet
    - TreeSet
- Map--双列集合<键值对>
  - HashMap
  - TreeMap
  - HasTable
  - Properties

# Collection 接口和常用方法

## Collection接口实现子类的特点

- Collection接口实现类的特点
- public interface Collection<E> extends Iterable<E>
  - collection实现子类可以存放多个元素，每个元素可以是Object
  - 有些Collection的实现类，可以存放重复的元素，有些不可以
  - 有些Collection的实现类，有些是有序的（List），有些不是有序的（Set）
  - Collection接口没有直接的实现子类，是通过它的子接口Set 和 List 来实现的

> Collection常见方法  以ArrayList为例
>
> ```java
> List list = new ArrayList();
> 
> add：添加单个元素;
> list.add("jack");
> list.add(10);//list.add(new Integer(10))
> list.add (true);
> 
> remove：删除指定元素;
> list.remove(0);//删除第一个元素
> list.remove(true);//指定删除某个元素
>   
> contains：查找元素是否存在;
> list.contains ("jack");//true
>   
> size：获取元素个数
>   list.size();
>   
> isEmpty：判断是否为空
>   list.isEmpty();
>   
> Clear：清空
>   list.clear();
>   
> addALL：添加多个元素
>   list.addAll(另一个list);
>   
> containsALL：查找多个元素是否都存在
>   list.contains(另一个list);
>   
> removeAll：删除多个元素
>   list.removeAll(另一个list);
>   
> ```

> Clooection 接口遍历元素方式1---使用 Iterator 迭代器
>
> 1） Iterator对象称为迭代器，主要用于遍历 Collection 集合中的元素。
> 2） 所有实现了Collection接口的集合类都有一个iterator（方法，用以返回一个实现了Iterator接口的对象，即可以返回一个迭代器。
> 3） Iterator 的结构.［图：］
> 4） Iterator 仅用于遍历集合，Iterator 本身并不存放对象。

> 迭代器执行原理
>
> > Iterator iterator = coll.iterator(); ： //得到一个集合的迭代器
> > //hasNext();  ：判断是否还有下一个元素
> > while(iterator.hasNext()){
> > 	//next() ：①指针下移②将下移以后集合位置上的元素返回
> > 	System.out.println(iterator.next());
> > }
>
> Iterator接口的方法
>
> hasNext()
>
> next()
>
> remove()
>
> 在调用iterator.next() 方法之前必须要调用iterator.hasNext()
> 进行检测。若不调用，且下一条记录无效，直接调用it.next() 会抛出NoSuchElementException异常。
>
> - `快捷键 itit 快速生成迭代器while循环输出`
>
> + `Command/Ctrl + J 提示所有的快捷模版`
>
> Java的迭代器和Python的类似, 退出循环后,迭代器指针位置不变.
>
> 如果想再次使用,需要重置迭代器.  `iterator = col.iterator();`

> Clooection 接口遍历元素方式2---for循环增强
>
> 增强for循环，可以代替iterator选代器，特点：增强for就是简化版的iterator
>
> 本质一样。只能用于遍历集合或数组。
> ＞基本语法
> `for（元素类型 元素名：集合名或数组名）｛`
> 	`访问元素`
> `｝`
>
> 用于Collection的遍历,也可以用于数组遍历.
>
> <u>***增强for循环遍历Collection在底层仍然是迭代器.***</u>
>
> 可以理解为就是简化版本的迭代器遍历.

# List接口的常用方法

> List接口基本介绍
>
> List 接口是 Collection 接口的子接口
>
> 1. List集合类中元素有序（即添加顺序和取出顺序一致）、且可重复
> 2. List集合中的每个元素都有其对应的顺序索引，即支持索引。索引从0开始
> 3. List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。
> 4. JDK API中List接口的常用实现类有：ArrayList. LinkedList和Vector.

> List接口常用方法
>
> List 集合里添加了一些根据索引来操作集合元素的方法
> 1) void add(int index, Object ele): 在index位置插入ele元素
> 2) boolean addAll(int index, Collection eles): 从索引index位置开始将eles中的所有元素添加进来.
> 3) Object get(int index)：获取指定index位置的元素
> 4) int indexOf(Object obj)：返回obj在集合中首次出现的位置
> 5) int lastlndexOf（Object obj）；返回obj在当前集合中末次出现的位置
> 6) Object remove(int index)：移除指定index位置的元素，并返回此元素
> 7) Object set(int index, Object ele) : 设置 指定index 位置的元素为ele,相当于是替换.
> 8) List subList（int fromlndex, int tolndex）：返回从fromlndex到tolndex-1位置的子集合, 遵从前闭后开,最后一个索引取不到的!

> List 的三种遍历方式
>
> `1） 方式一：使用iterator`
> `Iterator iter = col.iterator;`
> `while(iter.hasNext()){`
> `Object o = iter.next);`
> `｝`
> `2） 方式二：使用增强for`
> `for(Object o:col){`
> `｝`
> `3） 方式三：使用普通for`
> `for(int i=0;i< list.size;i+ +){`
> `Object object = list.get(i);`
> `System.out.println(object);`
> `｝`
>
> 说明：使用LinkedList 使用方式和 ArrayList 一样

> ArrayList 注意事项
>
> 1）<u>***permits all elements, including null***</u> , ArrayList 可以加入null，并且多个
> 2） ArrayList 是由数组来实现数据存储的
> 3） ArrayList 基本等同于Vector，除了 ArrayList 是线程不安全（执行效率高）看源码.  在多线程情况下，不建议使用ArrayList

# ArrayList的底层操作机制源码分析（重点，难点）

> 1） ArrayList中维护了一个Object类型的数组elementData. 
> `transient Object[] elementData;`
>
> transient 关键字 表示短暂的暂时的,表示该属性不会被序列化. (IO流中的知识)
>
> 2） 当创建对象时，如果使用的是无参构造器，则初始elementData容量为0 (jdk7분10)
>
> 3） 当添加元素时：先判断是否需要扩容，如果需要扩容，则调用grow方法，否则直接添加元素到合适位置
>
> 4）如果使用的是无参构造器，如果第一次添加，需要扩容的话，则扩容elementData为10，如果需要再次扩容的话，则扩容elementData为1.5倍。
>
> 5） 如果使用的是指定容量capacity的构造器，则初始elementData容量为capacity
>
> 6）如果使用的是指定容量capacity的构造器，如果需要扩容，则直接扩容elementData为1.5倍。

![image-20240209下午15729799](/Users/yannlau/Library/Application Support/typora-user-images/image-20240209下午15729799.png)

去掉勾选才可以看到 Collection类的完整数组,包含后面扩容后还未使用的null.

# Vector底层结构和源码剖析

> 1. Vector 类的定义说明
>
>    public class Vector<E>
>    extends AbstractList<E>
>    implements List<E>, RandomAccess, Cloneable, Serializable
>
> 2.  Vector底层也是一个对象数组，protected Object[] elementData;
>
> 3.  Vector 是线程同步的，即线程安全，Vector类的操作方法带有 synchronized
>
>    ```java
>    public synchronized E get(int index) {
>    if (index > = elementCount)
>    	throw new ArrayIndexOutOfBoundsException(index);
>    return elementData（index）：
>    }
>    ```
>
> 4. 在开发中，需要线程同步安全时，考虑使用Vector

> Vector 和 ArrayList 比较

|           | 底层结构 | 版本   | 线程安全(同步) 效率 | 扩容倍数                                                     |
| --------- | -------- | ------ | ------------------- | ------------------------------------------------------------ |
| ArrayList | 可变数组 | Jdk1.2 | 不安全,效率高       | 如果有参构造1.5倍, 如果无参第一次10,从第二次开始1.5倍        |
| Vector    | 可变数组 | Jdk1.0 | 安全,效率不高       | 如果是无参,默认10,满后,就按两倍扩容,如果指定大小,则每次直接按2倍扩容 |

new Vector(x,y); x指定初始大小,y指定每次扩容的大小

面试可能会问 线程安全Collection的扩容机制

# LinkedList底层结构

> LinkedList的全面说明
> 1） LinkedList实现了双向链表和双端队列特点
> 2）可以添加任意元素（元素可以重复），包括null
> 3）<u>***线程不安全，没有实现同步***</u>

> LinkedList 底层操作机制
>
> 1. LinkedList底层维护了一个双向链表
> 2. LinkedList中维护了两个属性first和last分别指向 首节点和尾节点
> 3. 每个节点（Node对象），里面又维护了prev、next、 item三个属性，其中通过prev指向前一个，通过next指向后一个节点。最终实现双向链表.
> 4. 所以LinkedList的元素的<u>***添加和删除***</u>，不是通过数组完成的，相对来说效率较高。

> LinkedList 方法
>
> `remove();` // 默认删除第一个元素
>
> 其他方法和迭代器 跟前面的ArrayList一样

# ArrayList和LinkedList比较

|            | 底层结构 | 增删的效率        | 改查的效率 |
| ---------- | -------- | ----------------- | ---------- |
| ArrayList  | 可变数组 | 较低,数组扩容     | 较高       |
| LinkedList | 双向链表 | 较高,通过链表追加 | 较低       |

> 如何选择ArrayList 和 LinkedList:
> 1） 如果我们改查的操作多，选择ArrayList
> 2） 如果我们增删的操作多，选择LinkedList
> 3）一般来说，在程序中，80%-90%都是查询，因此大部分情况下会选择ArrayList
> 4） 在一个项目中，根据业务灵活选择，也可能这样，一个模块使用的是ArrayList，另外一个模块是LinkedList.

# Set接口的方法

> Set 接口基本介绍
>
> 1. 无序（添加和取出的顺序不一致），没有索引
> 2. 不允许重复元素，所以最多包含一个null,可以多次添加,但是set中不会多
> 3. JDK API中Set接口的常见实现类有：HashSet和TreeSet

> 常用方法
>
> 1. Set接口的常用方法
>    1. 和List接口一样,Set接口也是Collection的子接口，因此，常用方法和Collection接囗一样.
> 2. Set接口的遍历方式
>    1. 同Collection的遍历方式一样，因为Set接口是Collection接口的子接口
>       1. 可以使用迭代器
>       2. 增强for
>       3. 不能使用索引的方式来获取

> 解读
>
> 1. 以Set 接口的实现类 HashSet 来讲解Set 接口的方法
>
> 2. set 接口的实现类的对象(Set接口对象)，不能存放重复的元素，因此最多set中只存在一个null
> 2. set 接口对象存放数据是无序(即添加的顺序和取出的顺序不一致)
> 2. 注意:取出的顺序的顺序虽然不是添加的顺序，但是他是固定的。
> 5. 遍历方式
>    1. 迭代器
>    2. 增强for
>    3. 不能用get(index)的普通循环方法了!

# HashSet

> HashSet全面说明
>
> 1. HashSet实现了Set接口
> 2. HashSet实际上是HashMap!
>    `public Hashset(){map = new HashMap<>();`
> 3. 可以存放nul值，但是只能有一个null
> 4. HashSet不保证元素是有序的,取决于hash后，再确定索引的结果
> 5. 不能有重复元素/对象.在前面 Set 接口使用已经讲过

> add();  方法
>
> 1.在执行add方法后，会返回一个boolean值
>
> 2.如果添加成功，返回 true，否则返回false
>
> 如果添加了重复元素会返回False

> 经典面试题
>
> set.add (new String("hsp"));//ok
> set.add（new String（"hsp"））；//加入不了.
>
> 看源码才能解决!

> HashSet 扩容机制
>
> 1. HashSet 底层是 HashMap
>    yannLau解释: HashSet里数组类型的Node[],而Node是是在HashMap中定义的. 是存储一对键值对的结点类.
> 2. 添加一个元素时，先得到hash值(  <u>***hash()***</u>  )->会转成->索引值
> 3. 找到存储数据表table，看这个索引位置是否已经存放的有元素
> 4. 如果没有，直接加入
> 5. 如果有，<u>***调用 equals 比较***</u>，如果相同，就放弃添加，如果不相同，则添加到最后, 注意 这里的 equals 可以是程序员重写规定的.
> 6. 在 Java8 中，如果一条链表的元素个数达到TREEIFY_THRESHOLD（默认是8），并且table的大小 >= MIN_TREEIFY_CAPACITY（默认64），就会进行树化（红黑树）!
> 7. 所以 hash一样必然不会存入, hash不一样,但是运气不好都放入同一个索引处, 经比较 equals一样, 也不会存入.

> HashSet 源码解读
>
> 1. 执行构造器 HashSet(){Map = new HashMap();}
>
> 2. 执行add()
>
>    ```java
>    set.add("java");
>    
>    public boolean add(E e) {//e = "java"
>    	return map.put(e,PRESENT)==null;
>      //PRESENT = static final Object PRESENT = new Object();
>    }
>    ```
>
> 3. 执行put(); 该方法会执行 hash(key) 得到key对应的hash值 算法 `(h = key.hashCode()) ^ (h >>> 16)`  为了尽量不发生碰撞
>
>    ```java
>    public V put(K key, V value) {
>      //key = "java" value = FRESENT
>    	return putVal(hash(key), key, value,false, true);
>    }
>    ```
>
> 4. 执行 putVal(); 方法
>
>    该方法的作用是 判断size,调整size,resize(),判断重复,new Node放入Node[] ,是否转成红黑树

> HashSet 扩容细节
>
> 1. HashSet底层是HashMap，第一次添加时，table 数组扩容到16，临界值（threshold）是16*加载因子（loadFactor）是0.75 = 12
> 2. 如果table 数组使用到了临界值 12.就会扩容到 16*2 = 32， 新的临界值就是32*0.75 = 24，依次类推
> 3. 在Java8中，如果一条链表的元素个数到达 TREEIFY_THRESHOLD（默认是8），并且table的大小 >=MIN_TREEIFY_CAPACITY（默认64），就会进行树化（红黑树），否则仍然采用数组扩容机制

两个对象的hashcode一样,一定加不进去,内容equals一样,但hashcode不一样,分两种情况:

1. 分到不同的index,可以加入
2. 恰好分到一样index发生hash碰撞, 加不进去
3. 因此要求部分内容相同就视为同一元素的话,需要重写hashcode方法和equals方法

`Objects.hash(Object x......可变参数);  根据x和y生成 hashcode`

# LinkedHashSet

> LinkedHashSet的全面说明
>
> 1. LinkedHashSet 是 HashSet 的子类
> 2.  LinkedHashSet 底层是一个 LinkedHashMap，底层维护了一个 数组+双向链表
> 3. LinkedHashSet 根据元素的 hashCode 值来决定元素的存储位置，同时使用链表维护元素的次序，这使得元素看起来是以插入顺序保存的。
> 4. LinkedHashSet 不允许添重复元素

> LinkedHashSet 源码说明
>
> 1. 在LinkedHastSet 中维护了一个hash表和双向链表
>    （ LinkedHashSet 有 head 和 tail）
> 2. 每一个节点有 before 和 after 属性，这样可以形成双向链表
> 3. 在添加一个元素时，先求hash值，在求索引，确定该元素在table的位置，然后将添加的元素加入到双向链表（如果已经存在，不添加［原则和hashset一样］）
>    tail.next = newElement // 示意代码
>    newElement.pre = tail
>    tail = newEelment;
> 4. 这样的话，我们遍历LinkedHashSet 也能确保插入顺序和遍历顺序一致

> LinkedHashSet 细节
>
> 老韩解读
> 1. LinkedHashSet 加入顺序和取出元素/数据的顺序一致
>
> 2. LinkedHashSet 底层维护的是一个LinkedHashMap（是HashMap的子类）
>
> 3. LinkedHashSet底层结构（数组table+双向链表）
>
> 4. 添加第一次时，直接将 数组table 扩容到 16，存放的结点类型是 LinkedHashMap\$Entry
>
> 5. 数组是 HashMap\$Node[ ] 存放的元素/数据是 LinkedHashMap$Entrvy类型 ( 和Map\$Entry不同!!!)
>
>   ```java
>   static class Entry<K,V> extends HashMap.Node<K,V>{
>   	Entry<K, V> before, after;
>   	Entry(int hash, K key, V value, Node<K,V> next){
>   		super(hash, key, value, next);
>     }
>   }
>   //其在Node的基础上加上了before 和 after
>   //LinkedHashSet的add方法,是在HashMap的基础上增加了 after 和 before的修改,其他逻辑和HashMap一致
>   //具体实现是在HashMap的add方法中有一个newNode方法,而LinkedHashSet重写了该方法,根据动态绑定,会走LinkedHashSet的newNode方法,其返回的是Entry类型的结点,并且对after和before做了判断修改!
>   ```

# Map接口和常用方法

> Map接口实现类的特点［很实用］
> 注意：这里讲的是JDK8的Map接口特点
>
> 1. Map与Collection并列存在。用于保存具有映射关系的数据：Key-Value
>
> 2. Map 中的key 和 value 可以是任何引用类型的数据，会封装到HashMap$Node对象中
>
> 3. Map 中的key 不允许重复，原因和HashSet 一样，前面分析过源码.
>
> 4. Map 中的 value 可以重复
>
> 5. Map 的key 可以为 null, value 也可以为null, 注意 key 为null，只能有一个，value 为null.可以多个.
>
> 6. 常用String类作为Map的key
>
> 7. key 和 value 之间存在单向一对一关系，即通过指定的key 总能找到对应的 value
>
>    get(key) 得到对应的 value
>
> 8. Map存放数据的key-value示意图，一对k-v 是放在一个Node中的，又因为Node 实现了Entry 接口，有些书上也说 一对k-y就是一个Entry

![image-20240210下午120538747](/Users/yannlau/Library/Application Support/typora-user-images/image-20240210下午120538747.png)

> 使用HashMap解读 Map
>
> 1. Map 接口实现类的特点，使用实现类HashMap
> 2. Map与Collection并列存在。用于保存具有映射关系的数据：Key-Value（双列元素）
> 3. Map 中的 key 和 value 可以是任何引用类型的数据，会封装到HashMap$Node 对象中
> 4. Map 中的 key 不允许重复，原因和HashSet 一样，前面分析过源码。
> 5. Map 中 Value可以重复
> 6. Map的key 可以为 null, value 也可以为nULL，注意 key 为null，只能有一个，value 为nULL，可以多个

> Map接口常用方法
>
> 1） put：添加
> 2） remove：根据键删除映射关系
> 3） get：根据键获取值
> 4） size：获取元素个数
> 5） isEmpty：判断个数是否为0
> 6） clear：清空
> 7） containsKey：查找键是否存在

> Map接口遍历方法
>
> 1） containsKey：查找键是否存在
> 2） keySet：获取所有的键
> 3） entrySet：获取所有关系
> 4） values：获取所有的值
>
> 1. 先取出 所有key,通过key取出对应的Value
>
>    ```java
>    Set keyset = map.keySet();
>    1.增强for
>      for (Object key : keyset){
>        Sout(key + "-" + map.get(key));
>      }
>    2.迭代器
>      Iterator iterator = keyset.iterator();
>    while(iterator.hasNext()){
>      Object next = iterator.next();
>    }
>    ```
>
> 2. 把所有的values取出
>
>    ```java
>    Collection values = map. values);
>    //这里可以使用所有的Collections使用的遍历方法
>    
>    1.增强for
>    2.iterator
>    ```
>
> 3. 通过EntrySet获得所有的 K-V
>
>    ```java
>    Set entrySet = map.entrySet();
>    1. 增强for
>      for(Object entry : entrySet){
>        //将entry 转成 Map.Entry
>        Map.Entry m = (Map.Entry) entry;
>        sout(m.getKey() + "-" + m.getValue());
>      }
>    2. Iterator iterator3 = entrySet.iterator();
>    while (iterator3.hasNext()) {
>    	Object next = iterator3.next();
>      Map.Entry m = (Map.Entry) entry;
>      sout(m.getKey() + "-" + m.getValue());
>    }
>    ```

# HashMap

> 1） Map接口的常用实现类：HashMap、Hashtable和Properties。
> 2） HashMap是 Map 接口使用频率最高的实现类。
> 3） HashMap 是以key-val 对的方式来存储数据.
> 4） key 不能重复，但是是值可以重复，允许使用null键和null值。
> 5） 如果添加相同的key，则会覆盖原来的key-val，等同于修改.（key不会替换，val会替换）
> 6） 与HashSet一样，不保证映射的顺序，因为底层是以hash表的方式来存储的.  JDK8的hashMap底层是 数组+链表+红黑树
> 7） <u>***HashMap没有实现同步，因此是线程不安全的***</u>

> HashMap源码剖析
>
> ＞扩容机制［和HashSet相同｝
> 1） HashMap底层维护了Node类型的数组table，默认为null
> 2） 当创建对象时，将加载因子（loadfactor）初始化为0.75.
> 3） 当添加key-val时，通过key的哈希值得到在table的索引。然后判断该索引处是否有元素，如果没有元素直接添加。如果该索引处有元素，继续判断该元素的key是否和准备加入的key相等，如果相等，则直接替换val；如果不相等需要判断是树结构还是链表结构，做出相应处理。如果添加时发现容量不够，则需要扩容。
> 4）第1次添加，则需要扩容table容量为16，临界值（threshold）为12.
> 5） 以后再扩容，则需要扩容table容量为原来的2倍，临界值为原来的2倍，即24，依次类推.
> 6）在Java8中，如果一条链表的元素个数超过 TREEIFY THRESHOLD（默认是8），并且
> table的大小 >= MIN_TREEIFY_CAPACITY（默认为64）.就会进行树化（红黑树）
> 7 ) 当红黑树节点少于8又会剪枝变回链表

# HashTable

![image-20240210下午42205609](/Users/yannlau/Library/Application Support/typora-user-images/image-20240210下午42205609.png)

> 基本介绍
>
> 1） 存放的元素是键值对：即K-V
> 2） hashtable的<u>***键和值都不能为null***</u>
> 3） hashTable 使用方法基本上和HashMap一样
> 4） <u>***hashTable 是线程安全的，hashMap 是线程不安全的***</u>

> Hashtable源码剖析
>
> 简单说明一下Hashtable的底层
>
> 1. 底层有数组 Hashtable$Entry[ ]初始化大小为 11   ,  `Hashtable\$Entry 继承了 Map\$Entry`
> 2. 临界值 > threshold 8 = 14 *0.75
> 3. 扩容：按照自己的扩容机制来进行即可。
> 4. 执行 方法 addEntry(hashi key,value,index);  添加K-V 封装到Entry
> 5. 当 if（count >= threshold）满足时，就进行扩容
> 6. 按照 int newCapacity = （oldCapacity << 1） + 1；的大小扩容.

> java 的 Hashtable扩容后，原来数组位置上的元素会重新计算index吗？位置是否发生改变？
>
> ChatGPT
>
> 在Java的 `Hashtable` 中，扩容发生时，原数组位置上的元素不会重新计算索引，而是会被重新散列到新的更大的数组中。在扩容时，`Hashtable`会创建一个新的数组，并将所有元素重新放置到新的数组中。
>
> `Hashtable` 的扩容过程如下：
>
> 1. 创建一个新的数组，其大小通常是原数组大小的两倍加一。
> 2. 将原数组中的元素重新计算哈希码并插入到新的数组中。
> 3. 新数组中的位置是通过重新计算哈希码和模运算来确定的，不会保留原数组中的位置关系。
>
> 由于新数组的大小增加了，重新计算哈希码和模运算可能会导致元素被分配到新的位置。这个过程确保了更好的散列效果，减少了哈希冲突的可能性。
>
> 总的来说，`Hashtable`扩容后，元素的位置会发生改变，而且新的位置是通过重新计算哈希码和模运算得到的。这也是为了保持较低的哈希冲突率，提高散列的效果。
>
> HashMap也会重新计算index插入.

> HashMap和Hashtable对比
>
> |           | 版本 | 线程安全(同步) | 效率 | 允许null键null值 |
> | --------- | ---- | -------------- | ---- | ---------------- |
> | HashMap   | 1.2  | 不安全         | 高   | 可以             |
> | HashTable | 1.0  | 安全           | 较低 | 不可以           |

# Properties

> 1. Properties类继承自Hashtable类并且实现了Map接口，也是使用一种键值对的形式来保存数据。
> 2. 他的使用特点和Hashtable类似, key和value不能为null
> 3. Properties 还可以用于 从 xxx.properties 文件中，加载数据到Properties类对象，并进行读取和修改
> 4. 说明：工作后 xxx.properties 文件通常作为配置文件，这个知识点在IO流举例，有兴趣可先看文章https://www.cnblogs.com/xudong-bupt/p/3758136.html

# 集合选择

总结-开发中如何选择集合实现类（记住）

在开发中，选择什么集合实现类，主要取决于业务操作特点，然后根据集合实现类特性进行选择，分析如下：

1. 先判断存储的类型（一组对象或一组键值对）
2. 一组对象：Collection接口 (一组对象[单列])
   1. 允许重复：List
      1. 增删多：LinkedList［底层维护了一个双向链表］
      2. 改查多：ArrayList ［底层维护 Object类型的可变数组］
   2. 不允许重复：Set
      1. 无序：HashSet［底层是HashMap，维护了一个哈希表 即（数组＋链表＋红黑树）］
      2. 排序：TreeSet
      3. 插入和取出顺序一致：LinkedHashSet(底层是linkedhashmap,linkedhashmap的底层又是HashMap)，维护数组+双向链表
3. 一组键值对：Map
   1. 键无序：HashMap［底层是：哈希表 jdk7：数组＋链表，jdk8：数组＋链表+红黑树］
   2. 键排序：TreeMap
   3. 键插入和取出顺序一致：LinkedHashMap
   4. 读取文件 Properties

# TreeSet

![image-20240210下午64944942](/Users/yannlau/Library/Application Support/typora-user-images/image-20240210下午64944942.png)

```java
1.当我们使用无参构造器，创建TreeSet时，仍然是无序的
2.老师希望添加的元素，按照字符串大小来排序
3.使用TreeSet 提供的一个构造器，可以传入一个比较器（匿名内部类）并指定排序规则
4．简单看看源码
  
1. 构造器把传入的匿名内部类(比较器对象)，赋给了 TreeSet的底层的TreeMap的属性this.comparator
  
public TreeMap(Comparator<? super K> comparator) {
		this.comparator = comparator;
}
//在 Comparator<? super K> 中，super K 表示比较器可以接受类型 K 或其父类型的对象。这就是 "super" 关键字的作用。因此，Comparator<? super K> 表示比较器可以比较 K 类型或其父类型的对象。
2. 在调用treeSet.add（"tom"），在底层会执行到
if (cpr != null) {
	do {
		parent = t;
		cmp = cpr.compare(key, t.key); //动态绑定到内部类的方法上
		if (cmp < 0)
			t = t.left;
		else if (cmp > 0)
			t = t.right;
		else //如果相等,返回0,该数据key加不进去
			return t. setValue(value);
	} while (t != null);
}
```

# Treemap

> 和Treeset用法差不多,只不过是键值对,按照key排序

# Collections工具类

> Collections工具类介绍
>
> 1. Collections 是一个操作 Set、 List 和 Map 等集合的工具类
> 2. Collections 中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作
>
> 排序操作：（均为static方法）
>
> 1. reverse（List）：反转 List 中元素的顺序
> 2. shuffle（List）：对 List 集合元素进行随机排序
> 3. sort（List）：根据元素的自然顺序对指定 List 集合元素按升序排序
> 4. sort（List, Comparator）：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
> 5. swap（List,int,int）：将指定 list 集合中的； i 处元素和 j 处元素进行交换
>
> 查找\替换
>
> 1. Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
> 2. Object max(Collection,Comparator）：根据 Comparator 指定的顺序返回给定集合中的最大元素
>
> 3) Object min(Collection)
> 4) Object min(Collection, Comparator)
> 5) int frequency（Collection, Object）：返回指定集合中指定元素的出现次数
> 6) void copy（List dest, List src）：将src中的内容复制到dest中
>   //为了完成一个完整拷贝，我们需要先给dest 赋值，大小和List.size()一样,要不然报错index越界
> 7) boolean replaceAlI（List list, Object oldVal, Object newVal）：使用新值替换 List 对象的所有旧值

![image-20240210下午104315506](/Users/yannlau/Library/Application Support/typora-user-images/image-20240210下午104315506.png)
