---
title: 企业软件开发_EJB
date: 2018-03-19 13:23:10
tags: [企业软件开发]
categories: [课堂笔记]
---
## EJB概述
### EJB定义
EJB(Enterprice JavaBean)是sun的服务器端组件模型，最大的用处是部署分布式应用程序，类似于微软的.net技术。凭借java跨平台的优势，用EJB技术部署的分布式系统可以不限于特定的平台。  
`企业商务软件的核心部分就是它的业务逻辑，业务逻辑抽象了整个商务过程的流程，并使用计算机语言将它们实现`。而J2EE对于这个问题的处理方法就是将业务逻辑从客户端软件中抽取出来，封装在一个组件中。这个组件运行在一个独立的服务器上，客户端软件通过网络调用组件提供的服务以实现业务逻辑，而客户端软件的功能单纯到只负责发送调用请求和显示处理结果。在J2EE中，`这个运行在一个独立的服务器上，并封装了业务逻辑的组件就是EJB组件`。
从上面可以看出，EJB就是将那些业务逻辑的“类”放到一个服务器上，用`C/S`形式的软件客户端对服务器上的“类”进行调用。  
EJB和JavaBean的异同：  
- 相同点
  - 都有get和set方法
  - 都是可重用的组件
  - 都可以进行组装来构建java应用
- 不同点
  - JavaBean比EJB小(JavaBean没EJB复杂)
  - JavaBean是开发组件(没有容器去专门管理),而EJB是部署组件

### EJB的优点
- EJB服务器完成“繁杂”的工作：应用开发人员关注于业务逻辑的实现而不是底层的实现机制
- 支持事务处理
  - 多个业务操作同时成功，或全部失败
  - 可以通过在代码外的描述来定义事务处理级别
- 可扩展性
  - EJB可以根据应用的增长而扩展
  - EJB服务器往往还提供了负载均衡
- 安全性：由EJB服务器提供资源的访问权限控制
- 使开发方式或应用程序变得容易

### EJB特点
- 清晰的体系结构：支持多层应用体系结构和基于构件开发
- 简化的编程模型  
EJB构件的开发者只需关注业务服务实现，一些复杂的任务如生命周期管理、持久性、事务处理、安全、一致性及资源管理等等都是由EJB容器负责。
- 通用的编程模型：提供各种服务的高层API，java是其编程语言
- 易移植性：可以部署到任何兼容的EJB容器中
- 支持事务处理：可以通过在代码外的描述来定义事务处理
- 可扩展性：EJB可以根据应用的增长而扩展，往往还提供负载平衡
- 安全性：由EJB服务器提供资源的访问权限控制

### EJB容器
>EJB在EJB容器中运行，运行时环境由容器建立。当完成EJB的开发后，需要将EJB组件部署到EJB容器中，只有正确部署后，EJB容器才能向客户提供业务服务。  

#### 作用
EJB构件并不在EJB服务器的顶部直接执行。一个称为EJB容器的中间件在EJB服务器环境中运行，EJB容器为EJB构件提供操作环境。
- EJB容器充当客户端和Bean之间的调用层，首先拦截调用，以确保持续、事务和安全性都正确应用于客户机对EJB组件执行的每一个操作
- EJB容器的最重要职责就是提供一个EJB可以运行的环境
- EJB容器负责管理EJB，负责EJB迁入和迁出内存，以及EJB实例化
- EJB容器装载EJB，并且使得客户端程序能进行远程调用  

#### 管理内容
主要来说，EJB容器管理了以下的系统级问题：
- 持久性：容器决定何时载入存储状态，又可分为CMP和BMP
- 生命周期管理：容器管理EJB构件的实例，使EJB构件实现最大的效能和内存利用率。容器能够激活和钝化EJB构件、管理实例池等等
- 安全性：部署描述文件定义了客户能够访问的不同的应用函数。容器只允许授权的客户访问这些受保护的函数
- 事务管理：容器负责管理分布式事务处理的复杂问题
- 远程连接：容器为远程连接，管理底层的通信问题，而且对EJB构件的开发者和客户都隐藏了通信问题  

