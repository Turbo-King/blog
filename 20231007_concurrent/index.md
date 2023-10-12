# 并发编程


<!--more-->

## 引入

{{< admonition >}}
一个 **注意** 横幅
{{< /admonition >}}

{{< admonition abstract >}}
一个 **摘要** 横幅
{{< /admonition >}}

{{< admonition info >}}
一个 **信息** 横幅
{{< /admonition >}}

{{< admonition tip >}}
一个 **技巧** 横幅
{{< /admonition >}}

{{< admonition success >}}
一个 **成功** 横幅
{{< /admonition >}}

{{< admonition question >}}
一个 **问题** 横幅
{{< /admonition >}}

{{< admonition warning >}}
一个 **警告** 横幅
{{< /admonition >}}

{{< admonition failure >}}
一个 **失败** 横幅
{{< /admonition >}}

{{< admonition danger >}}
一个 **危险** 横幅
{{< /admonition >}}

{{< admonition bug >}}
一个 **Bug** 横幅
{{< /admonition >}}

{{< admonition example >}}
一个 **示例** 横幅
{{< /admonition >}}

{{< admonition quote >}}
一个 **引用** 横幅
{{< /admonition >}}









## 进程与线程

### 介绍

`进程`

- 程序由指令和数据组成，但这些指令要运行，数据要读写，就必须将指令加载至 CPU ，数据加载至内存。在指令运行过程中还需要用到磁盘、网络等设备。进程就是用来加载指令、管理内存、管理 IO 的。
- 当一个程序被运行，从磁盘加载这个程序的代码至内存，这时就开启了一个进程。
- 进程就可以视为程序的一个实例。大部分程序可以同时运行多个实例进程（例如记事本、画图、浏览器等），也有的程序只能启动一个实例进程（例如网易云音乐、360安全卫士等）

`线程`

- 一个进程之内可以分为一到多个线程。
- 一个线程就是一个指令流，将指令流中的一条条指令以一定的顺序交给CPU执行。
- Java中，线程作为最小调度单位，进程作为资源分配的最小单位。在 Windows 中进程是不活动的，只是作为线程的容器。

{{< admonition info 线程与进程对比>}}

1. 进程基本上相互独立的，而线程存在于进程内，是进程的一个子集

2. 进程拥有共享的资源，如内存空间等，供其内部的线程共享

3. 进程间通信较为复杂

    - 同一台计算机的进程通信称为`IPC`（Inter-process communication）

    - 不同计算机之间的进程通信，需要通过网络，并遵循共同的协议，例如 HTTP

4. 线程通信相对简单，因为他们共享进程内的内存。例如：多个线程可以访问同一个共享变量

5. 线程更轻量，线程上下文切换成本一般上要比进程上下文切换低

{{< /admonition >}}



### 并行与并发

单核 CPU 下，线程实际还是`串行执行`的，操作系统中有一个组件叫做**任务调度器**，将 CPU 的时间片（Windows 下时间片最小约为15毫秒）分给不同的程序使用，只是由于 CPU 在线程间（时间片很短）的切换非常快，人类感觉是`同时运行的`。总结一句话就是：`微观串行，宏观并行`，一般会将这种`线程轮流使用 CPU `的做法称为**并发**，**concurrent**

| CPU  | 时间片1 | 时间片2 | 时间片3 | 时间片4 |
| :--: | :-----: | :-----: | :-----: | :-----: |
| core |  线程1  |  线程2  |  线程3  |  线程4  |

![并发](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%B9%B6%E5%8F%91.jpg "并发")

<br>

多核CPU下，每个`核（core）`都可以调度运行线程，这时候线程可以是并行的

|  CPU  | 时间片1 | 时间片2 | 时间片3 | 时间片4 |
| :---: | :-----: | :-----: | :-----: | :-----: |
| Core1 |  线程1  |  线程2  |  线程3  |  线程4  |
| Core2 |  线程1  |  线程2  |  线程3  |  线程4  |

![并行](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%B9%B6%E8%A1%8C.jpg "并行")

<br>

