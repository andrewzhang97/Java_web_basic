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




