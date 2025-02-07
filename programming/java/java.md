[TOC]

# 开发环境搭建

## Ubuntu 16.04 安装
```
$ sudo apt-get update
$ sudo apt-get install openjdk-9-jdk
```

## 查看版本
```
$ java -version
openjdk version "9-internal"
OpenJDK Runtime Environment (build 9-internal+0-2016-04-14-195246.buildd.src)
OpenJDK 64-Bit Server VM (build 9-internal+0-2016-04-14-195246.buildd.src, mixed mode)

$ javac -version
javac 9-internal
```

# Java基础

## Java介绍
- 三大特性
  - 封装
    - 属性
    - 方法
  - 继承
    - 基类
    - 派生类
  - 多态
    - 方法重载(overload)
    - 方法覆盖(override)

- 编译和运行
  ```shell
  $ javac main.java
  $ java main
  ```

- Java的注释
  - 单行注释
    ```
    // This is a single line comment.
    ```

  - 多行注释
    ```
    /*
     * This is a multiline
     * comment
     */
    ```

  - 多行注释并写入javadoc
    ```
    /**
     * This is a multiline comment.<br>
     * and it will be written into javadoc.
     */
    ```

## 基本数据类型
- 基本类型的长度如下

  |基本类型|长度 |举例      |
  |--------|-----|----------|
  |boolean |1 bit|true,false|
  |byte    |1 byte| 123     |
  |char    |2 byte|'a'|
  |short   |2 byte|12|
  |int     |4 byte|123|
  |long    |8 byte|123L|
  |float   |4 byte|1.23f|
  |double  |8 byte|1.23d|

- Java基本数据类型都是signed,不存在unsigned类型


## 基本数据类型的封装
- Integer
- Double
- Long
- Float
- Short
- Byte
- Character
- Boolean


## 数组
- 定义
  ``` java
  int[] array = new int[3];
  ```

- 数组下标从0开始
- 属性
  - length: 数组的长度
    ```java
    array.length
    ```

## 字符串
- **String**
  - immutable
  - 定义
    ```java
    String str = "Hello, World!"
    ```

  - 方法
    - +=: 字符串连接
    - length(): 返回字符的个数
    - charAt(): 返回字符所在的索引

- **StringBuffer**
  - mutable, 线程安全, 适用于**多线程**(很多method加了**synchronized**)
  - 通常用于字符串修改: 插入、删除、追加等
  - 定义
    ```java
    StringBuffer sb = new StringBuffer("Hello, World!");
    ```
  - 方法

- **StringBuilder**
  - mutable, 线程不安全，适用于**单线程**


- 区别
```
Mutability Difference:

String is immutable, if you try to alter their values, another object gets created, whereas StringBuffer and StringBuilder are mutable so they can change their values.

Thread-Safety Difference:

The difference between StringBuffer and StringBuilder is that StringBuffer is thread-safe. So when the application needs to be run only in a single thread then it is better to use StringBuilder. StringBuilder is more efficient than StringBuffer.

Situations:

If your string is not going to change use a String class because a String object is immutable.
If your string can change (example: lots of logic and operations in the construction of the string) and will only be accessed from a single thread, using a StringBuilder is good enough.
If your string can change, and will be accessed from multiple threads, use a StringBuffer because StringBuffer is synchronous so you have thread-safety.
```

## 类
### 封装
- 类的声明
  ```java
  修改符 class class_name {
    修饰符 类型 member_ = value;
    修饰符 类型 method(类型 para1, ...)
  }
  ```

- 类的使用
  ```java
  import java.lang.*
  new java.lang.Date()
  ```

- 静态变量
  - 静态变量属于类，不属于对象
  - 静态变量只能在静态方法中使用
  - 初始化
    ```java
    static {
      count_ = 100;
    }
    ```

- method
  - class(static) method: 静态方法，由类调用
  - instance method: 实例方法，由对象调用

- 成员
  - class variable
  - instance variable

### 继承
```java
public class A {
}

public B extends A {
}
```

- 修饰符
  - public
  - private
  - protected
  - final

### 多态
- overload
  - 同一个类, method名相同, 但signature不同

- override
  - 子类通过继承覆盖父类(同样的signature)
  - final/static/private method不能被override


## 内部类

作者：Paranoid
链接：https://www.zhihu.com/question/28197253/answer/365692360
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

首先，我们需要明白，为什么要设计Java内部类？然后再去探究为何又将内部类设计为静态与非静态两种。

- 将一个类设计成内部类，我觉得有两方面因素：
  - 在一个类内部，需要操作某种属性，而这个属性需要涉及的面又很广，我们可以考虑，将这些属性设计为内部类。
  - 好比你设计类 B 的目的只是为了给类 A 使用，那么，我们就可将其设定为内部类，没有必要将类 B 设置成单独的 Java 文件，防止与其他类产生依赖关系。



