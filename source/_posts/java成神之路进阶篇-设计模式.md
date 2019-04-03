---
title: java成神之路进阶篇-设计模式
date: 2019-03-14 17:09:34
tags: [java]
categories: [java整理总结]
---
`前言`
> java成神之路进阶篇之设计模式。
不积跬步无以至千里，不积小流无以成江海。
<!--more-->
**************

## 设计模式
### 单例模式（转发）
#### 概念
`单例模式（Singleton Pattern）`是Java中最简单的设计模式之一。这种类型的设计模式属于创建型模式。书中给出的定义为：`保证一个类仅有一个实例，并提供一个访问它的全局访问点`。

单例模式一般体现在类声明中，单例的类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。

>注意：
1、单例类只能有一个实例。
2、单例类必须自己创建自己的唯一实例。
3、单例类必须给所有其他对象提供这一实例。

#### 优缺点
`优点：`
- 在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）。
- 避免对资源的多重占用（比如写文件操作）。

`缺点：`
没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。

#### 单例模式的六种写法
>单例模式的代码要素：
1、将构造函数私有化
2、在类的内部创建实例
3、提供获取唯一实例的方法

- 1、饿汉式
```java
//code 1
public class Singleton {
    //在类内部实例化一个实例
    private static Singleton instance = new Singleton();
    //私有的构造函数,外部无法访问
    private Singleton() {
    }
    //对外提供获取实例的静态方法
    public static Singleton getInstance() {
        return instance;
    }
}
```
所谓`饿汉`，这是个比较形象的比喻。对于一个饿汉来说，他希望他想要用到这个实例的时候就能够立即拿到，而不需要任何等待时间。所以，`通过static的静态初始化方式，在该类第一次被加载的时候，就有一个SimpleSingleton的实例被创建出来了`。这样就保证在第一次想要使用该对象时，他已经被初始化好了。

同时，由于该实例在类被加载的时候就创建出来了，所以也避免了`线程安全问题。（原因可见：Java的ClassLoader机制（源码级别）、Java类的加载、链接和初始化）`

`饿汉模式的变种:`
```java
//code 3
public class Singleton2 {
    //在类内部定义
    private static Singleton2 instance;
    static {
        //实例化该实例
        instance = new Singleton2();
    }
    //私有的构造函数,外部无法访问
    private Singleton2() {
    }
    //对外提供获取实例的静态方法
    public static Singleton2 getInstance() {
        return instance;
    }
}
```
code 3和code 1其实是一样的，都是在类被加载的时候实例化一个对象。

`饿汉式单例的缺点与解决办法：`
饿汉式单例，在类被加载的时候对象就会实例化。这也许会造成不必要的消耗，因为有可能这个实例根本就不会被用到。而且，如果这个类被多次加载的话也会造成多次实例化。其实解决这个问题的方式有很多，下面提供两种解决方式，第一种是使用静态内部类的形式。第二种是使用懒汉式。

- 2、懒汉式，线程不安全
```java
//code 5
public class Singleton {
    //定义实例
    private static Singleton instance;
    //私有构造方法
    private Singleton(){}
    //对外提供获取实例的静态方法
    public static Singleton getInstance() {
        //在对象被使用的时候才实例化
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```
这段代码简单明了，而且使用了`懒加载模式(即对象被需要使用时才创建对象实例)`，但是却存在致命的问题。`当有多个线程并行调用 getInstance()的时候，就会创建多个实例。也就是说在多线程下不能正常工作`。

- 3、线程安全的懒汉式
```java
//code 6
public class SynchronizedSingleton {
    //定义实例
    private static SynchronizedSingleton instance;
    //私有构造方法
    private SynchronizedSingleton(){}
    //对外提供获取实例的静态方法,对该方法加锁
    public static synchronized SynchronizedSingleton getInstance() {
        //在对象被使用的时候才实例化
        if (instance == null) {
            instance = new SynchronizedSingleton();
        }
        return instance;
    }
}
```
`针对线程不安全的懒汉式的单例，其实解决方式很简单，就是给创建对象的步骤加锁`。

这种写法能够在多线程中很好的工作，而且看起来它也具备很好的延迟加载，但是，遗憾的是，他`效率很低`，因为99%情况下不需要同步。（因为上面的synchronized的加锁范围是整个方法，该方法的所有操作都是同步进行的，但是对于非第一次创建对象的情况，也就是没有进入if语句中的情况，根本不需要同步操作，可以直接返回instance。）这就引出了双重检验锁。