>引用**Rob Pike**的一段描述：
>
>- **并发（concurrent）**是同一时间应对（dealing with）多件事情的能力
>- **并行（parallel）**是同一时间动手做（doing）多件事情的能力
>
>**用例**
>
>- 家庭主妇做饭，打扫卫生，给孩子喂奶，她一个人轮流交替做这么多件事，这时就是并发
>- 家庭主妇雇佣了个保姆，她们一起做这些事，这时既有并发，也有并行（这时会产生竞争，例如锅只有一口，一个人用锅时，另一个人就得等待）
>- 雇了三个保姆，一个专做饭，一个专打扫卫生，一个专喂奶，互不打扰，这时就是并行
>
>**Rob Pike** 资料
>
>- Golang语言的创造者
>- [Rob Pike - 百科百度](https://baike.baidu.com/item/羅布·派克/10983505)

### 应用

#### 应用之异步调用

以调用角度来讲，如果

- 需要等待结果返回，才能继续运行就是同步
- 不需要等待结果返回，就能继续运行就是异步

<br>

**设计**

多线程可以让方法执行变为异步的（既不要巴巴干等着）比如说读取磁盘文件时，假设读取操作话费了5分钟，如果没有线程调度机制，这5秒CPU什么都做不了，其它代码都得暂停...

<br>

**结论**

- 比如在项目中，视频文件需要转换格式等操作比较费时，这时开启一个新线程处理视频转换，避免阻塞主线程
- tomcat的异步servlet也是类似的目的，让用户线程处理耗时较长的操作，避免阻塞tomcat的工作线程
- UI程序中，开线程进行其他操作，避免阻塞UI线程

<br>

#### 应用之提高效率

充分利用多核CPU的优势，提高运行效率，想象下面的场景，执行3个计算，最后将计算结果汇总

```bash
计算 1 花费 10 ms

计算 2 花费 11 ms

计算 3 花费 9 ms

汇总需要 1 ms
```

- 如果是串行执行，那么总共花费的时间是`10 + 11 + 9 + 1 = 31ms`
- 但如果是四核CPU，各个核心分别使用线程1执行计算1，线程2执行计算2，线程3执行计算3，那么3个线程是并行的，花费时间只取决于最长的那个线程运行的时间，即`11ms`最后加上汇总时间只会花费`12ms`

>**注意**
>
>需要在多核CPU才能提高效率，单核仍然是轮流执行

<br>

**结论**

1. 单核CPU下，多线程不能实际提高程序运行效率，只是为了能够在不同的任务之间切换，不同线程轮流使用CPU，不至于一个线程总占用CPU，别的线程没法干活
2. 多核CPU可以并行跑多个线程，但能否提高程序运行效率还是要分情况的
    - 有些任务，经过精心设计，将任务拆分，并行执行，当然可以提高程序的运行效率，但不是所有计算任务都能拆分（参考[阿姆达尔定律](https://baike.baidu.com/item/阿姆达尔定律?fromModule=lemma_search-box)）
    - 也不是所有任务都需要拆分，任务的目的如果不同，谈拆分和效率没啥意义
3. IO操作不占用CPU，只是我们一般拷贝文件使用的是`阻塞IO`，这时相当于线程虽然不用CPU，但需要一直等待IO结束，没能充分利用线程。所以才用后面的`非阻塞IO`和`异步IO`优化

<br>

## Java线程

### 创建和运行线程

#### 方法一 **Thread**

{{< admonition example Thread使用步骤>}}

1. 创建一个继承于Thread类的子类

2. 重写Thread类的run() ——> 将此线程执行的操作声明在run()中
3. 创建Thread类的子类的对象
4. 通过此对象调用start()执行线程

{{< /admonition >}}

**创建模版**

```java
// 创建线程对象 
Thread t = new Thread() {
	public void run() {
	// 要执行的任务
	} 
}; 
// 启动线程 
t.start();
```

**实际应用示例**

```java
// 构造方法的参数是给线程指定名字，推荐 
Thread t1 = new Thread("t1") {
	@Override 
  // run 方法内实现了要执行的任务
  public void run() { 
    log.debug("hello"); 
  }
}; t1.start();
```

**输出**

```bash
19:19:00 [t1] c.ThreadStarter - hello
```

<br>

#### 方法二 **Runable**

{{< admonition example Runable使用步骤>}}

1. 创建一个实现了Runnable接口的类
2. 实现类去实现Runnable中的抽象方法：run()
3. 创建实现类的对象
4. 将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
5. 通过Thread类的对象调用start()
    - 启动线程
    - 调用当前线程的run()——>调用了Runnable类型的target的run()

{{< /admonition >}}

把线程和任务（需要执行的代码）分开

- Thread 代表线程
- Runable 可运行的任务（线程需要执行的代码）

**创建模版**

```java
Runnable runnable = new Runnable() { 
  public void run(){ 
    // 要执行的任务 
  } 
}; 
// 创建线程对象 
Thread t = new Thread( runnable ); 
// 启动线程 
t.start();
```

**实际应用示例**

```java
// 创建任务对象 
Runnable task2 = new Runnable() { 
  @Override public void run() { 
    log.debug("hello"); 
  } 
};

// 参数1 是任务对象; 参数2 是线程名字，推荐 
Thread t2 = new Thread(task2, "t2"); 
t2.start();
```

**输出**

```java
19:19:00 [t2] c.ThreadStarter - hello
```

<br>

Java 8以后可以使用 Lambda 精简代码

```java
// 创建任务对象 
Runnable task2 = () -> log.debug("hello");

// 参数1 是任务对象; 参数2 是线程名字，推荐 
Thread t2 = new Thread(task2, "t2"); 
t2.start();
```

<br>

**Thread 与 Runnable 的关系**

- 方法一是把线程和任务合并在了一起，方法二是把线程和任务分开了
- 用 Runable 更容易与线程池等高级 API 配合
- 用 Runable 让任务类脱离了 Thread 继承体系，更灵活

<br>

#### 方法三 **Callable**

{{< admonition example FutureTask使用步骤>}}

1. 创建一个实现 Callable 的实现类
2. 实现 call() 方法，将此线程需要执行的操作声明在 call() 中
3. 创建 Callable 接口实现类的对象
4. 将此 Callable 接口实现类的对象作为传递到 FutureTask 构造器中，创建 FutureTask 的对象
5. 将 FutureTask 的对象作为参数传递到 Thread 类的构造器中，创建 Thread 对象，并调用 start()
6. 获取 Callable 中 call() 方法的返回值

{{< /admonition >}}

**实现Callable接口的方式创建线程的强大之处**

- call() 可以有返回值的
- call() 可以抛出异常，被外面的操作捕获，获取异常的信息
- Callable 是支持泛型的

**实际应用示例**

```java
// 创建任务对象
FutureTask<Integer> task3 = new FutureTask<>(() -> {
	log.debug("hello");
  return 100;
});

// 参数1 是任务对象; 参数2 是线程名字，推荐 
new Thread(task3, "t3").start();

// 主线程阻塞，同步等待 task 执行完毕的结果 
Integer result = task3.get(); 
log.debug("结果是:{}", result);
```

**输出**

```bash
19:22:27 [t3] c.ThreadStarter - hello 
19:22:27 [main] c.ThreadStarter - 结果是:100
```