#### 交互机制
EJB依赖容器来获取它的资源需求，并通过以下机制与容器交互：
- 回调方法：每个EJB都会实现EnterpriseBean接口的子类型及其回调方法，回调方法让EJB在事件之前或之后立即执行内部调整。
- EJBContext对象：每个EJB都会得到一个EJBContext对象，EJB通过EJBContext对象请求关于环境的信息，如客户端身份或事务状态
- Java命名和目录接口(JNDI)：每个EJB自动拥有对一个特定命名系统ENC(Environment Naming Context)的访问权，JNDI ENC运行EJB访问各种资源，如JDBC连接、其他EJB等

#### EJB和JNDI资源
EJB对象由容器进行管理，因此不能在程序中使用创建实例的方法来生成EJB对象，而需要采用Java的JNDI来获得EJB对象的引用。  
简单来说，JNDI就是一种将对象和名字绑定的技术，对象工厂(此处指JBoss容器)负责生产出对象，这些对象都有唯一的名字绑定。外部程序可以通过名字来获得对象的引用
- 在`javax.naming`包中提供了`Context`接口，该接口提供了通过名字检索对象的方法
- `Object lookup(String name)`返回与指定的名字绑定的对象

![hh](企业软件开发-EJB/pic11.png)

### EJB运行方式
![hh](企业软件开发-EJB/pic10.png)

### EJB访问过程
- EJB客户端通过JNDI查找EJB的Home接口
- 然后通过Home接口获得EJB对外接口
- 获得对外接口后，调用对外接口定义的EJB业务方法，而真正的逻辑是Bean对象完成的
![hh](企业软件开发-EJB/pic14.png)  

客户端如何获得EJB对象的引用？  
- EJB对象通过Home对象来得到。在容器中由Home对象专门负责对EJB对象的实例化和回收：  
  - 生成EJB对象
  - 查找现存的EJB对象(仅限于实体Bean)
  - 删除EJB对象  
- Home对象由EJB容器自动生成
- Home对象依照Home接口生成
- Home接口在EJB容器中控制Bean的生命周期(包括创建、删除和定位Bean)。Home接口是创建EJB的工厂
  - 远程Home接口：继承`javax.ejb.EJBHome`
  - 本地Home接口：继承`javax.ejb.EJBLocalHome`
- Home接口是由EJB容器提供的

### EJB实现技术
EJB是运行在独立服务器上的组件，客户端是通过网络对EJB对象进行调用的。在java中能够实现远程对象调用的技术是RMI，而EJB技术基础就是RMI。通过RMI技术，J2EE将EJB组件创建为远程对象，客户端就可以通过网络调用EJB对象了。
#### 对象的序列化
对象的序列化过程就是将对象状态转换成字节流和从字节流恢复对象。将对象状态转换成字节流之后，可以用java.io包中的各种字节流类将其保存到文件中，或者通过网络连接将对象数据发送到另一个主机。  
通俗点说，对象的序列化就是将你程序中实例化的某个类的对象，转换成字节数组，也就是说可以放到一个byte数组中，这时候，你既然已经把一个对象放到了byte数组中，那么你就可以随便处置它了，用的最多的就是将它发送到网络上远程的计算机上。
![hh](企业软件开发-EJB/pic5.png)

#### 分布式计算与RPC
RPC并不是一个纯粹的java概念，因为在java诞生之前就已经有了RPC这个概念，RPC是“Remote Procedure Call”的缩写，即为“远程过程调用”。说白了，就是本地计算机调用远程计算机的一个函数。
![RPC](企业软件开发-EJB/pic6.png)  

#### RMI
##### 概念
RMI就是将对象序列化和RPC结合在一起，英文全称为“Remote Method Invocation”，即为“远程方法调用”，它就是利用java对象序列化的机制实现分布式计算，实现远程类对象的实例化以及调用方法。通俗点就是利用对象序列化来实现远程调用，使用这个方法来调用远程类时，就不需要编写socket程序，也不需要把对象进行序列化操作，直接调用就行。  
远程方法调用是一种计算机之间对象相互调用对方函数，启动对方进程的一种机制，使用这种机制，某一台计算机上的对象在调用另外一台计算机上的方法时，使用的程序语法规则和在本地机上对象间的方法调用的语法规则一样。  
![RPC](企业软件开发-EJB/pic7.png)

