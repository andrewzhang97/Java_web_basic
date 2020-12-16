# Java-web
常见校招级面试知识点总结，不保证一定对，恳请大佬指正。 写下来只是为了帮助自己复习一边。

## java基础

### static
static 意思为静态 可以修饰属性、类和方法<br>
<br>
属性：static的属性在编译期完成加载(类初始化的链接过程之中) 存于方法区之中，可以被修改。
<br>
类：只有静态内部类可以修饰成为static，外部非static方法也可以创建对象（用的比较少）
<br>
方法: 调用static 方法不需要实例化对象，通过类名即可调用。保存在。 例如：
```
class A{
  public static void fun(){
    System.out.println("out");
  }
}

public class Main{
  public static void main(String[] args){
    A.fun();
  }
}
```
***

### final
同样可以用来修饰方法，类和变量<br>
变量：变量值一旦初始化便不可修改，一般是在初始化阶段或者编译阶段初始化完成。<br>
类：一旦初始化便不可进行继承，除非出于安全的考虑，一般不考虑设计为final类。<br>
方法：该方法不可被重载或重写。一般和private绑定。
***

### 多态、重载和重写

### 抽象类和接口的区别
抽象类：抽象类只是一种特殊的方法的类，只有声明，没有实现。里面可以存在非抽象方法。
```
abstract void fun();
abstract class father{
  abstract void func();
}
```
抽象类是public和protected中的一种，抽象类只能进行单继承，抽象类虽然不可实例化，但是如果main()也可调用其中的函数。<br>
抽象类更多的是对object的抽象化
***
接口：
```
interface eat{
  void fun();
}
``` 
接口没有方法体的方法，成员变量默认为public static final，一个类可以实现多个接口，接口的抽象级别更高. <br>
接口更多的是进行行为的规范。
***
### String
***
```
String s1="abc";
String s2=new String("abc");
```
s1和s2不同。 对于s1，“abc”是存在方法区内的字符串常量池之中，而s2 要先在堆（heap）创建一个为空的string对象。再去字符串常量池中新建一个“abc”对象，再指向字符串常量池中的对象。

```
String s3="abc";
String s4="edf";
String s5=s3+s4;
String s6="abcedf"
```
如果相加的都是静态字符串，例如s5，那么就去字符串常量池中创建一个，由于s5的过程的是在初始化阶段完成，而s6是在编译期完成，所以s5不等于s6。<br>
对于s3+s4的步骤，先是stringbuilder().append("abc").append("edf").toString()

```
str.intern();
```
str.intern() 去常量池中查找是否存在相同unicode的字符串，如果没有则创建返回常量池中的地址。
***
```
String StringBuffer Stringbuilder
```
String：不可变 final类且private final char value[],所有属性都是private final的
StringBuffer：可变类，线程安全 支持并发操作 但是代价会更高
StringBuilder：可变类 线程不安全 不支持并发
***

### Java的反射
反射是通过运行时的二进制字节码来修改类的属性或执行方法。<br>
反射的性能开销：平时我们会调用 Class.forName、Class.getMethod、以及 Method.invoke 这三个操作。其中，Class.forName 会调用本地方法，Class.getMethod 则会遍历该类的公有方法，如果没有匹配到，它还将遍历父类的公有方法，可想而知，这两个操作都非常耗时。下面就是 Method.invoke 调用本身的开销了，首先是 invoke 方法的参数是一个可变长参数，也就是构建一个 Object 数组存参数，这也同时带来了基本数据类型的装箱操作，在 invoke 内部会进行运行时权限检查，这也是一个损耗点。普通方法调用可能有一系列优化手段，比如方法内联、逃逸分析，而这又是反射调用所不能做的，性能差距再一次被放大。

### Java注解
存放在Java字节码中的一个Annotations 数组，类方法属性都可以加上注解，再通过反射来拿到这些信息。


### 类加载过程

1、从javac文件生成到二进制字节码文件.class 再将.class文件提交到JVM中执行。<br>
2、helloloader 引导器进行加载进内存。<br>
3、链接-验证 验证二进制字节码是否规范，防止影响JVM的安全。（包括文件格式验，符号引用的验证）。<br>
4、准备 对static类型的变量进行加载并分配内存设置初始值。<br>
5、解析 将符号引用替换为直接引用。<br>
6、初始化 变量初始赋值。只有对类需要主动使用时才需要类的初始化。 <br>
***
#### 双亲委派模型
bootstarpsLoader => extclassLoader => appclassLoader =>userclassLoader <br>
当一个类加载器收到了加载请求，先不去加载，而是去上面一层的父加载器完成。只有父加载器无法加载时，才会给到子加载器。