- 然后我们再来说说为什么又将内部类设计为静态内部类与内部类
  - 首先来看一下静态内部类的特点：如 昭言 用户所述那样，我是静态内部类，只不过是想借你的外壳用一下。本身来说，我和你没有什么“强依赖”上的关系。没有你，我也可以创建实例。那么，在设计内部类的时候我们就可以做出权衡：如果我内部类与你外部类关系不紧密，耦合程度不高，不需要访问外部类的所有属性或方法，那么我就设计成静态内部类。而且，由于**静态内部类与外部类并不会保存相互之间的引用**，因此在一定程度上，还会节省那么一点内存资源，何乐而不为呢~~
  - 既然上面已经说了什么时候应该用静态内部类，那么如果你的需求不符合静态内部类所提供的一切好处，你就应该考虑使用内部类了。最大的特点就是：你在内部类中需要访问有关外部类的所有属性及方法，我知晓你的一切

总结：首先需要知道为什么会有内部类，什么时候应该使用内部类，我们再去讨论，为什么 Java 的设计者们又将内部类设计为静态与非静态，这样就很清晰了


## 匿名类


## 对象
```java
String s = new String("Hello, world");
String s2 = s;
```

s和s2本身是对象引用，均指向在Heap中分配的对象.

## 抽象类
- 抽象类和接口都**不能被直接实例化**
- 抽象类要被子类继承，接口要被子类实现。
- 接口里面只能对方法进行声明，抽象类既可以对方法进行声明也可以对方法进行实现。
- 抽象类里面的抽象方法必须全部被子类实现，如果子类不能全部实现，那么子类必须也是抽象类。接口里面的方法也必须全部被子类实现，如果子类不能实现那么子类必须是抽象类。
- 接口里面的方法只能声明，不能有具体的实现。这说明接口是设计的结果，抽象类是重构的结果。
- 抽象类里面可以没有抽象方法
- 如果一个类里面有抽象方法，那么这个类一定是抽象类。
- 抽象类中的方法都要被实现，所以抽象方法不能是静态的static，也不能是私有的private。
- 接口（类）可以继承接口，甚至可以继承多个接口。但是类只能继承一个类。
- 抽象级别（从高到低）：接口>抽象类>实现类。

## 接口
- 定义
  ```
  interface if_name {
  }
  ```

- 使用
  ```
  public class implements if_name{

  }
  ```

## 泛型
- Class<?>
  - 类型通配符
  - 类型实参, 不是类型形参