##### RMI调用机制
![RPC](企业软件开发-EJB/pic7.png)
RMI采用`stubs`和`skeletons`来进行远程对象的通讯。`stub`充当远程对象的客户端代理，有着和远程对象相同的远程接口，远程对象的调用实际是通过调用该对象的客户端代理对象`stub`来完成的。  
![](企业软件开发-EJB/PIC13.PNG)
###### stub(客户端代理对象)
负责处理与客户端的网络通信，负责调用参数和返回值的流化(serialization)、打包解包以及网络层的通讯过程。用户调用这个对象。
- 与远程对象所在的虚拟机建立连接
- 打包(marshal)参数并发送到远程虚拟机
- 等待执行结果
- 解包(unmarshal)返回值或返回的错误
- 返回调用结果给调用程序

###### skeleton(服务器端代理对象)
负责处理分布式对象(ejb)的通信，`stub`通过网络调用
- 解包`stub`传来的参数
- 调用远程对象匹配的方法
- 打包返回值或错误发送给`stub`对象


##### 优点
RMI机制给分布式计算的系统设计、编程都带来了极大的方便，只要按照RMI规则设计程序，可以不必再过问在RMI之下的网络细节，如TCP和socket等等。任何两台计算机之间的通讯完全由RMI负责，调用远程计算机上的对象就像本地对象一样方便。RMI可以将完整的对象作为参数和返回值进行传递，而不仅仅是预定义的数据类型，也就是说可以传递类似java哈希表这样的复杂类型的参数。

##### 缺点
如果是较为简单的方法调用，其执行效率也许会比本地执行慢很多，即便和远程socket机制的简单数据返回的应用相比，也会慢一些，原因是其在网络间需要传递的信息不仅仅包含该函数的返回值信息，还会包含该对象序列化后的字节内容。  

### EJB的服务群集
#### 概念
RMI是将各种任务与功能的类放到不同的服务器上，然后通过各个服务器间建立的调用规则实现分布式的运算。故EJB的服务群集就是将原来在一个计算机上运算的几个类，分别放到其他计算机上去运行，以便分担运行这几个类所需要占用的CPU和内存资源，同时，也可以将不同的软件功能模块放到不同的服务器上，当需要修改某些功能的时候直接修改这些服务器上的类就行，修改之后所有客户端的软件都被修改了。
![RPC](企业软件开发-EJB/pic8.png)

#### 缺点
![RPC](企业软件开发-EJB/pic9.png)  
如图，如果想实现各个服务器针对同一个数据库的查询，那么不管你部署多少个功能服务器，都需要针对一个数据库服务器进行查询操作。也就是说，不管你的“计算”有多么的“分布”也同样需要从一台服务器中取得数据。虽然看起来将各个功能模块分布在不同的服务器上从而分担了各个主计算机的CPU资源，然而真正的瓶颈并不在这里，而是在数据库服务器上。数据库服务器都会非常忙的应付各个服务器的查询和操作请求。因此显然EJB并不能完全解决负载的问题。  
假设将每个功能服务器后面都部署一个数据库，解决了数据库查询负载的问题，但是出现了“数据共享”的问题。

## EJB组成部分
- 远程接口：remote接口
remote接口继承`javax.ejb.EJBObject`接口，定义了在bean中实现的业务逻辑方法
- 本地接口：home接口
home接口继承了`javax.ejb.EJBHome`接口，定义了创建、查找EJB的方法
- Bean类(三类)：客户端不能对Bean直接操作实现业务逻辑。根据需要可以实现：
  - `javax.ejb.SessionBean`
  - `javax.ejb.EntityBean`
  - `java.ejb.MessageDrivenBean`

**EJB接口之Local与Remote**
发布EJB时，可以选择每个接口的访问方式：本地访问或远程访问。顾名思义，本地访问就是指调用者在与EJB同一台机器（JVM）中访问EJB，否则就是远程访问了。在一个EJB中，Remote与Local能够同时使用，但是需要配置各自的value属性，且一个接口只能用一种注解标注