### JVM内存分区
线程私有：栈、本地方法区、程序计数器<br>
线程共有：堆、方法区
***
栈：会出现oom 可以自动扩充空间，当物理内存用尽则oom<br>
栈帧：一个方法对应一个栈帧，内部分为操作数栈、动态链接、局部变量表。<br>
操作数栈中间放着每次操作的汇编<br>
局部变量表是一组变量存储空间，存放方法内自定义的变量。内部存在slot，一个slot为32位<br>
动态链接：调用其他方法时所指向运行时常量池的符号引用，为了支持这个过程，这个存有这个引用<br>

***
堆：存在GC和OOM<br>
堆主要负责管理new 出来的对象，几乎所有的内存的对象都在堆之中，当然也会存在垃圾回收的情况

***
方法区：存在GC和OOM<br>
方法去主要存放.class的文件，类的元数据信息，运行时常量池，字段和方法。包括静态变量和常量。

***
程序计数器:当前所执行的Java方法的JVM地址指令
***
本地方法栈：调用本地原生的c++方法，对于每个线程创建一个。

### 垃圾回收
堆内存当中存在年代分区：分为新生代（Eden）和老年代（Old），只对Eden进行垃圾回收的叫做Minor GC，当Eden区满时进行。对于所有的堆内存进行垃圾回收叫做Full GC，当System.gc()运行时进行。
***
对于Eden区来说，分为From和To区，占比为8:1:1 最前面为Eden区。Eden主要保存快速创建并销毁的对象（在运行之中大部分的对象都是如此）JVM在分配的时候，为每个线程也会分配一个ThreadLocal Allocation Buffer。这一段是线程私有。
***
老年代：对于长生命周期对象，会放在在老年区，如果有大文件，则放在老年区来找到足够长的连续空间。
*** 
对象标记分为两种方法：可达性分析算法（GCROOT），引用计数法。<br>
GCroot的对象包括的虚拟机栈和本地方法栈中引用的对象、方法区中类静态属性引用的对象、方法区中常量引用的对象。<br>
引用计数法就是给对象添加一个引用计数器，每当有一个地方引用时就加一，引用失效时就减一。但是在JVM之中容易出现循环引用的问题，JVM并未采用。
***
对于Java来说，中间存在4中引用类型。强引用，软引用，弱引用，虚引用。当一个对象具有强引用，那么垃圾回收器不会回收他，即使会抛出oom错误。 软引用：当空间充足的时候不会垃圾回收，一旦进入到内存不足时，进行垃圾回收。 弱引用：有更短暂的生命周期，当垃圾回收时，不论出现什么情况都会回收他。不过垃圾回收器的线程优先级并不高，可能并不会发现他。 虚引用：并不决定对象的生命周期，在任何时候都可以被垃圾回收器回收。

#### 垃圾回收器
serial和parnew： 最简单的垃圾回收器。
***
CMS：1、初始标记，仅仅关联与GCRoot有关的对象，需要Stop the World，这一步速度快 2、并发标记，边标记边运行，把与第一步标记有关的对象进行标记出来。3、重新标记，这一步需要stw，修正在并发标记过程之中，新创建的gc对象。4、并发清除，这一步由于使用的是标记清除法，所以不需要stw，但是标记清除法会产生内存碎片。
***
G1:分为四类region，eden、survivor、old、humongous。h为大文件区。每个region存有rset和cset，rset存放引用对象，cset存放需要清理的对象。过程分为1、初始标记 2、并发标记 一边标记一边运行 3、最终标记 stw 4、并发清除 需要stw，复制算法，按照cset的优先级进行排序，每一次选择优先级更高的进行回收。
***
最近貌似出了个zgc jdk11（还没看） TODO

## Java容器

### map

#### hashmap、concurenthashmap、linkedhashmap、treemap、hashtable