- 4、双重检测机制(DCL)懒汉式
```java
//code 7
public class Singleton {

    private static Singleton singleton;

    private Singleton() {
    }

    public static Singleton getSingleton() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}
```
针对上面code 6存在的问题，相信对并发编程了解的同学都知道如何解决。其实上面的代码存在的问题主要是锁的范围太大了。只要缩小锁的范围就可以了。那么如何缩小锁的范围呢？`相比于同步方法，同步代码块的加锁范围更小`。code 6可以改造成上面的样子。

`双重检验锁模式（Double Checked Locking Pattern）`，是一种使用同步块加锁的方法。`称其为双重检查锁，因为会有两次检查 instance == null，一次是在同步块外，一次是在同步块内`。为什么在同步块内还要再检验一次？因为可能会有多个线程一起进入同步块外的if，如果在同步块内不进行二次检验的话就会生成多个实例了。

但是，事情这的有这么容易吗？上面的代码看上去好像是没有任何问题。`实现了惰性初始化，解决了同步问题，还减小了锁的范围，提高了效率`。但是，该代码还存在隐患。隐患的原因主要和Java内存模型（JMM）有关。

主要在于`singleton = new Singleton()`这句，这并非是一个原子操作，事实上在JVM 中这句话大概做了下面3件事情:
- 给singleton分配内存
- 调用Singleton的构造函数来初始化成员变量
- 将singleton对象指向分配的内存空间（执行完这步singleton就为非 null 了）

但是在 JVM 的即时编译器中存在`指令重排序的优化`。也就是说上面的第二步和第三步的顺序是不能保证的，最终的执行顺序可能是1-2-3也可能是1-3-2。如果是后者，则在3 执行完毕、2 未执行之前，被线程二抢占了，这时instance已经是非null 了（但却没有初始化），所以线程二会直接返回instance，然后使用，然后顺理成章地报错。

在J2SE 1.4或更早的版本中使用双重检查锁有潜在的危险，有时会正常工作（区分正确实现和有小问题的实现是很困难的。取决于编译器，线程的调度和其他并发系统活动，不正确的实现双重检查锁导致的异常结果可能会间歇性出现。重现异常是十分困难的。） 在J2SE 5.0中，这一问题被修正了。`volatile关键字保证多个线程可以正确处理单件实例`。

所以，针对code 7 ，可以有code 8 和code 9两种替代方案：
```java
//code 8
public class VolatileSingleton {
    private static volatile VolatileSingleton singleton;

    private VolatileSingleton() {
    }

    public static VolatileSingleton getSingleton() {
        if (singleton == null) {
            synchronized (VolatileSingleton.class) {
                if (singleton == null) {
                    singleton = new VolatileSingleton();
                }
            }
        }
        return singleton;
    }
}
```
有些人认为使用 volatile 的原因是可见性，也就是可以保证线程在本地不会存有 singleton 的副本，每次都是去主内存中读取。但其实是不对的。使用 volatile 的主要原因是其另一个特性：禁止指令重排序优化。也就是说，在 volatile 变量的赋值操作后面会有一个内存屏障（生成的汇编代码上），读操作不会被重排序到内存屏障之前。比如上面的例子，取操作必须在执行完 1-2-3 之后或者 1-3-2 之后，不存在执行到 1-3 然后取到值的情况。从「先行发生原则」的角度理解的话，就是对于一个 volatile 变量的写操作都先行发生于后面对这个变量的读操作（这里的“后面”是时间上的先后顺序）。

但是特别注意在 Java 5 以前的版本使用了 volatile 的双检锁还是有问题的。其原因是 Java 5 以前的 JMM （Java 内存模型）是存在缺陷的，即时将变量声明成 volatile 也不能完全避免重排序，主要是 volatile 变量前后的代码仍然存在重排序问题。这个 volatile 屏蔽重排序的问题在 Java 5 中才得以修复，所以在这之后才可以放心使用 volatile。

上面这种双重校验锁的方式用的比较广泛，他解决了前面提到的所有问题。但是，即使是这种看上去完美无缺的方式也可能存在问题，那就是遇到序列化的时候。

