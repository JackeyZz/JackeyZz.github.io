---
title: java成神之路基础篇-基础知识
date: 2019-03-07 13:58:26
tags: [java]
categories: [java整理总结]
---
`前言`
> java成神之路基础篇之基础知识。
不积跬步无以至千里，不积小流无以成江海。
<!--more-->
**************

## 基本数据类型
### 8种基本数据类型：整型、浮点型、布尔型、字符型
- 整型(4种)
  - byte：8位
  - short: 16位
  - int: 32位
  - long: 64位
- 浮点型(2种)
  - float: 32位。直接赋值时必须在数字后加上f或F
  - double：64位。赋值时可以加d或D也可以不加
- 布尔型(1种)
  - boolean：true,false
- 字符型(1种)
  - char：16位。本质是数，故是整数类型，可以用`+`连接，内部是先转换成数字相加，再转回char

### 什么是浮点型？什么是单精度和双精度？为什么不能用浮点型表示金额？
- 浮点类型，即带小数部分的数据。有两种常用数据类型：float类型，double类型。
  - 表示形式有十进制数形式、科学计数法形式（例如，5.12e2（5.12*10^2）,5.12E2（5.12*10^2）,e或E代表指数为10）。
  - 三种特殊的浮点数值：正无穷大（正数除以0，POSITIVE_INFINITY表示）、负无穷大（负数除以0，NEGATIVE_INFINITY表示）、非数（0.0除以0.0，或一个负数开方得到一个非数，NaN表示，NaN不与任何数值相等，包括NaN）。
- 单精度和双精度
  - 单精度(float)：1位符号，8位指数，23位小数。
![](java成神之路基础篇-基础知识/pic1.png)
精度主要取决于尾数部分的位数，float为23位，除去全部为0的情况以外，最小为2的-23次方，约等于1.19乘以10的-7次方，所以float小数部分只能精确到后面6位，加上小数点前的一位，即有效数字为7位
  - 双精度(double)：1位符号，11位指数，52位小数
