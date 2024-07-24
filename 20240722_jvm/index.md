# JVM 原理拙见


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

<!--more-->

## 概念

`JVM` 是 Java 的核心和基础，处于 Java 编译器和 OS 平台之间的虚拟处理器。它是一种利用软件方法实现的抽象的计算机基于下层的操作系统和硬件平台，可以在上面执行 Java 的字节码程序。Java 编译器只要面向 `JVM`，生成 `JVM` 能够理解的代码或字节码文件。Java 源文件经编译成字节码程序，通过 `JVM` 将每一条指令翻译成不同平台机器码，通过特定的平台运行。

<br>

## JVM的生命周期

1. **JVM 的诞生**：当启动一个 Java 程序时，一个 JVM 实例就产生了，任何一个拥有`public static void main(String[] args)`函数的 class 都可以作为 JVM 实例运行的起点。
2. **JVM 实例的运行**：main() 作为该程序初始线程的起点，任何其他线程均由该线程启动。JVM 内部有两种线程：**守护线程**和**非守护线程**，main() 属于非守护线程，守护线程通常由 JVM 自己使用，Java 程序也可以标明自己创建的线程是守护线程。
3. **JVM 实例的消亡**：当程序中的所有非守护线程都终止时，JVM 才退出；若安全管理器允许，程序也可以使用 java.lang.Runtime 类或者 java.lang.System.exit() 来退出。

<br>

## JVM运行内存模型

概念：JVM 把文件 .class 字节码加载到内存，对数据进行校验，转换和解析，并初始化，最终形成可以被虚拟机直接使用的 Java 类型，这就是虚拟机的类加载机制。

![类加载机制](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E7%B1%BB%E5%8A%A0%E8%BD%BD%E6%9C%BA%E5%88%B6.jpeg "类加载机制")

<br>

### 类装载器

> 类加载器的任务是根据一个类的全限定名来读取此类的二进制字节流到 JVM 中，然后转换为一个与目标对应的 java.lang.Class 对象实例

<br>

{{< admonition tip 虚拟机提供的四类类加载机制>}}

-  **启动类加载器（Bootstrap ClassLoader）** C++ 编写，加载 Java 核心 java.*
    - 这个类加载器负载放在 <JAVA_HOME>/lib 目录中的，或者被 -Xbootclasspath 参数所指定的路径中，并且是虚拟机识别的类库。用户无法直接使用。
    - 由于引导类加载器涉及到虚拟机本地实现细节，开发者无法直接获取到启动类加载器的引用，所以不允许直接通过引用进行操作。
- 扩展类加载器（Extend加）

{{< /admonition >}}