- **Local 接口**
  - 注解为”@Local”的接口称为本地接口（或者@Local和@Remote都不存在时，默认为Local接口）。
  - 由于在同一个JVM中运行，调用者调用EJB时，直接查找地址访问EJB，快速。
  - 访问的是服务端的EJB本身。
  - 若需要传入对象做参数，对象可以不用实现序列化接口。
  - 常用于同一个EJB中Session Bean和Entity Bean之间的相互调用
- **Remote 接口**
  - 注解为”@Remote”的接口称为远程接口
  - 以RMI(Remote Method Invocation 远程方法调用)的方式实现调用
  - 访问的是EJB的拷贝。
  - 若需要传入对象做参数，对象必须实现序列化接口。
  - 常用于 servlet/jsp 或 Java 应用客户端的调用。
- **使用总结**
  - 如果你的 EJB 客户与 EJB 处于同一个JVM环境中时，你可以只生成 local 类型接口（包括EJBHome 与EJBObject），如果你需要在与EJB容器不同的JVM环境中调用你的EJB的话，你必须生成Remote类型的接口（包括EJBHome 与 EJBObject）;
  - 在一般情况下建议两种类型的接口都生成。尤其是Session Bean,Entity Bean，可以只生成local类型的接口，如果想远程调用你的Entity Bean一般用Session Bean做代理
  - 如果你不是远程调用EJB的话，使用EJB时建议调用local接口，这样效率高，因为远程调用就意味着建立网络连接，效率必然不如local调用。
***
### 主接口
从EJB客户视图来看：
- 本地用户：使用本地接口和本地主接口。按引用传递值
- 远程接口：使用远程接口和远程主接口。按变量传递值

主接口负责控制bean的寿命周期操作：生成、删除寻找bean。是客户程序访问EJB的第一个联系点。客户通过JNDI找到bean主接口，然后可以对bean进行下列操作：
- 生成新的实例或者寻找现有的bean实例(本地或者远程)
- 访问EJBMetaData接口(远程)
- 取得bean实例的序列化引用(远程)
- 删除bean实例
- 执行主业务方法

主接口实现的方法：
- `void remove(Handle handle) throws RemoteException,RemoveException`
- `void remove(Object object) throws RemoteException,RemoveException`
- `EJBMetaData getEJBMetaData() throws RemoteException`
- `HomeHandle getHomeHandle() throws RemoteException`

### bean类结构
bean类中封装了所有业务逻辑方法，实现了本地和远程接口。  
必须要实现的方法：
- `public void ejbCreate(){}`
- `public void ejbRemove(){}`
- `public void ejbActivate(){}`//激活有状态会话bean
- `public void ejbPassivate(){}`
- `public void setSessionContext(SessionContext ctx){}`//会话

### 远程接口
远程接口组件定义了要向客户提供的业务方法，这些方法在bean的实现类中去实现。
远程接口的方法：
- `EJBHome getEJBHome() throws RemoteException;`
- `Object getPrimaryKey() throws RemoteException;`
- `void remove() throws RemoteException, RemoveException;`
- `Handle getHandle() throws RemoteException`
- `boolean isIdentical(EJBObject eJBObject) throws RemoteException`

标准的设计模式是业务逻辑层的所有访问使用会话bean，这些会话bean需要发表远程接口，使客户层可以访问这些会话bean。而实体和无状态会话bean可以看出“实施细节”，只发表本地接口。  

## EJB三种类型
EJB在一个系统中所扮演的角色：
- 封装业务逻辑(session bean)
- 访问数据库(entity bean)
- 面向消息的编程(message driven bean)
- 跟遗留系统集成(JCA:通过java可以访问非java的系统，调用者必须是java)
![EJB](企业软件开发-EJB/pic1.png)

### Session Bean(会话)
session Bean用于实现业务逻辑，它可以是有状态的，也可以是无状态的。每当客户端请求时，容器就会选择一个Session Bean来为客户端服务。Session Bean可以直接访问数据库，但更多时候会通过Entity Bean实现数据访问。  
特点：  
- 代表客户端进行操作
- 可以识别事务处理
- 存在的时间比较短，在客户端结束会话时结束  