- Class<T>
  - 泛型类
  - 泛型接口
  - 泛型方法
  - [Java 泛型和C++的模板的区别?](https://www.zhihu.com/question/33304378)
    - c++ template必须传入类型进行实例化, java generic不能实例化, 只能类型擦除
    - c++ template传入类型不同, 实例类型也不同. java generic的所有实例都是同一类型, 类型参数会在**运行时**擦除
    - c++ template 可以传入基本数据类型, java只是传入类类型(class type)
    - c++ template的类型参数T可以用于静态变量和方法, java generic不行

## 反射

## 关键字
- 继承: **extends**
- 实现接口: **implements**
- super: 父类
- this: 当前对象

### volatile
- 保证可见性
- 保证有序性(禁止指令重排)

### synchronized
- 保证
  - 保证原子性
  - 保证可见性
  - 保证有序性

- [ ] 实现原理

### final/finally/finalize的区别
- final
  - 修改**成员变量**为常量, 不能被修改
  - 修饰**方法**不能被改写
  - 修饰**类**不能被继承

- finally
  - try/catch/finally语句，最终都要执行的语句块

- finalize
  - gc回收前调用对象的finalize()方法
  - Called by the garbage collector on an object when garbage collection determines that there are no more references to the object.

# openjdk源码实现

## 异常

## 集合
- hashmap
- ConcurrentHashMap 在Java7和Java8中的区别？为什么Java8并发效率更好？什么情况下用HashMap，什么情况用ConcurrentHashMap？
- AtomicInteger怎么实现原子修改的

### Set
- Set代表无序, 不可重复, 独一无二的集合；
- Set是个**interface**
- libcore/ojluni/src/main/java/java/util/Set.java

#### HashSet

- libcore/ojluni/src/main/java/java/util/HashSet.java
- HashSet实现了Set接口
- HashSet**内部使用HashMap**来实现
- HashSet**线程不安全**, 需要在外面调用时保证

- LinkedHashSet
  - 是HashSet的子类
  - 根据元素的hashCode值来决定元素的存储位置，同时使用链表维护元素的次序，使得元素是以插入的顺序来保存的


#### TreeSet
- TreeSet可以确保集合元素处于**排序**状态


#### EnumSet
- EnumSet中的所有元素都必须是指定枚举类型的枚举值
- EnumSet的集合元素也是有序的


### List
- List代表有序、重复的集合；

#### ArrayList
- 线程不安全
- 实现: 数组


#### vector
- 线程安全
- 实现: 数组


#### LinkedList
- 实现: 链表

### Map
- Map代表具有映射关系的集合
- Map是个接口


#### Hashtable
- Hashtable是一个线程安全的Map实现
- 数组+链表

#### HashMap
- 线程不安全

#### LinkedHashMap
- LinkedHashMap=散列表+循环双向链表

#### TreeMap
- TreeMap 是一个有序的key-value集合，它是通过**红黑树**实现的

#### ConcurrentHashMap
- java 7, 分段的数组+链表
- Java 8后, 数组+链表/红黑二叉树。
  - Java 8在链表长度超过一定阈值（8）时将链表（寻址时间复杂度为O(N)）转换为红黑树（寻址时间复杂度为O(log(N))）

### Queue
- Queue代表一种队列集合实现

#### PriorityQueue
- 线程不安全

#### PriorityBlockingQueue
- 线程安全

## ==和equals的区别

### ==
- 基本数据类型: 比较值是否相等
- 对象: 比较是否是同一个对象的引用(对象的内存地址)

### equals
equals()是Object类的一个方法，设计目的是让派生类去override equals()方法。


- Ojbect类的equals

 http://androidxref.com/8.0.0_r4/xref/libcore/ojluni/src/main/java/java/lang/Object.java

```
171    public boolean equals(Object obj) {
172        return (this == obj);
173    }
```
- String对equals的重写

  http://androidxref.com/8.0.0_r4/xref/libcore/ojluni/src/main/java/java/lang/String.java

  - 先比较对象引用是否相等，是则返回true
  - 再比较是否是String对象，不是则返回false
  - 若是String对象，
    - 判断长度是否相同，不是则返回false
    - 是，则依次比较每个字符。若都相等，则返回true, 否则返回false.
```
943    public boolean equals(Object anObject) {
944        if (this == anObject) {
945            return true;
946        }
947        if (anObject instanceof String) {
948            String anotherString = (String) anObject;
949            int n = length();
950            if (n == anotherString.length()) {
951                int i = 0;
952                while (n-- != 0) {
953                    if (charAt(i) != anotherString.charAt(i))
954                            return false;
955                    i++;
956                }
957                return true;
958            }
959        }
960        return false;
961    }
```

- Integer对equals的重写
  - file
  libcore/ojluni/src/main/java/java/lang/Integer.java

  - code
    ```
    public boolean equals(Object obj) {
        if (obj instanceof Integer) {
           return value == ((Integer)obj).intValue();
        }
        return false;
    }
    ```

## ThreadLocal<T>
- src
  - libcore/ojluni/src/main/java/java/lang/ThreadLocal.java

# Java相关utils
## javac
- 源码

  ```java
  public class Main {
    public static void main(String[] args) {
     System.out.println("Hello, world!");
    }
  }
  ```

  - javac编译
    ```shell
    $ javac Main.java

    $ ls -l
     total 8
    -rw-r--r-- 1 SPREADTRUM\tim.zhang SPREADTRUM\domain^users 415  4月 19 17:49 Main.class
    -rw-r--r-- 1 SPREADTRUM\tim.zhang SPREADTRUM\domain^users 731  2月 10 13:45 Main.java
    ```

## java运行
   ```shell
   $ java Main
   Hello, world!
   ```

## javap
- 打印signature
  ```shell
  $ javap -s Main.class
  Compiled from "Main.java"
  public class Main {
  public Main();
    descriptor: ()V

  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
		}
  ```

- disassemble class file
  ```shell
  $ javap -c Main.class
  Compiled from "Main.java"
  public class Main {
    public Main();
      Code:
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return

    public static void main(java.lang.String[]);
      Code:
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #3                  // String Hello, world!
         5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
  }
  ```

## javah
- 查看JNI函数名
- javah -jni 工具来生成头文件，来保证函数名的正确性

## javadoc
- 用法
  ```shell
  $ javadoc -d doc/  Main.java
  Loading source file Main.java...
  Constructing Javadoc information...
  Standard Doclet version 1.8.0_03-Ubuntu
  Building tree for all the packages and classes...
  Generating doc/Main.html...
  Generating doc/package-frame.html...
  Generating doc/package-summary.html...
  Generating doc/package-tree.html...
  Generating doc/constant-values.html...
  Building index for all the packages and classes...
  Generating doc/overview-tree.html...
  Generating doc/index-all.html...
  Generating doc/deprecated-list.html...
  Building index for all classes...
  Generating doc/allclasses-frame.html...
  Generating doc/allclasses-noframe.html...
  Generating doc/index.html...
  Generating doc/help-doc.html...
  ```