`使用final:`
```java
//code 9
class FinalWrapper<T> {
    public final T value;

    public FinalWrapper(T value) {
        this.value = value;
    }
}

public class FinalSingleton {
    private FinalWrapper<FinalSingleton> helperWrapper = null;

    public FinalSingleton getHelper() {
        FinalWrapper<FinalSingleton> wrapper = helperWrapper;

        if (wrapper == null) {
            synchronized (this) {
                if (helperWrapper == null) {
                    helperWrapper = new FinalWrapper<FinalSingleton>(new FinalSingleton());
                }
                wrapper = helperWrapper;
            }
        }
        return wrapper.value;
    }
}
```
- 5、静态内部类
```java
public class Singleton {  
    private static class SingletonHolder {  
        private static final Singleton INSTANCE = new Singleton();  
    }  

    private Singleton (){}  

    public static final Singleton getInstance() {  
        return SingletonHolder.INSTANCE;  
    }  
}   
```
这种方式能达到双检锁方式一样的功效，但实现更简单。对静态域使用延迟初始化，应使用这种方式而不是双检锁方式。这种方式只适用于静态域的情况，双检锁方式可在实例域需要延迟初始化时使用。
这种方式同样利用了 classloder 机制来保证初始化 instance 时只有一个线程，它跟第 1 种方式不同的是：第 1 种方式只要 Singleton 类被装载了，那么 instance 就会被实例化（没有达到 lazy loading 效果），而这种方式是 Singleton 类被装载了，instance 不一定被初始化。因为 SingletonHolder 类没有被主动使用，只有通过显式调用 getInstance 方法时，才会显式装载 SingletonHolder 类，从而实例化 instance。想象一下，如果实例化 instance 很消耗资源，所以想让它延迟加载，另外一方面，又不希望在 Singleton 类加载时就实例化，因为不能确保 Singleton 类还可能在其他的地方被主动使用从而被加载，那么这个时候实例化 instance 显然是不合适的。这个时候，这种方式相比第 1 种方式就显得很合理。

这种写法仍然使用JVM本身机制保证了线程安全问题；由于 SingletonHolder 是私有的，除了 getInstance() 之外没有办法访问它，因此它是懒加载的；同时读取实例的时候不会进行同步，没有性能缺陷；也不依赖 JDK 版本。

- 6、枚举式
```java
// code 10
public enum  Singleton {

    INSTANCE;

    Singleton() {
    }
}
```
在Java 1.5之前，实现单例一般只有以上几种办法，在Java 1.5之后，还有另外一种实现单例的方式，那就是使用枚举。可以通过Singleton.INSTANCE来访问实例。

这种方式是`Effective Java`作者`Josh Bloch` 提倡的方式(Effective Java 第3条)，它不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象（下面会介绍），可谓是很坚强的壁垒啊，在深度分析Java的枚举类型—-枚举的线程安全性及序列化问题中有详细介绍枚举的线程安全问题和序列化问题。

那这种有啥好处？枚举的方式实现：
- 简洁
- 无尝提供了序列化机制
- 绝对防止多次实例化，即使是在面对复杂的序列化或者反射攻击的时候(安全)！

#### 注意点
有两个问题需要注意：
1、如果单例由不同的类装载器装入，那便有可能存在多个单例类的实例。假定不是远端存取，例如一些servlet容器对每个servlet使用完全不同的类装载器，这样的话如果有两个servlet访问一个单例类，它们就都会有各自的实例。
该问题可以通过如下方式修复：
```java
private static Class getClass(String classname)  
throws ClassNotFoundException {  
    ClassLoader classLoader = Thread.currentThread().getContextClassLoader();
    if(classLoader == null)     
          classLoader = Singleton.class.getClassLoader();     
          return (classLoader.loadClass(classname));     
       }     
    }  
```

2、如果Singleton实现了java.io.Serializable接口，那么这个类的实例就可能被序列化和复原。不管怎样，如果你序列化一个单例类的对象，接下来复原多个那个对象，那你就会有多个单例类的实例。序列化问题参考下面的分析。

单例与序列化
在单例与序列化的那些事儿一文中，分析过单例和序列化之前的关系——序列化可以破坏单例。要想防止序列化对单例的破坏，只要在Singleton类中定义readResolve就可以解决该问题。
```java
//code 11
package com.hollis;
import java.io.Serializable;
/**
 * Created by hollis on 16/2/5.
 * 使用双重校验锁方式实现单例
 */
public class Singleton implements Serializable{
    private volatile static Singleton singleton;
    private Singleton (){}
    public static Singleton getSingleton() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }

    private Object readResolve() {
        return singleton;
    }
}
```
#### 总结
一般来说，单例模式有五种写法：懒汉、饿汉、双重检验锁、静态内部类、枚举。上述所说都是线程安全的实现，文章给出的第2种方法不算正确的写法。
一般情况下直接使用饿汉式就好了，如果明确要求要懒加载（lazy initialization）会倾向于使用静态内部类，如果涉及到反序列化创建对象时，可以使用枚举的方式来实现单例。如果有其他特殊的需求，可以考虑使用双检锁方式。