何时使用会话Bean：  
- 只要求一个客户端访问一个Bean实例时
- 不要求Bean状态持久，而仅仅要求Bean状态存在一段时间时
- 要求用EJB实现Webservice时
![会话Bean](企业软件开发-EJB/pic2.png)
#### 无状态会话Bean
不必在方法调用之间保存客户状态，每一个无状态会话Bean实例等同于另一个无状态会话Bean
- 在EJB中是最简单的一种Bean
- 每次调用只对客户提供业务逻辑，但不保存客户端的任何数据状态
- 但并不意味着无状态类型的会话Bean没有状态，而是这些状态被保持在客户端，容器不负责管理  

生命周期:  
- 无状态会话Bean的生命周期由容器决定，Bean的客户并不实际拥有Bean的直接引用
- 无状态会话Bean只有两种状态：存在或不存在  
![hh](企业软件开发-EJB/pic15.png)  

存在的问题：  
- 数据传输负载问题：本该存储在J2EE服务器的数据被存储在客户中，每次调用这些数据都要以参数的方式传递给Bean
- 安全性问题：如果数据状态非常敏感，可以将用户状态保存到服务器而不是客户端中

#### 有状态会话Bean
实现贯穿多个方法请求和事务的商业过程，代表应用服务器单个客户并代表某个客户执行任务。即在方法调用期间，如果有状态会话Bean的状态改变，那么在接着的方法调用中，客户端会出现同样的状态。
- 一个有状态的session bean始终和一个客户相联系，在众多的方法调用过程中，这个实例代表的客户都保持一定的状态
- 也就是说有状态的session bean实例和会话对象之间有一对一的关系。同一客户端的下一个方法请求必须得到保存在Bean中的会话状态
- 有状态的session bean类的实例总是同一个对象联系在一起，并同该对象绑定在一起用来指明一个确切的客户

##### 保存在session bean中的状态通常包括：  
- 来自数据库的数据
- 由用户输入的信息

##### 生命周期：不存在、就绪和挂起  
有状态会话bean的状态信息必须序列化，因此多了钝化与激活两个中间状态。任何不被方法调用的EJB都可以钝化，除非它处于一个事务过程中。  
##### 组件池
组件池中无状态EJB可以被任意地指派给其他EJB请求，而有状态EJB却不行，因为包含了特定的与客户的会话状态信息。
![hh](企业软件开发-EJB/pic16.png)

#### 代码实例
##### 无状态会话Bean
- 创建Remote接口：展示Bean类的哪些方法可以供远程客户端访问
  - 所有的方法要声明为Public；
  - 方法声明要遵循RMI规范;
  - 方法声明要抛出java.rmi.RemoteException；
  - 方法参数及返回值是可序列化的对象。
  ![hh](企业软件开发-EJB/pic24.png)
- 创建Home接口：容器根据我们所提供的home接口生成home对象， home对象是用于对EJB对象进行管理，也就是用于创建、查找和删除EJB对象
![hh](企业软件开发-EJB/pic26.png)
- 创建Bean类
![hh](企业软件开发-EJB/pic25.png)
- 编辑部署文件
  - Ejb-jar.xml，描述EJB的结构、类型、环境参数等。
  - Weblogic-ejb-jar.xml，定义与EJB部署相关的信息。
  ![hh](企业软件开发-EJB/pic27.png)
  ![hh](企业软件开发-EJB/pic28.png)
- 编写客户端测试程序代码
  - 创建上下文环境
  - 通过上下文环境查找home对象
  - 通过home对象创建ejb对象
  - 调用ejb对象的方法，ejb对象再调用Bean实例的方法
  - 清除远程对象
![hh](企业软件开发-EJB/pic29.png)
![hh](企业软件开发-EJB/pic30.png)
![hh](企业软件开发-EJB/pic31.png)
![hh](企业软件开发-EJB/pic32.png)
![hh](企业软件开发-EJB/pic33.png)

##### 有状态会话Bean
![hh](企业软件开发-EJB/pic34.png)
![hh](企业软件开发-EJB/pic35.png)

