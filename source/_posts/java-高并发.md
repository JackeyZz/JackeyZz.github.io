---
title: java_高并发
date: 2018-03-11 00:51:53
tags: [java,并发]
categories: [java整理总结]
---
## 并发_基本的线程机制
### 定义任务
线程驱动任务，故可以使用`Runnable接口`提供一种描述任务的方式，其中`run()方法`通常被写成无限循环或者某种形式的循环。
```
hh
```
### Thread类
`Thread`定义一个线程去驱动继承`Runnable接口`的任务类，调用`Thread`对象的`start()`方法为该线程执行必需的初始化操作，程序就会自动去调用`Runnable`的`run()`方法，以便在这个新线程中启动该任务。其中`start()`会迅速返回到`main`线程。因此程序是多个线程"同时"执行的。
当然多次执行的结果可能会不一样，这是因为线程调度机制是非确定性的。
```
hh
```
### 使用Executor
`java.util.concurrent`包中的执行器`Executor`将会管理`Thread对象`,从而简化了并发编程。`ExecutorService`(具有服务生命周期的`Executor`，例如关闭)知道如何构建恰当的上下文来执行`Runnable接口`。
- `ExecutorService exec=Executors.newCachedThreadPool()`
- `ExecutorService exec=Executors.newFixedThreadPool(5)`
- `ExecutorService exec=Executors.newSingleThreadExecutor()`  

执行`exec.execute(new 任务)`启动执行器，执行`exec.shutdown()`可以防止新任务被提交给这个`Executor`，当前线程(本例中即驱动`main()`的线程)将继续运行在`shutdown()`被调用之前提交的所有任务。
```
hh
```
### 从任务中产生返回值
使用`Callable接口`，一种具有类型参数的泛型，它的类型参数表示的是从方法`call()`中返回的值，并且必须使用`ExecutorService.submit()`方法调用它。而`submit()`方法会产生`Feature`对象，它用`Callable`返回结果的特定类型进行了参数化，你可以用`isDone()`方法来查询`Feature`是否已经完成。当任务完成时，它具有一个结果，你可以调用`get()`方法来获取该结果。你也可以不用`isDone()`进行检查就直接调用`get()`，在这种情况下，`get()`将阻塞直至结果准备就绪。
```
hh
```
### 休眠
影响任务行为的一种简单方法就是调用`sleep()`，这将使任务中止执行给定的时间。对`sleep()`的调用可以抛出`InterruptedException`异常，并在`run()`中被捕获。
```
hh
```
### 优先级