![](java成神之路基础篇-基础知识/pic2.png)
double 尾数部分52位，最小为2的-52次方，约为2.22乘以10的-16次方，所以精确到小数点后15位，有效位数为16位
- 不能用浮点型表示金额-`精度丢失`
`在程序里写的10进制小数，计算机内部无法用二进制的小数来精确的表达。原因在于：`
由于浮点数在计算机内部的表达方式如上图：符号位+阶码+尾数，因此对于二进制小数，小数点右边能表达的值是 1/2, 1/4, 1/8, 1/16, 1/32, 1/64, 1/128 … 1/(2^n），因此并不能够正确表示10进制的小数值。
`实例：`
可以这样查看一下一个float的内部表示（以0.09f为例）： 
`Float.floatToRawIntBits(0.09f)`
你将会得到：1035489772， 这是10进制的， 转化成二进制， 在前面加几个0补足 32位就是：
`0 01111011 01110000101000111101100`
你可以看到它分成了3段：第一段代表了符号(s) : 0 正数；第二段是阶码(e)：01111011，对应的10进制是123；第三段是尾数(M)。
你看到了尾数和阶码，就会明白这其实是所谓的科学计数法: (-1)^s * M * 2^e
对于阶码e，一共有8位，这是个有符号数， 特别是按照IEEE754规范， 如果不是0或者255， 那就需要减去一个叫偏置量的值，对于float 是127。所以 E = e - 127 = 123-127 = -4
对于尾数M ，如果阶码不是0或者255， 他其实隐藏了一个小数点左边的一个1（节省空间，充分压榨每一个bit啊）。即 M = 1.01110000101000111101100
现在写出来就是: 
1.01110000101000111101100 * 2^-4 
=0.000101110000101000111101100 
= 1/16 + 1/64 + 1/128+ 1/256 + …. 
= 0.0900000035762786865234375
`结论：`这就是0.09的内部表示， 很明显他比0.09更大一些， 是不精确的！

## 自动拆装箱
### 什么是包装类型、什么是基本类型、什么是自动拆装箱
> 在实际程序使用中，程序界面上用户输入的数据都是以字符串类型进行存储的。而程序开发中，我们需要把字符串数据，根据需求转换成指定的基本数据类型，如年龄需要转换成int类型，考试成绩需要转换成double类型等。那么，想实现字符串与基本数据之间转换怎么办呢？

Java中提供了相应的对象来解决该问题，`基本数据类型对象包装类`：java将基本数据类型值封装成了对象。封装成对象有什么好处？可以提供更多的操作基本数值的功能。
8种基本类型对应的包装类如下：
![](java成神之路基础篇-基础知识/pic3.png)
其中需要注意int对应的是Integer，char对应的Character，其他6个都是基本类型首字母大写即可。
`基本数据类型对象包装类特点：用于在基本数据和字符串之间进行转换:`
- 将字符串转成基本类型：
![](java成神之路基础篇-基础知识/pic4.png)
```java
System.out.println(Integer.parseInt("123") + 2);
//打印结果为 125
```
  - 将基本数值转成字符串有3种方式：
    - 基本类型直接与`""`相连接即可；`34+""`
    -  调用String的valueOf方法；String.valueOf(34)；
![](java成神之路基础篇-基础知识/pic5.png)
    - 调用包装类中的toString方法；Integer.toString(34)；
![](java成神之路基础篇-基础知识/pic6.png)
- 基本类型和对象转换
使用int类型与Integer对象转换进行演示，其他基本类型转换方式相同。
  - 基本数值---->包装对象
![](java成神之路基础篇-基础知识/pic7.png)
```java
  Integer i = new Integer(4);//使用构造函数函数
  Integer ii = new Integer("4");//构造函数中可以传递一个数字字符串
```
![](java成神之路基础篇-基础知识/pic8.png)
```java
  Integer iii = Integer.valueOf(4);//使用包装类中的valueOf方法
  Integer iiii = Integer.valueOf("4");//使用包装类中的valueOf方法
```
  - 包装对象---->基本数值
  ![](java成神之路基础篇-基础知识/pic9.png)
```java
  int num = i.intValue();
```
- 自动装箱拆箱
在需要的情况下，基本类型与包装类型可以通用。有些时候我们必须使用引用数据类型时，可以传入基本数据类型。
> 比如：基本类型可以使用运算符直接进行计算，但是引用类型不可以。而基本类型包装类作为引用类型的一种却可以计算，原因在于，Java”偷偷地”自动地进行了对象向基本数据类型的转换。

相对应的，引用数据类型变量的值必须是new出来的内存空间地址值，而我们可以将一个基本类型的值赋值给一个基本类型包装类的引用。原因同样在于Java又”偷偷地”自动地进行了基本数据类型向对象的转换。
>1. 自动拆箱：对象转成基本数值
>2. 自动装箱：基本数值转成对象

```java
Integer i = 4;//自动装箱。相当于Integer i = Integer.valueOf(4);
i = i + 5;//等号右边：将i对象转成基本数值(自动拆箱) i.intValue() + 5; 加法运算完成后，再次装箱，把基本数值转成对象。
```
> 3. 自动装箱(byte常量池)细节的演示:
当数值在byte范围之内时，进行自动装箱，不会新创建对象空间而是使用原来已有的空间。

```java
Integer a = new Integer(3);
Integer b = new Integer(3);
System.out.println(a==b);//false
System.out.println(a.equals(b));//true

System.out.println("---------------------");
Integer x = 127;
Integer y = 127;
//在jdk1.5自动装箱时，如果数值在byte范围之内，不会新创建对象空间而是使用原来已有的空间。
System.out.println(x==y); //true
System.out.println(x.equals(y)); //true
```

### Integer 的缓存机制
`实例：`
```java
public class JavaIntegerCache {
    public static void main(String... strings) {
        Integer integer1 = 3;
        Integer integer2 = 3;
        if (integer1 == integer2)
            System.out.println("integer1 == integer2");
        else
            System.out.println("integer1 != integer2");
        Integer integer3 = 300;
        Integer integer4 = 300;
        if (integer3 == integer4)
            System.out.println("integer3 == integer4");
        else
            System.out.println("integer3 != integer4");
    }
}
//output
integer1 == integer2
integer3 != integer4
```
在Java中，== 比较的是对象引用，而equals比较的是值。但是由于Integer的缓存机制，通过使用相同的对象引用实现了缓存和重用。这种Integer缓存策略仅在自动装箱(autoboxing)(相当于调用valueOf方法)的时候有用，使用构造器创建的 Integer 对象不能被缓存。上面的规则适用于整数区间 -128 到 +127。
```java
public static Integer valueOf(int i) {
       if (i &amp;gt;= IntegerCache.low &amp;amp;&amp;amp; i &amp;lt;= IntegerCache.high)
           return IntegerCache.cache[i + (-IntegerCache.low)];
       return new Integer(i);
   }
```
在创建新的Integer对象之前会先在 IntegerCache.cache 中查找。有一个专门的 Java 类来负责 Integer 的缓存。
```java
private static class IntegerCache {
        static final int low = -128;
        static final int high;
        static final Integer cache[];

        static {
            // high value may be configured by property
            int h = 127;
            String integerCacheHighPropValue =
                sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
            if (integerCacheHighPropValue != null) {
                try {
                    int i = parseInt(integerCacheHighPropValue);
                    i = Math.max(i, 127);
                    // Maximum array size is Integer.MAX_VALUE
                    h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
                } catch( NumberFormatException nfe) {
                    // If the property cannot be parsed into an int, ignore it.
                }
            }
            high = h;

            cache = new Integer[(high - low) + 1];
            int j = low;
            for(int k = 0; k &amp;lt; cache.length; k++)
                cache[k] = new Integer(j++);

            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high &amp;gt;= 127;
        }

        private IntegerCache() {}
    }
```
Javadoc 详细的说明这个类是用来实现缓存支持，并支持 -128 到 127 之间的自动装箱过程。最大值 127 可以通过 JVM 的启动参数 -XX:AutoBoxCacheMax=size 修改。 缓存通过一个 for 循环实现。从小到大的创建尽可能多的整数并存储在一个名为 cache 的整数数组中。这个缓存会在 Integer 类第一次被使用的时候被初始化出来。以后，就可以使用缓存中包含的实例对象，而不是创建一个新的实例(在自动装箱的情况下)。
- 其他缓存对象
有 ByteCache 用于缓存 Byte 对象；有 ShortCache 用于缓存 Short 对象；有 LongCache 用于缓存 Long 对象；有 CharacterCache 用于缓存 Character 对象。
Byte，Short，Long 有固定范围: -128 到 127。对于 Character, 范围是 0 到 127。除了 Integer 可以通过参数改变范围外，其它的都不行。

## String
### 字符串的不可变性
`实例：`
```java
  String c1=new String("abc");
  String c2=new String("abc");
  String c3=c1;
  c1 = "han"
```
![](java成神之路基础篇-基础知识/pic10.png)
![](java成神之路基础篇-基础知识/pic11.png)
![](java成神之路基础篇-基础知识/pic12.png)
一旦一个String对象在内存中创建，它将是不可改变的，所有的String类中方法并不是改变String对象自己，而是重新创建一个新的String对象。这就是`字符串的不可变性`。
`使用StringBuffer 或者 StringBuilder可以修改字符串`。

### JDK 6 和 JDK 7 中 substring 的原理及区别
`substring(start, end)`截取字符串的某一段子字符串。
#### JDK 6
![](java成神之路基础篇-基础知识/pic13.png)
String是通过字符数组实现的。在jdk 6 中，String类包含三个成员变量：char value[]， int offset，int count。他们分别用来存储真正的字符数组，数组的第一个位置索引以及字符串中包含的字符个数。
当调用substring方法的时候，会创建一个新的string对象，但是这个string的值仍然指向堆中的同一个字符数组。这两个对象中只有count和offset 的值是不同的。
`导致的问题：`如果你有一个很长很长的字符串，但是当你使用substring进行切割的时候你只需要很短的一段。这可能导致性能问题，因为你需要的只是一小段字符序列，但是你却引用了整个字符串（因为这个非常长的字符数组一直在被引用，所以无法被回收，就可能导致内存泄露）。在JDK 6中，一般用以下方式来解决该问题，原理其实就是生成一个新的字符串并引用他。
```java
x = x.substring(x, y) + ""
```
#### JDK 7
![](java成神之路基础篇-基础知识/pic14.png)
在jdk 7 中，substring方法会在堆内存中创建一个新的数组。

### replaceFirst、replaceAll、replace 区别
> replaceFirst与replaceAll中的匹配字符串用到了正则表达式，而replace则没有。

`实例：`
```java
String s = "my.test.txt";
System.out.println(s.replace(".", "#"));
System.out.println(s.replaceAll(".", "#"));
System.out.println(s.replaceFirst(".", "#"));

//Output
my#test#txt
###########
#y.test.txt

s.replaceFirst("\\.", "#")

//Output
my#test.txt
```
部分被正则表达式识别的字符列表：
- `.`匹配除换行符以外的任意字符
- `^`匹配字符串的开始
- `$`匹配字符串的结束
- `*`重复零次或更多次
- `+`重复一次或更多次
- `?`重复零次或一次

### String 对“+”的重载
在Java中是不支持重载运算符的，String的+是java中唯一的一个重载运算符，那么java使如何实现这个加号的呢？我们先看一段代码：
```java
public static void main(String[] args) {
    String s1 = "yves";
    String s2 = s1 + "he";
}
```
反编译结果如下：
```java
public static void main(String[] args) {
    String s1 = "yves";
    String s2 = (new StringBuilder(String.valueOf(s1))).append("he").toString();
}
```
`结论：`其实String对+的支持其实就是使用了StringBuilder以及他的append、toString两个方法。
### 字符串拼接的几种方式和区别
- 直接用“+”号：jvm底层还是调用StringBuilder的append方法，每拼接一个字符串会创建对象，损耗性能。
- 使用String的方法concat：申请一个char类型的buf数组，将需要拼接的字符串都放在这个数组里，最后再转换成String对象。
- 使用StringBuilder的append：
- 使用StringBuffer的append：append方法加了sychronized关键字，因此是线程安全的。

`结论：`
- 无论如何直接用“+”号连接字符串都是最慢的
- 在拼接少数字符串（不超过4个）的时候，concat效率是最高的
- 多个字符串拼接的时候，StringBuilder/StringBuffer的效率是碾压的
- 在不需要考虑线程安全问题的时候，使用StringBuilder的效率比StringBuffer更高

### String.valueOf 和 Integer.toString 的区别
二者都是将int转换成String类型的。
其中String.valueOf有大量的重载方法，其重载方法核心代码是将JAVA基本类型(int,double,boolean等)和对象(Object)通过不同的处理方式转换成String型，而String.valueOf(int)中调用了Integer.toString的方法。

### switch对String的支持
自java 1.7以后, java switch开始支持String类型。
```java
public class SwitchString {
    public static void main(String[] args) {
        switch (args[0]) {
	    case "A" : break;
            case "B" : break;
            default :
	}//switch
    }
}
```
`反编译得到：`
```java
public class SwitchString {
    public SwitchString() {
    }

    public static void main(String[] var0) {
        String var1 = var0[0];
        byte var2 = -1;
        switch(var1.hashCode()) {
        case 65:
            if (var1.equals("A")) {
                var2 = 0;
            }
            break;
        case 66:
            if (var1.equals("B")) {
                var2 = 1;
            }
        }

        switch(var2) {
        case 0:
        case 1:
        default:
        }
    }
}
```
`结论：`java中switch支持String，是利用String的hash值，本质上是switch-int结构。由于String的Hash可能会冲突，即两个不同的String可能计算出相同的hash值，因此利用到了equals方法来防止hash冲突的问题。最后利用switch-byte结构，精确匹配。
### 字符串池、常量池（运行时常量池、Class 常量池）、intern
## 熟悉Java中各种关键字
transient、instanceof、final、static、volatile、synchronized、const原理及用法
## 集合类
### 常用集合类的使用
### ArrayList 和 LinkedList 和 Vector 的区别
### SynchronizedList 和 Vector 的区别
### HashMap、HashTable、ConcurrentHashMap 区别、
### Set 和 List 区别？Set 如何保证元素不重复？
### Java 8 中 stream 相关用法
### apache 集合处理工具类的使用、不同版本的JDK中HashMap的实现的区别以及原因
### Collection 和 Collections 区别
### Arrays.asList 获得的 List 使用时需要注意什么
### Enumeration 和 Iterator 区别
### fail-fast 和 fail-safe
### CopyOnWriteArrayList、ConcurrentSkipListMap
## 枚举
### 枚举的用法、枚举的实现、枚举与单例、Enum 类
### Java 枚举如何比较
### switch 对枚举的支持
### 枚举的序列化如何实现
### 枚举的线程安全性问题
## IO
### 字符流、字节流、输入流、输出流、
### 同步、异步、阻塞、非阻塞、Linux 5种IO模型
### BIO、NIO 和 AIO 的区别、三种 IO 的用法与原理、netty
## 反射
### 反射与工厂模式、反射有什么用
### Class 类、java.lang.reflect.*
## 动态代理
### 静态代理、动态代理
### 动态代理和反射的关系
### 动态代理的几种实现方式
### AOP
## 序列化
### 什么是序列化与反序列化
### 为什么序列化
### 序列化底层原理
### 序列化与单例模式
### protobuf
### 为什么说序列化并不安全
## 注解
### 元注解
### 自定义注解
### Java 中常用注解使用
### 注解与反射的结合
### Spring 常用注解
## JMS
### 什么是Java消息服务、JMS消息传送模型
## JMX
### java.lang.management.*
### javax.management.*
## 泛型
### 泛型与继承、类型擦除
### 泛型中 KTVE?
### object 等的含义
### 泛型各种用法
### `List<object>`和原始类型List之间的区别
### `List<?>`和`List<Object>`之间的区别是什么
### 限定通配符和非限定通配符
### 上下界限定符 extends 和 super
## 单元测试
### junit
### mock
### mockito
### 内存数据库（h2）
## 正则表达式
### java.lang.util.regex.*
## 常用的 Java 工具库
### commons.lang
### `commons.*...`
### guava-libraries
### netty
## API & SPI
### API、API 和 SPI 的关系和区别
### 如何定义 SPI、SPI 的实现原理
## 异常
### 异常类型、正确处理异常、自定义异常
### Error 和 Exception
### 异常链、try-with-resources
### finally 和 return 的执行顺序
## 时间处理
### 时区、冬令时和夏令时、时间戳、Java 中时间 API
### 格林威治时间、CET,UTC,GMT,CST 几种常见时间的含义和关系
### SimpleDateFormat 的线程安全性问题
### Java 8 中的时间处理
### 如何在东八区的计算机上获取美国时间
## 编码方式
### Unicode
### 有了Unicode为啥还需要UTF-8
### GBK、GB2312、GB18030 之间的区别
### UTF8、UTF16、UTF32 区别
### URL编解码
### Big Endian 和 Little Endian
### 如何解决乱码问题
## 语法糖
### Java 中语法糖原理、解语法糖
语法糖：switch 支持 String 与枚举、泛型、自动装箱与拆箱、方法变长参数、枚举、内部类、条件编译、 断言、数值字面量、for-each、try-with-resource、Lambda 表达式