### 实体Bean
#### 概念
Entity Bean是域模型对象，用于实现O/R映射，负责将数据库中的表记录映射为内存中的Entity对象，事实上，创建一个实体Bean对象相当于新建一条记录，删除一个实体Bean会同时从数据库中删除对应记录，修改一个实体Bean时，容器会自动将实体Bean的状态和数据库同步。
![](企业软件开发-EJB/pic18.png)
![](企业软件开发-EJB/pic19.png)
![](企业软件开发-EJB/pic20.png)

##### 持久化
在程序退出后信息可以长期保存的一种数据存储技术。主流的关系数据库存取技术有：  
- JDBC直接访问数据库
- EJB entity bean
- JDO技术(Java Data Object)
- 第三方O/R工具，如hibernate

![](企业软件开发-EJB/pic21.png)

###### 持久化类型
![](企业软件开发-EJB/pic23.png)  
- 共享访问
  - 多个客户端可共享一个实体Bean
  - 实体Bean应与事务处理同时使用
  - 事务处理属性在Bean的部署描述符中指定
- 主键
  - 每个实体Bean都由对象标识符唯一标识
- 关系
  - 实体Bean可与其他实体Bean相关联

##### 对象-关系映射(Object Relational Mapping)
将对象映射到RDBMS的技术。它能够实现内存对象同关系数据的相互转换。
- 通过硬编码实现O/R Mapping
- 借助于O/R Mapping产品，自动完成映射过程

![](企业软件开发-EJB/pic17.png)

##### 使用实体Bean的时候
- Bean应表示业务实体而非业务过程时
- Bean的状态应是持久的时候

##### 实体Bean的生命周期
![](企业软件开发-EJB/pic22.png)

#### 实体Bean分类
![实体Bean](企业软件开发-EJB/pic3.png)
#### 实体Bean的编程模型
##### Home接口
![hh](企业软件开发-EJB/pic36.png)
Home接口必须遵守以下规定：
- 类型为interface；
- 必须是public
- 必须直接或间接继承javax.ejb.EJBHome接口， EJBHome继承自java.rmi.Remote。
- 声明一个至多个finder函数，函数名称须以find开头接着名称，如findByName()。Finder函数至少包含findByPrimaryKey()，且返回类型必须是bean的remote接口。
- Create()和finder()函数丢出的异常必须包含java.rmi.RemoteException，finder()函数须另外包含javax.ejb.FinderException。
- 声明一个至多个create()函数，返回值类型必须是bean的Remote Interface。每一个create()函数都必须对应于Bean class内定义的ejbCreate()函数和ejbPostCreate()，须有相同的传入参数，且create()函数声明所丢出的异常类必须包含ejbCreate()函数内所丢出的异常类。至少要声明Create()函数和finder()函数其中一项。
- 在CMP实体bean的Home接口内，所声明的函数是用来建立一个相对于数据表记录（create）、搜寻一笔或多笔数据表记录（find）和删除一笔数据表记录（remove），就如同数据库查询语言的INSERT、SELECT和DELETE。
- 当调用create()函数时，EJB容器会调用bean类内对应的ejbCreate()，并执行数据表记录的添加，所以create()函数的传入参数至少要包含所有primary key的值。
![hh](企业软件开发-EJB/pic37.png)
- Find方法：在CMP的情况下，find方法的实现是在配置时自动生成的，不同的EJB容器供应商在定义find方法的工作方式上使用不同的策略。但无论是采用什么方式实现，在配置bean时，都需要定义find方法的规则。
- 方法findByPrimaryKey()是所有实体bean的home接口都必须支持的一个标准方法。此外，EJB允许在home接口中指定其它find方法，但其名称必须符合findlookup-type这个模式。
- 在CMP情况下，home接口中的任何find方法都要向容器解释。
- 在上述例子中，方法findByPrimaryKey()是只返回一个远程引用，而findByCapacity(int capacity)方法可以返回几个远程引用，所以其类型要选Enumeration或Collection。