hashmap本质是一个数组+链表+红黑树的结构。 数组中存放key值，链表中存有value。 每一个存储节点是一个Entry。
```
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable, Serializable {
    // 数组
    transient Node<K,V>[] table;    
	static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        // 链表
        Node<K,V> next;
```

```
 //默认hash桶初始长度16
  static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; 

  //hash表最大容量2的30次幂
  static final int MAXIMUM_CAPACITY = 1 << 30;

  //默认负载因子 0.75
  static final float DEFAULT_LOAD_FACTOR = 0.75f;

  //链表的数量大于等于8个并且桶的数量大于等于64时链表树化 
  static final int TREEIFY_THRESHOLD = 8;

  //hash表某个节点链表的数量小于等于6时树拆分
  static final int UNTREEIFY_THRESHOLD = 6;

  //树化时最小桶的数量
  static final int MIN_TREEIFY_CAPACITY = 64;
```
负载因子：0.75 当hashmap之中的元素个数大于capacity*loadfactor且新加入的元素的位置不为空，扩容成原来的两倍。<br>
查找时使用（n - 1）& hash 来进行查找而不是遍历。<br>
允许null键，有专门的函数进行null的逻辑处理。<br>
线程不安全。<br>
***
hashtable: 相比于hashmap并不来自同一个collection，线程安全，实现办法是通过synchronize，每次在put的过程之中，在方法之上打上synchronize。<br>
concurrenthashmap：在put方法之中打上CAS，get不处理实现线程安全。每一步CAS时，使用volatile关键字，保证不会读到脏数据却直接写入内存<br>
LinkedHashmap:保证有序，内部会构建一个双向链表，每一次插入的时候都会在每个node之上前后穿起来，get会进入到双向链表表的最后一位。
***
Arraylist：
扩容大小：原大小*1.5+1，初始大小10. 连续空间数组，扩容的时候是开辟一段新的空间，把原有的数据复制到新的数组之中。<br>
Arraylist删除元素可能会出现问题，最好使用iterator来进行删除或者从尾开始遍历。
```
Iterator<Integer> it=arr.iterator();
while(it.hasNext()){
  Integer i=it.next();
  if(i.equals(target)){
    it.remove();
  }
}
```
为了保证线程安全，使用copyonwriteArraylist，插入时加锁，读不加
***
hashset：与map类似，只是没有链表板块。 再插入之中同样是需要进行一个判断，如果相同的hashcode存在则不再插入，不存在则插入数组
***
Linkedlist：双向链表，无扩容机制。

## Java 多线程

### 线程、进程、协程的关系和区别
进程资源分配的独立单位，拥有独立的内存，CPU栈，独立的虚拟内存。cpu时间片。当发生切换时会保存cpu上下文（程序计数器、cpu寄存器），用户态资源。<br>
线程最小的调度单位。共享虚拟内存，当线程切换时，不用保存虚拟内存。一个进程可以有n个线程<br>
协程时最轻量级的，且只在用户态进行。没有独立的虚拟内存和CPU栈。一个线程可有多个协程<br>
***
用户态和内核态：用户态级别更低，切换只能使用系统调用、硬中断和异常。

### Java创建线程的方式
可以使用Thread，Runnable，Callable，和线程池的方式进行创建。Thread是通过new Thread来重写run方法，Runnable接口可以将线程和任务想分离。 Callable可以产生带返回值的线程（FutureTask）
```
Futuretask <Integer> task=new FutureTask<>(new Callable<Integer>){
  public Integer run(){
      log.debug("123");
      return null;
  }
}

Thread t=new Thread(task);
t.start();
Integer t=task.get();
```
### 线程的生命周期
新建、就绪、阻塞、运行、死亡<br>
引起阻塞：锁，sleep
引起就绪：更高级的线程进入

### Object和Thread的一些方法
wait/notify/notifyAll: 他们都是object方法。wait 函数是当一个线程调用了共享变量的 wait 方法时，该调用线程就会被阻塞挂起，直到其他线程调用了该共享变量的 notify/notifyAll 方法唤醒。<br>
Join：等待线程结束。如果在main线程之中，等到join之后，main才可以介入。
sleep：不释放锁的对象，只释放cpu的使用权。

### Synchronize
在JIT的逃逸分析之中，会把锁自动分成为四种无锁、偏向锁、轻量级锁、重量级锁。<br>







