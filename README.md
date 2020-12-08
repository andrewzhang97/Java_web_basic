# Java-web
常见面试知识点总结，不保证一定对，恳请大佬指正

## java基础

### 抽象类和接口的区别
抽象类：抽象类只是一种特殊的方法的类，只有声明，没有实现。里面可以存在非抽象方法。
```
abstract void fun();
abstract class father{
  abstract void func();
}
```
抽象类是public和protected中的一种，抽象类只能进行单继承，抽象类虽然不可实例化，但是如果main()也可调用其中的函数。
抽象类更多的是对object的抽象化
<br>
接口：
```
interface eat{
  void fun();
}
``` 
接口没有方法体的方法，成员变量默认为public static final，一个类可以实现多个接口，接口的抽象级别更高
接口更多的是进行行为的规范