##### 远程接口
Remote接口必须遵守以下规定：
- 类型为interface；
- 必须是public；
- 必须直接或间接继承javax.ejb.EJBObject。 EJBObject继承自java.rmi.Remote。
- 每一个声明在Remote接口的business函数，在Bean类内都必须有相应的实现，函数名称要一样，并具有相同的传入参数及返回值类型，且business函数声明所丢出的异常类，必须包含java.rmi.RemoteException和对应于Bean class的business函数所有丢出的异常类。
![hh](企业软件开发-EJB/pic38.png)

##### Bean类
Bean类必须遵守以下规定：
- 类型为class；
- 必须是public；
- 必须直接或间接实现javax.ejb.EntityBean接口。EntityBean继承自javax.ejb.EnterpriseBean,而EnterpriseBean继承自java.io.Serializable。
- 实现在Remote接口内声明的business函数，且名称要与Remote接口内声明的名称一样，相同的传入参数及返回值类型。
- 实现实体Bean接口定义的九个callback函数：setEntityContext()、	unsetEntityContext()、ejbCreate()、ejbPostCreate()、ejbActivate()、ejbPassivate()、ejbRemove()、ejbLoad()、ejbStore()
上述九个回调函数都要声明成public，且除了ejbCreate()函数要返回Primary key类的类型外，其余的返回值都是void。
- 根据需求定义的business methods必须在remote接口内做相对应的函数说明。这些bean类内所定义的函数都是由EJB容器调用的，客户端只能通过Home接口和remote接口内所声明的函数，来间接调用执行bean类里的函数。客户端和bean之间的通讯与互动部分，所需要的类都是在bean的部署时期，由EJB容器自动生成的。

```java
package com.titan.ship;
import javax.ejb.EntityContext;
public class ShipBean implements javax.EntityBean {
	public int id;
	public String name;
 	public int capacity;
 	public double tonnage;
	public EntityContext context;
public ShipPK ejbCreate(int id, String name, int capacity,
                            double tonnage) {
	this.id=id;
	this.name=name;
	this.capacity=capacity;
	this.tonnage=tonnage;
	return null;
}
public ShipPK ejbCreate(int id, String name) {
	this.id=id;
	this.name=name;
	this.capacity=0;
	this.tonnage=0;
	return null;
}
public void ejbPostCreate(int id, String name, int capacity,
		double tonnage) {
	ShipPK pk = (ShipPK)context.getPrimaryKey();
	//Do something useful with the primary key.
}
public void ejbPostCreate(int id, String name) {
	Ship myself = (Ship)context.getEJBOblect();
	//Do something useful with the EJBObject reference.
}
public 	void setEntityContext(EntityContext ctx) {
	context = ctx;
}
public 	void unsetEntityContext(EntityContext ctx) {
	context = null;
}
public void ejbActivate( ) {}
public void ejbpassivate( ) {}
public void ejbLoad( ) {}
public void ejbStore( ) {}
public void ejbREmove( ) {}

public String getName() {
	return name;
}
public void setName(String n) {
	name=n;
}
public int getCapacity() {
	return capacity;
}
public double getTonnage() {
	return tonnage;
}
public void setTonnage(double tons) {
	tonnage = tond;
}
}
```
#### BMP实体Bean(Bean管理持久性)
- Bean实例被客户引用，并执行一个业务方法后，容器会执行Bean的ejbStore()方法，并由这个方法把数据保存到数据库中（我们不再为Bean定义全局类变量，而是定义一些私有类变量）
- 执行完毕后Bean被钝化，并调用ejbPassviate()方法通知Bean
- 当客户过一段时间又调用这个Bean的某业务方法时，被钝化的Bean又重新的激活，但是并不是马上执行这个业务方法，而是由EJB对象首先调用ejbActivate()方法通知Bean，Bean实例要激活，然后调用Bean的ejbLoad()方法，这个方法负责从数据库中提取数据，Bean实例被初始化，最后才开始执行要执行的业务方法。
#### CMP实体Bean(容器管理持久性)
**CMP的优点：**
- Bean的状态是独立定义的，可以独立于保存bean状态的数据库定义bean。这使得实体bean有更好的重用性，在应用程序中具有更高的灵活性。
- 使用经两地映射工具定义bean的字段到数据库映射的方式，使得设计人员很容易将bean实例中的每个字段映射到数据库中的字段上或将bean序列化到一个文件中。

