---
layout: post
title: C#中 Thread、ThreadPool、Task的区别
date: 2020-08-17 10:11:55
categories: C#
tag: C# 基础
---

**这三者都是为了处理耗时任务，都是异步的；**

> **Task适合用于多处理器，且i系列多处理器。**
>
> **Thread则适用于所有的处理器，实时性更高。**

先说 Thread与ThreadPoll

前台线程：主程序必须等待线程执行完毕后才可退出程序。**Thread默认为前台线程**，也可以设置为后台线程

后台线程：主程序执行完毕后就退出，不管线程是否执行完毕。**ThreadPool默认为后台线程**

线程消耗：开启一个新线程，线程不做任何操作，都要消耗1M左右的内存

ThreadPoll是线程池 其目的是为了减少开启新线程消耗的资源(使用线程池中的空闲线程，不必在开启新线程，以及统一管理线程(线程池中的线程执行完毕后，回归到线程池里，等待新任务).

**总结：ThreadPoll性能优于Thread，但是Thread和ThreadPoll对线程的控制都不是很好，例如线程等待(线程执行一段时间无响应后，直接停止线程，释放资源 等 都没有直接的API来控制 只能通过硬编码来实现，同时ThreadPool使用的是线程池全局队列，全局队列中的线程依旧会存在竞争共享资源的情况，从而影响性能。****

**Task的背后的实现也是使用了线程池线程**，但它的性能优于ThreadPoll,因为它使用的不是线程池的全局队列，而是使用的本地队列，使线程之间的资源竞争减少。同时Task提供了丰富的API来管理线程、控制。但是相对前面的两种耗内存，Task依赖于CPU对于多核的CPU性能远超前两者，单核的CPU三者的性能没什么差别。

下面我们来写个简单的代码

**创建Task有两种方法：**

```C#
Task t = Task.Factory.StartNew(() => {
    Console.WriteLine("任务已启动....");
});
```

或

```C#
Task t2 = new Task(() => {
    Console.WriteLine("开启一个新任务");
});
t2.Start();//任务已启动...
```

第一种方法不需要调用start 初始化后任务就开始了。

**Task还可以随时取消正在执行的任务:**

```C#
static void Main(string[] args)

{
    CancellationTokenSource cts = new CancellationTokenSource();
    Task t3 = new Task(() => LongRunTask(cts.Token));
    t2.Start();
    Thread.Sleep(3000);
    cts.Cancel();
    Console.Read();
}

private static void LongRunTask(CancellationToken token)
{
    while (true)
    {
        if (!token.IsCancellationRequested)
        {
            Thread.Sleep(500);
            Console.WriteLine(".");
        }
        else
        {
            Console.WriteLine("任务取消了");
            break;
        }
    }
}
```

**任务启动任务  当一个任务完成后 开启一个新的任务**

```C#
Task task = new Task(() => { Thread.Sleep(5000); Console.WriteLine("Hello，"); Thread.Sleep(5000); });
task.Start();
Task newTask = task.ContinueWith(t => Console.WriteLine("开启了另一个任务"));
```

对于ContinueWith方法，我们可以配合TaskContinuationOptions枚举，得到更多我们想要的行为。

```C#
Task parant = new Task(() =>
{
    new Task(() => Console.WriteLine("1")).Start();
    new Task(() => Console.WriteLine("2")).Start();
    new Task(() => Console.WriteLine("3")).Start();
    new Task(() => Console.WriteLine("4")).Start();
});
parant.Start();
```

值得注意的是，以上代码中所示的子任务的调用并不是以代码的出现先后为顺序来调用的。

#### 任务工厂

在某些情况下，我们会创建大量的任务来满足业务，而恰好这些任务公用某个状态参数，为了避免大量的调用任务的构造器和一次又一次的传递参数，我们可以使用任务工厂来处理这种大量的创建任务，如下代码：

```C#
Task parent = new Task(() => 
    { 
        CancellationTokenSource cts = new CancellationTokenSource(); 
        TaskFactory tf = new TaskFactory(cts.Token); 
        var childTask = new[] 
        { 
            tf.StartNew(()=>ConcreteTask(cts.Token)), 
            tf.StartNew(()=>ConcreteTask(cts.Token)), 
            tf.StartNew(()=>ConcreteTask(cts.Token)) 
        };
        Thread.Sleep(5000);//此处睡眠等任务开始一定时间后才取消任务 
        cts.Cancel(); 
    } 
);
parent.Start();//开始执行任务 
```





---

---

## 个人信息：
### 姓名：邹建
### 就读于： 四川工程职业技术学院
### 2019年加入 软件工作室(DS)
### QQ：451991189
### 个人博客：https://unitysir.github.io/
### B站ID：308511666

## 如果内容对你有所帮助：
![wx](https://pic4.zhimg.com/v2-87fbc8ee6ab3fd92f423d414d039b627_b.jpeg)

![zfb](https://pic2.zhimg.com/v2-b8ab4acf7899b2ced11287cdbd8279b5_b.jpeg)