**CMP Bean的主键：**
- 只有EntityBean有主键，Session调用主键方法将抛出一个异常。EntityBean是数据面向对象的表示，每个Bean的实例代表一行记录，所以就必须有一个主键来标识这个对象，以能够对其进行持久性操作 。
- CMP Bean由容器来负责实例的生成、装入、寻找、更新、删除等，所以主键也由容器来控制。
- 对于CMP Bean,javax.ejb.EJBObject类已经为我们定义了一个默认的构造方 法public abstract Object getPrimaryKey()，并且不需要我们再为其改造。
- 主键类型一般对应于数据表主关键字类型，CMP Bean 的主键是在部署者部署Bean 时被指定的

### 消息驱动Bean
MessageDriven Bean是EJB2.0中引入的新的企业Bean，它基于JMS消息，只能接收客户端发送的JMS消息然后处理。MDB实际上是一个异步的无状态Session Bean，客户端调用MDB后无需等待，立刻返回，MDB将异步处理客户请求。这适合于需要异步处理请求的场合，比如订单处理，这样就能避免客户端长时间的等待一个方法调用直到返回结果。
**MDB的特点:**
- 没有home/local home以及remote/local接口,只有一个bean类;
- 只有一个商业方法onMessage方法,MDB的bean类实现了MessageListener和MessageDrivenBean接口,MessageListener接口中定义了这个方法;
- 没有任何返回值;
- 不能抛异常给消息的生产者,在onMessage方法中只能抛出系统级别的异常,不能抛出应用级别的异常;
- MDB是无状态的,所以可以引入pool机制提高消息处理的效率;
- 可以是持久的或则非持久的订阅者.

![消息驱动](企业软件开发-EJB/pic4.png)
#### JMS
##### JMS简介
Java消息服务 (Java Message Service，简称JMS)是一个Java API, 是Sun推出的用于统一不同消息服务和消息调用接口的标准。定义客户端如何与底层的消息服务提供者之间进行交互.通过队列服务器形式提供。
**消息类型**
- PTP(点到点)消息队列
这个消息队列可以同时有多个发送者，每个发送者可以自由的向当前队列发送消息。被发送的消息按照先发先进的原则依次排列在队列中。只能有一个接收者
- Pub/Sub（发布/订阅）消息队列
所有发送者可以自由地向消息队列中发送消息，首先发送的消息先进入队列，后发送的消息排在队列后面。一个消息可以被多个使用者接受并使用。默认情况下，如果当前队列没有使用者，则队列中的消息会自动被丢失。

**JMS的应用组成**
- 消息发送者(消息生产者)
- JMS提供者
- 消息接收者(消息消费者)
- 管理对象
- 消息
  - 组成：消息头、属性、消息体
  - 消息类型:StreamMessage、MapMessage、TextMessage、ObjectMessage、BytesMessage、XMLMessage

**消息传递方式**
- 非持久性模式
- 持久性模式

##### JMS的使用
![消息驱动](企业软件开发-EJB/pic39.png)
**JMS相关API**
- 连接工厂（ConnectionFactory）：
创建一个管理对象，由服务器的管理员创建，并绑定到JNDI树上。客户端使用JNDI检索ConnectionFactory，然后利用它建立一个JMS连接。
- 连接(Connection)
代表一个与JMS提供者的活动连接
- 目的(Destination)
标识消息的接收方式，实际的消息源和消息存储位置
- 会话(Session)
表示客户端与JMS服务器之间的会话状态。
- 消息生产者(MessageProducer)
由Session创建的用于将消息发送到目的的一个对象
- 消息消费者(MessageConsumer)
由Session创建的用于从目的接收消息的一个对象

**发送消息的过程**
- 获得一个WebLogic Server上下文的引用；
- 使用JNDI查询管理对象ConnectionFactory和Destination
- 使用管理对象ConnectionFactory建立连接Connection
- 使用Connection建立会话Session
- 使用Session和Destination创建消息生产者MessageProducer 。
- 使用连接创建一个需要发送的消息类型的实例；
- 使用消息生产者MessageProducer发送消息
