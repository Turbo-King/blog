---
weight: 5
title: "设计模式"
date: 2022-12-05T08:53:36+08:00
draft: false
author: "Turbo-King"
authorLink: "https://turbo-king.github.io/"
description: "设计模式讲解"
featuredImage : "https://cdn.jsdelivr.net/gh/Turbo-King/images/featured-image20221205.jpg"

tags: ["Java","coding","principle"]
categories: ["Markdown"]

lightgallery: true
---

# 浅谈设计模式

<!--more-->

## 前言

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

> 编写软件过程中，程序员面临着来自**耦合性**，**内聚性**以及**可维护性**，**可扩展性**，**重用性**，**灵活性** 等多方面的挑战，设计模式是为了让程序(软件)，具有更好：
>
> 1. **代码重用性**（即：相同的代码，不用多次编写）
> 2. **可读性**（即：编程规范性，便于其他程序员的阅读与理解）
> 3. **可扩展性**（即：当需要增加新的功能时，非常方便，称为可维护性）
> 4. **可靠性**（即：当我们增加新的功能后，对原来的功能没有影响）
> 5. 使程序呈现**高内聚，低耦合**的特性

设计模式包含了面向对象的精髓：“懂了设计模式，你就懂了面向对象分析和设计（OOA/D）的精要”。

Scott Mayers 在其巨著《Effective C++》就曾经说过：C++老手和 C++新手的区别就是前者手背上有很多伤疤。

<br><br><br>

## 设计模式七大原则

设计模式原则，其实就是程序员在编程时，应当遵守的原则，也是各种设计模式的基础（即：设计模式为什么这样设计的依据）

{{< admonition abstract 设计模式常用的七大原则 >}}

1. **单一职责原则**
2. **接口隔离原则**
3. **依赖倒转（倒置）原则**
4. **里氏替换原则**
5. **开闭原则**
6. **迪米特法则**
7. **合成服用原则**

{{< /admonition >}}



### 单一职责原则

#### 介绍

对类来说，一个类应该只负责一项职责。如类A负责两个不同的职责：职责1，职责2。当职责1需求变更而改变A时，可能造成职责2执行错误，所以需要将类A的粒度分解为A1，A2。类图如下所示

![单一职责.png](https://cdn.jsdelivr.net/gh/Turbo-King/images/单一职责.png.jpeg "单一职责原则")



#### 单一职责原则注意事项和细节

{{< admonition tip 单一职责原则 >}}

1. 降低类的复杂度，一个类只负责一项职责。
2. 提高类的可读性，可维护性。
3. 降低变更引起的风险。
4. 通常情况下，我们应当遵守单一职责原则，只有逻辑足够简单，才可以在代码级违反单一职责原则；只有类中的方法数量足够少，可以在方法级别保持单一职责原则。

{{< /admonition >}}



### 接口隔离原则

#### 介绍

客户端不应该依赖他不需要的接口，即对类对另一个类的依赖应该建立在最小的接口上。

![截屏2022-12-05 14.40.01](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2022-12-05%2014.40.01.png "接口隔离原则-未拆分")

类A通过接口Interface1依赖类B，类C通过Interface1依赖类D，如果接口Interface1对于类A和类C来说不是最小接口，那么类B和类D必须去实现他们不需要的方法。

**隔离原则应当如下处理：**

将接口Interface1拆分成为几个独立的接口（这里我们拆分成为3个接口），类A和类C分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则。



#### 使用接口隔离原则改进

{{< admonition example  步骤>}}

1. 类A通过接口Interface1依赖类B，类C通过接口Interface1依赖类D，如果接口Interface1对于类A和类C来说不是最小接口，那么类B和类D必须去实现他们不需要的方法
2. 将接口Interface1拆分为独立的几个接口，类A和类C分别与他们需要的接口建立依赖关系。也就是采用接口隔离原则
3. 接口Interface1中出现的方法，根据实际情况拆分为三个接口

{{< /admonition >}}

拆分实现如下：

![截屏2022-12-05 14.56.50](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2022-12-05%2014.56.50.png "接口隔离原则-拆分")

```java
public class Segregation {
    public static void main(String[] args) {
        A a = new A();
        a.depend1(new B()); // A类通过接口去依赖B类
        a.depend2(new B());
        a.depend3(new B());

        C c = new C();
        c.depend1(new D()); // C类通过接口去依赖D类
        c.depend4(new D());
        c.depend5(new D());

    }
}

interface Interface1 {
    void operation1();
}

interface Interface2 {
    void operation2();

    void operation3();
}

interface Interface3 {
    void operation4();

    void operation5();
}

class D implements Interface1, Interface3 {

    @Override
    public void operation1() {
        System.out.println("D 实现了 operation1");
    }

    @Override
    public void operation4() {
        System.out.println("D 实现了 operation4");

    }

    @Override
    public void operation5() {
        System.out.println("D 实现了 operation5");

    }
}

class B implements Interface1, Interface2 {

    @Override
    public void operation1() {
        System.out.println("B 实现了 operation1");
    }

    @Override
    public void operation2() {
        System.out.println("B 实现了 operation2");

    }

    @Override
    public void operation3() {
        System.out.println("B 实现了 operation3");

    }
}

// A类通过接口Interface依赖使用B类，用到了1，2，3方法
class A {
    public void depend1(Interface1 interface1) {
        interface1.operation1();
    }

    public void depend2(Interface2 interface2) {
        interface2.operation2();
    }

    public void depend3(Interface2 interface2) {
        interface2.operation3();
    }
}

// C类通过接口Interface依赖D类，用到了1，4，5方法
class C {
    public void depend1(Interface1 interface1) {
        interface1.operation1();
    }

    public void depend4(Interface3 interface3) {
        interface3.operation4();
    }

    public void depend5(Interface3 interface3) {
        interface3.operation5();
    }
}
```



### 依赖倒转原则

#### 介绍

依赖倒转原则(Dependence Inversion Principle)是指：

1. 高层模块不应该依赖底层模块，二者都应该依赖其抽象

2. 抽象不应该依赖细节，细节应该依赖抽象

3. 依赖倒转(倒置)的中心思想是面向接口编程

4. 依赖倒转原则是基于这样的设计原理：相较于细节的多变性，抽象的东西要稳定的多。以抽象为基础搭建的框架比以细节为基础的框架要稳定的多。在Java中，抽象指的是借口或抽象类，细节就是具体的实现类

5. 使用接口或抽象类的目的是制定好规范，而不涉及任何具体的操作，把展现细节的任务交给他们的实现类去完成

    ![依赖倒转](https://cdn.jsdelivr.net/gh/Turbo-King/images/24a4bef88c92f6af9dbf584f9c382456_r-20230227093317943.jpg "依赖倒转")

    

{{< admonition abstract 依赖关系传递的三种方式 >}}

1. 接口传递
2. 构造方法传递
3. setter方式传递

{{< /admonition >}}

```java
public class DependencyPass {

    public static void main(String[] args) {
        // TODO Auto-generated method stub

    }

}

// 方式1： 通过接口传递实现依赖
// 开关的接口
interface IOpenAndClose {
    public void open(ITV tv); //抽象方法,接收接口
}

interface ITV { //ITV接口
    public void play();
}

// 实现接口
class OpenAndClose implements IOpenAndClose {
    public void open(ITV tv) {
        tv.play();
    }
}

// 方式2: 通过构造方法依赖传递
interface IOpenAndClose {
    public void open(); //抽象方法
}

interface ITV { //ITV接口
    public void play();
}

class OpenAndClose implements IOpenAndClose {
    public ITV tv;

    public OpenAndClose(ITV tv) {
        this.tv = tv;
    }

    public void open() {
        this.tv.play();
    }
}

// 方式3 , 通过setter方法传递
interface IOpenAndClose {
    public void open(); // 抽象方法

    public void setTv(ITV tv);
}

interface ITV { // ITV接口
    public void play();
}

class OpenAndClose implements IOpenAndClose {
    private ITV tv;

    public void setTv(ITV tv) {
        this.tv = tv;
    }

    public void open() {
        this.tv.play();
    }
}
```

#### 依赖倒转原则的注意事项和细节

{{< admonition tip 依赖倒转原则 >}}

1. 底层模块尽量都要有抽象类或接口，或者两者都有，程序稳定性更好
2. 变量的声明类型尽量是抽象类或接口，这样我们的变量引用和实际对象间，就存在一个缓冲层，利于程序扩展和优化
3. 继承时遵循里氏替换原则

{{< /admonition >}}

### 里氏替换原则

{{< admonition question OO中的继承性的思考和说明 >}}

1. 继承包含这样一层含义：父类中凡是已经实现好的方法，实际上是在设定规范和契约，虽然他不强制要求所有的子类必须遵循这些契约，但是如果自类对这些已经实现的方法随意修改，就会对整个继承体系造成破坏
2. 继承在给程序设计带来便利的同时，也带来了弊端。比如使用继承会给程序带来侵入性，程序的可移植性降低，增加对象的耦合性，如果一个类被其他的类所继承，则当这个类需要修改时，必须考虑到所有的子类，并且父类修改后，所有涉及到子类的功能都有可能产生故障

{{< /admonition >}}

#### 介绍

1. 里氏替换原则(Liskov Substitution Principle)在1988年，由麻省理工学院里一位姓里的女士提出的

2. 如果对每个类型为T1的对象o1，都有类型为T2的对象o2，使得以T1定义的所有程序P在所有的对象o1都替换成o2时，程序P的行为没有发生变化，那么类型T2是类型T1的子类型。换句话说，所有的引用基类的地方必须能透明地使用其子类的对象
3. 在使用继承，遵循里氏替换原则，在**子类中尽量不要重写父类的方法**
4. 里氏替换原则告诉我们，继承实际上让两个类耦合性增强了，在适当的情况下，可以通过**耦合、组合、依赖**来解决问题

![里式替换原则](https://cdn.jsdelivr.net/gh/Turbo-King/images/111901060-b86c2f00-8a70-11eb-99fb-f28d28c62cb4.png "里氏替换")

**里氏替换原则通用做法：原来的父类和子类都继承一个更通俗的基类，原有的继承关系去掉，采用依赖、聚合、组合等关系替代。**



### 开闭原则

#### 介绍

1. 开闭原则(Open Closed Principle)是编程中最基础、最重要的设计原则
2. 一个软件实体如类，模块和函数应该对扩展开放(对提供方)，对修改关闭(对使用方)。用抽象构建框架，用实现扩展细节
3. 当软件需要变化时，尽量通过扩展软件实体的行为来实现变化，而不是通过修改已有的代码来实现变化
4. 编程中遵循其他原则，以及使用设计模式的目的就是遵循开闭原则

![代码演示](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-02%2021.09.58.png "类图演示")



```Java
public class Ocp {

	public static void main(String[] args) {
		//使用看看存在的问题
		GraphicEditor graphicEditor = new GraphicEditor();
		graphicEditor.drawShape(new Rectangle());
		graphicEditor.drawShape(new Circle());
		graphicEditor.drawShape(new Triangle());
	}

}

//这是一个用于绘图的类 [使用方]
class GraphicEditor {
	//接收Shape对象，然后根据type，来绘制不同的图形
	public void drawShape(Shape s) {
		if (s.m_type == 1)
			drawRectangle(s);
		else if (s.m_type == 2)
			drawCircle(s);
		else if (s.m_type == 3)
			drawTriangle(s);
	}

	//绘制矩形
	public void drawRectangle(Shape r) {
		System.out.println(" 绘制矩形 ");
	}

	//绘制圆形
	public void drawCircle(Shape r) {
		System.out.println(" 绘制圆形 ");
	}
	
	//绘制三角形
	public void drawTriangle(Shape r) {
		System.out.println(" 绘制三角形 ");
	}
}

//Shape类，基类
class Shape {
	int m_type;
}

class Rectangle extends Shape {
	Rectangle() {
		super.m_type = 1;
	}
}

class Circle extends Shape {
	Circle() {
		super.m_type = 2;
	}
}

//新增画三角形
class Triangle extends Shape {
	Triangle() {
		super.m_type = 3;
	}
}
```



#### 演示方式优缺点

1. 代码易于理解，操作简单
2. 缺点是违反了设计模式的OCP原则，即**对扩展开放（提供方），对修改关闭（适用方）**。即当我们给类增加新功能的时候，尽量不修改代码，或者尽可能少修改代码
3. 比如我们这时要新增加一个图形种类三角形，我们需要修改较多地方



{{< admonition example 优化分析 >}}

把创建Shape类做成抽象类，并提供一个抽象的draw方法，让子类去实现即可，这样我们有新的图形种类时，只需要让新的图形类继承Shape，并实现draw方法即可。（使用方的代码就不需要修改，即可满足开闭原则）

{{< / admonition >}}

```Java
public class Ocp {

	public static void main(String[] args) {
		//使用看看存在的问题
		GraphicEditor graphicEditor = new GraphicEditor();
		graphicEditor.drawShape(new Rectangle());
		graphicEditor.drawShape(new Circle());
		graphicEditor.drawShape(new Triangle());
		graphicEditor.drawShape(new OtherGraphic());
	}

}

//这是一个用于绘图的类 [使用方]
class GraphicEditor {
	//接收Shape对象，调用draw方法
	public void drawShape(Shape s) {
		s.draw();
	}

	
}

//Shape类，基类
abstract class Shape {
	int m_type;
	
	public abstract void draw();//抽象方法
}

class Rectangle extends Shape {
	Rectangle() {
		super.m_type = 1;
	}

	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制矩形 ");
	}
}

class Circle extends Shape {
	Circle() {
		super.m_type = 2;
	}
	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制圆形 ");
	}
}

//新增画三角形
class Triangle extends Shape {
	Triangle() {
		super.m_type = 3;
	}
	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制三角形 ");
	}
}

//新增一个图形
class OtherGraphic extends Shape {
	OtherGraphic() {
		super.m_type = 4;
	}

	@Override
	public void draw() {
		// TODO Auto-generated method stub
		System.out.println(" 绘制其它图形 ");
	}
}
```



### 迪米特法则

#### 介绍

1. 一个对象应该对其他对象保持最少的了解
2. 类和类关于越密切，耦合度就越大
3. 迪米特法则(Demeter Principle)又叫最少知道原则，即一个类对自己依赖的类知道的越少越好。也就是说，对于被依赖的类不管多么复杂，都尽量将逻辑封装在类的内部。对外除了提供的public方法，不对外泄露任何信息
4. 迪米特法则还有个更简单的定义：只与直接的朋友通信
5. **直接的朋友**：每个对象都会与其他对象有耦合关系，只要两个对象之间有耦合关系，我们就说这两个对象之间是朋友关系。耦合的方式有很多：依赖、关联、组合、聚合等。其中，我们成出现的成员变量，方法参数，方法返回值中的类为直接的朋友，而出现在局部变量中的类不是直接的朋友。也就是说，陌生的类最好不要以局部变量的形式出现在类的内部

#### 实例应用

**描述**：有一个学校，下属有各个学院和总部，现要求打印出学校总部员工ID和学院员工的ID

```Java
import java.util.ArrayList;
import java.util.List;

//客户端
public class Demeter {

	public static void main(String[] args) {
		//创建了一个 SchoolManager 对象
		SchoolManager schoolManager = new SchoolManager();
		//输出学院的员工id 和  学校总部的员工信息
		schoolManager.printAllEmployee(new CollegeManager());

	}

}


//学校总部员工类
class Employee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//学院的员工类
class CollegeEmployee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//管理学院员工的管理类
class CollegeManager {
	//返回学院的所有员工
	public List<CollegeEmployee> getAllEmployee() {
		List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
		for (int i = 0; i < 10; i++) { //这里我们增加了10个员工到 list
			CollegeEmployee emp = new CollegeEmployee();
			emp.setId("学院员工id= " + i);
			list.add(emp);
		}
		return list;
	}
}

//学校管理类

//分析 SchoolManager 类的直接朋友类有哪些 Employee、CollegeManager
//CollegeEmployee 不是 直接朋友 而是一个陌生类，这样违背了 迪米特法则 
class SchoolManager {
	//返回学校总部的员工
	public List<Employee> getAllEmployee() {
		List<Employee> list = new ArrayList<Employee>();
		
		for (int i = 0; i < 5; i++) { //这里我们增加了5个员工到 list
			Employee emp = new Employee();
			emp.setId("学校总部员工id= " + i);
			list.add(emp);
		}
		return list;
	}

	//该方法完成输出学校总部和学院员工信息(id)
	void printAllEmployee(CollegeManager sub) {
		
		//分析问题
		//1. 这里的 CollegeEmployee 不是  SchoolManager的直接朋友
		//2. CollegeEmployee 是以局部变量方式出现在 SchoolManager
		//3. 违反了 迪米特法则 
		
		//获取到学院员工
		List<CollegeEmployee> list1 = sub.getAllEmployee();
		System.out.println("------------学院员工------------");
		for (CollegeEmployee e : list1) {
			System.out.println(e.getId());
		}
		//获取到学校总部员工
		List<Employee> list2 = this.getAllEmployee();
		System.out.println("------------学校总部员工------------");
		for (Employee e : list2) {
			System.out.println(e.getId());
		}
	}
}
```

{{< admonition example 应用实例改进 >}}

1. 前面设计的问题在于SchoolManager中，CollegeEmployee类并不是SchoolManager类的直接朋友
2. 按照迪米特法则，应该避免类中出现这样非直接朋友关系的耦合

{{< / admonition >}}

```Java
import java.util.ArrayList;
import java.util.List;

//客户端
public class Demeter {

	public static void main(String[] args) {
		System.out.println("~~~使用迪米特法则的改进~~~");
		//创建了一个 SchoolManager 对象
		SchoolManager schoolManager = new SchoolManager();
		//输出学院的员工id 和  学校总部的员工信息
		schoolManager.printAllEmployee(new CollegeManager());

	}

}


//学校总部员工类
class Employee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//学院的员工类
class CollegeEmployee {
	private String id;

	public void setId(String id) {
		this.id = id;
	}

	public String getId() {
		return id;
	}
}


//管理学院员工的管理类
class CollegeManager {
	//返回学院的所有员工
	public List<CollegeEmployee> getAllEmployee() {
		List<CollegeEmployee> list = new ArrayList<CollegeEmployee>();
		for (int i = 0; i < 10; i++) { //这里我们增加了10个员工到 list
			CollegeEmployee emp = new CollegeEmployee();
			emp.setId("学院员工id= " + i);
			list.add(emp);
		}
		return list;
	}
	
	//输出学院员工的信息
	public void printEmployee() {
		//获取到学院员工
		List<CollegeEmployee> list1 = getAllEmployee();
		System.out.println("------------学院员工------------");
		for (CollegeEmployee e : list1) {
			System.out.println(e.getId());
		}
	}
}

//学校管理类

//分析 SchoolManager 类的直接朋友类有哪些 Employee、CollegeManager
//CollegeEmployee 不是 直接朋友 而是一个陌生类，这样违背了 迪米特法则 
class SchoolManager {
	//返回学校总部的员工
	public List<Employee> getAllEmployee() {
		List<Employee> list = new ArrayList<Employee>();
		
		for (int i = 0; i < 5; i++) { //这里我们增加了5个员工到 list
			Employee emp = new Employee();
			emp.setId("学校总部员工id= " + i);
			list.add(emp);
		}
		return list;
	}

	//该方法完成输出学校总部和学院员工信息(id)
	void printAllEmployee(CollegeManager sub) {
		
		//分析问题
		//1. 将输出学院的员工方法，封装到CollegeManager
		sub.printEmployee();
	
		//获取到学校总部员工
		List<Employee> list2 = this.getAllEmployee();
		System.out.println("------------学校总部员工------------");
		for (Employee e : list2) {
			System.out.println(e.getId());
		}
	}
}
```

{{< admonition tip 迪米特法则注意事项和细节 >}}

1. 迪米特法则的核心是降低类之间的耦合
2. 但是注意：由于每个类都减少了不必要的依赖，因此迪米特法则只是要求降低类间（对象间）耦合关系，并不是要求完全没有依赖关系

{{< /admonition >}}



### 合成复用原则

#### 介绍

原则是尽量使用合成/聚合的方式，而不是使用继承

![截屏2023-03-05 21.05.08](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-05%2021.05.08.png "合成复用原则")

{{< admonition abstract 设计原则核心思想 >}}

1. 找出应用中可能需要变化之处，把它们独立出来，不要和那些不需要变化的代码混在一起
2. 针对接口编程，而不是针对实现编程
3. 为了交互对象之间的松耦合设计而努力

{{< /admonition >}}

<br><br><br>

## UML类图

### 介绍

1. UML--Unified modeling language UML（统一建模语言），是一种用于软件系统分析和设计的语言工具，它用于帮助软件开发人员进行思考和记录思路的结果
2. UML本身是一套符号的规定，就像数学符号和化学符号一样，这些符号用于描述软件模型中的各个元素和他们之间的关系，比如类、接口、实现、泛化、依赖、组合、聚合等

![截屏2023-03-05 23.01.54](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-05%2023.01.54.png "类关系")

<br>

{{< admonition tip UML图例分类 >}}

1. 用例图（use case）
2. 静态结构图：**类图**、对象图、包图、组件图、部署图
3. 动态行为图：交互图（时序图与协作图）、状态图、活动图

**说明**：

1. **类图用于描述系统中类（对象）本身的组成和类（对象）之间的各种静态关系，是UML图中最核心的图例**
2. **设计模式中使用类图描述模式设计过程**
3. **类之间关系：依赖、泛化（继承）、实现、关联、聚合与组合**

{{< /admonition >}}

### 依赖关系（Dependence）

只要在类中使用到了对方，那么他们之间就存在依赖关系。如果没有对方，连编译都通过不了。

![截屏2023-03-06 10.59.17](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2010.59.17.png "依赖关系")

### 泛化关系（Generalization）

如果一个模型元素（子代）基于另一个模型元素（父代），那么这两个元素之间就存在泛化关系（**继承关系**）

![截屏2023-03-06 11.04.28](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2011.04.28.png "泛化关系")

### 实现关系（Implementation）

实现关系实际上是A类实现B接口，它是**依赖关系的特例**

![截屏2023-03-06 11.10.37](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2011.10.37.png "实现关系")

### 关联关系（Association）

关联关系实际上是类与类之间的联系，它是**依赖关系的特例**

关联具有**导航性**：即双向关系或单向关系

关系具有**多重性**：如“1”（表示有且仅有一个），“0...”（表示0个或者多个），“0，1”（表示0个或者一个），“n...m”（表示n到m个都可以），“m...*”（表示至少m个）

![截屏2023-03-06 22.59.32](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2022.59.32.png "关联关系")

<br>

### 聚合关系（Aggregation）

是整体与部分的关系, 且部分可以离开整体而单独存在。

聚合关系是关联关系的一种，是强的关联关系

关联和聚合在语法上无法区分，必须考察具体的逻辑关系

**如下所示**：车和轮胎是整体和部分的关系，轮胎离开车仍然可以存在

![截屏2023-03-06 22.52.13](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2022.52.13.png "聚合关系")

<br>

### 组合关系（Composition）

组合关系：也是整体与部分的关系，但是**整体与部分不可以分开**

组合关系是关联关系中的一种，也是比聚合关系还要强的关系，它要求普通的聚合关系中代表整体的对象负责代表部分对象的生命周期

**如下所示**：公司和部分是整体和部门的关系，没有公司就不存在部门

![截屏2023-03-06 23.01.18](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-03-06%2023.01.18.png "组合关系")

<br>

<br>

<br>

## 设计模式

### 介绍

1. 设计模式是程序员在面对同类软件工程设计问题所总结出来的有用的经验，模式不是代码，而是某类问题的通用解决方案，设计模式（Design Pattern）代表了**最佳的实践**。这些解决方案是众多软件开发人员经过相当长的一段时间的试验和错误总结出来的。
2. 设计模式的本质是提高软件的维护性、通用型和扩展性，并降低软件的复杂度。
3. 《设计模式》是经典的书，作者是Erich Gamma、Richard Helm、Ralph Johnson和John Vlissides Design（俗称“四人组GOF”）
4. 设计模式并不局限于某种语言，Java、PHP、C++都有设计模式。

### 设计模式分类

{{< admonition abstract 模式分为三种类型 共23种 >}}

1. **创建型模式：** **单例模式**、抽象工厂模式、原型模式、建造者模式、**工厂模式**。
2. **结构型模式：** 适配器模式、桥接模式、**装饰模式**、组合模式、外观模式、享元模式、**代理模式**。
3. **行为型模式：** 模版方法模式、命令模式、访问者模式、迭代器模式、**观察者模式**、中介者模式、备忘录模式、解释器模式（Interpreter模式）、状态模式、策略模式、职责模式（责任链模式）。

{{< /admonition >}}

### 单例模式

#### 介绍

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类中**只能存在一个对象实例**，并且该类只提供一个取得其对象实例的方法（静态方法）。

<br>

**单例模式举例：** 比如Hibernate的SessionFactory，它充当数据存储源的代理，并负责创建Session对象。SessionFactory并不是一个轻量级的，一般情况下，一个项目通常只需要一个SessionFactory就够，这时就会使用到单例模式。

{{< admonition example 单例模式的八种方式 >}}

1. **饿汉式（静态常量）**
2. **饿汉式（静态代码块）**
3. 懒汉式（线程不安全）
4. 懒汉式（线程安全，同步方法）
5. 懒汉式（线程安全，同步代码块）
6. **双重检查**
7. **静态内部类**
8. **枚举**

{{< / admonition >}}

#### 饿汉式（静态常量）

##### 实现步骤

1. 构造器私有化（防止new）
2. 类的内部创建对象
3. 向外暴露一个静态的公共方法
4. 代码实现

```java
public class SingletonTest01 {

	public static void main(String[] args) {
		//测试
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

//饿汉式(静态变量)

class Singleton {
	
	//1. 构造器私有化, 外部能new
	private Singleton() {
		
	}
	//2.本类内部创建对象实例
	private final static Singleton instance = new Singleton();
	
	//3. 提供一个公有的静态方法，返回实例对象
	public static Singleton getInstance() {
		return instance;
	}
	
}
```

##### 模式优缺点说明：

1. **优点：** 这种写法比较简单，就是在类装载的时候就完成实例化。避免了线程同步问题。
2. **缺点：** 在类装载的时候就完成实例化，没有达到Lazy Loading的效果。如果从始至终从未使用过这个实例，则会造成内存的浪费。
3. 这种方式基于classoder机制避免了多线程的同步问题，不过，instabce在类装载时就实例化，在单例模式中大多数都是调用getInstance方法，但是导致类装载的原因有很多种，因此不能确定有其他的方式（或者其他的静态方法）导致类装载，这个时候初始化instance就没有达到lazy loading的效果。
4. 结论：这种单例模式可用，可能造成内存浪费。

<br>

#### 饿汉式（静态代码块）

##### 实例应用

```java
package com.atguigu.singleton.type2;

public class SingletonTest02 {

	public static void main(String[] args) {
		//测试
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

//饿汉式(静态变量)

class Singleton {
	
	//1. 构造器私有化, 外部能new
	private Singleton() {
		
	}
	
	//2.本类内部创建对象实例
	private  static Singleton instance;
	
	static { // 在静态代码块中，创建单例对象
		instance = new Singleton();
	}
	
	//3. 提供一个公有的静态方法，返回实例对象
	public static Singleton getInstance() {
		return instance;
	}
	
}
```

##### 模式优缺点说明

1. 这种方式和上面的方式其实类似，只不过将类实例化的过程放在了静态代码块中，也是在类装载的时候，就执行静态代码块中的代码，初始化类的实例。（优缺点与上面一致）
2. 结论：这种单例模式可用，但是可能造成内存浪费。

<br>

#### 懒汉式（线程不安全）

##### 实例应用

```java
public class SingletonTest03 {

	public static void main(String[] args) {
		System.out.println("懒汉式1 ， 线程不安全~");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

class Singleton {
	private static Singleton instance;
	
	private Singleton() {}
	
	//提供一个静态的公有方法，当使用到该方法时，才去创建 instance
	//即懒汉式
	public static Singleton getInstance() {
		if(instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```

##### 模式优缺点说明

1. 起到了Lazy Loading的效果，但是只能在单线程下使用。
2. 如果在多线程下，一个线程进入了if(singleton == null)判断语句块，还未来得及往下执行，另一个线程也通过了这个判断语句，这时便会产生多个实例。所以在多线程环境下不可以使用这种方式。
3. 结论：在实际开发中，不要使用这种方式。

<br>

#### 懒汉式（线程安全，同步方法）

##### 应用实例

```java
public class SingletonTest04 {

	public static void main(String[] args) {
		System.out.println("懒汉式2 ， 线程安全~");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
	}

}

// 懒汉式(线程安全，同步方法)
class Singleton {
	private static Singleton instance;
	
	private Singleton() {}
	
	//提供一个静态的公有方法，加入同步处理的代码，解决线程安全问题
	//即懒汉式
	public static synchronized Singleton getInstance() {
		if(instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```

##### 模式优缺点说明

1. 解决了线程不安全问题
2. 效率太低了，每个线程再想获得类的实例时候，执行getInstance()方法都要进行同步。而其实这个方法只执行一次实例化代码就够了，后面的想获得该类实例，直接return就行了。方法进行同步效率太低。
3. 结论：在实际开发中，不推荐使用这种方式

<br>

#### 懒汉式（线程安全，同步代码块）

##### 应用实例

```java
class Singleton {
	private static Singleton singleton;
	
	private Singleton() {}

	public Singleton getInstance() {
		if(singleton == null) {
			synchronized (Singleton.class) {
				singleton = new Singleton();
			}
		}
		return singleton;
	}
}
// 不推荐使用
```

##### 双重检查

```java
public class SingletonTest06 {

	public static void main(String[] args) {
		System.out.println("双重检查");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
		
	}

}

// 懒汉式(线程安全，同步方法)
class Singleton {
	private static volatile Singleton instance;
	
	private Singleton() {}
	
	//提供一个静态的公有方法，加入双重检查代码，解决线程安全问题, 同时解决懒加载问题
	//同时保证了效率, 推荐使用
	
	public static synchronized Singleton getInstance() {
		if(instance == null) {
			synchronized (Singleton.class) {
				if(instance == null) {
					instance = new Singleton();
				}
			}
			
		}
		return instance;
	}
}
```

##### 模式优缺点说明

1. Double-Check概念是多线程开发中常使用到的，如代码中所示，我们进行了两次if（singleton==null），这样就可以保证线程安全了。
2. 这样，实例化代码只用执行一次，后面再次访问时，判断if（singleton==null），直接return实例化对象，也避免了反复进行方法同步。
3. 线程安全；延迟加载；效率较高；
4. 结论：在实际开发中，推荐使用这种单例设计模式。

<br>

#### 静态内部类

##### 应用实例

```java
public class SingletonTest07 {

	public static void main(String[] args) {
		System.out.println("使用静态内部类完成单例模式");
		Singleton instance = Singleton.getInstance();
		Singleton instance2 = Singleton.getInstance();
		System.out.println(instance == instance2); // true
		System.out.println("instance.hashCode=" + instance.hashCode());
		System.out.println("instance2.hashCode=" + instance2.hashCode());
		
	}

}

// 静态内部类完成， 推荐使用
class Singleton {
	private static volatile Singleton instance;
	
	//构造器私有化
	private Singleton() {}
	
	//写一个静态内部类,该类中有一个静态属性 Singleton
	private static class SingletonInstance {
		private static final Singleton INSTANCE = new Singleton(); 
	}
	
	//提供一个静态的公有方法，直接返回SingletonInstance.INSTANCE
	
	public static synchronized Singleton getInstance() {
		
		return SingletonInstance.INSTANCE;
	}
}
```

##### 模式优缺点说明

1. 这种方式采用了类加载的机制来保证初始化实例时只有一个线程。
2. 静态内部类方式在Singleton类被装载时并不会立即实例化，而是在需要实例化时，调用getInstance方法，才会装载SingletonInstance类，从而完成Singleton的实例化。
3. 类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM帮我们保证了线程的安全性，在类进行初始化时，别的线程是无法进入的。
4. 优点：避免了线程不安全，利用静态内部类特点实现延迟加载，效率高。
5. 结论：推荐使用。

<br>

#### 枚举

##### 应用实例

```java
public class SingletonTest08 {
	public static void main(String[] args) {
		Singleton instance = Singleton.INSTANCE;
		Singleton instance2 = Singleton.INSTANCE;
		System.out.println(instance == instance2);
		
		System.out.println(instance.hashCode());
		System.out.println(instance2.hashCode());
		
		instance.sayOK();
	}
}

//使用枚举，可以实现单例, 推荐
enum Singleton {
	INSTANCE; //属性
	public void sayOK() {
		System.out.println("ok~");
	}
}
```

##### 模式优缺点说明

1. 这借助JDK1.5中添加的枚举来实现单例模式。不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象。
2. 这种方式是Effective Java作者Jsoh Bloch提倡的方式。
3. 结论：推荐使用

<br>

#### 单例模式在JDK应用的源码分析

##### 源码分析

**Java.lang.Runtime就是经典的单例模式（饿汉式）**

```java
public class Runtime {
    private static Runtime currentRuntime = new Runtime();

    /**
     * Returns the runtime object associated with the current Java application.
     * Most of the methods of class <code>Runtime</code> are instance
     * methods and must be invoked with respect to the current runtime object.
     *
     * @return  the <code>Runtime</code> object associated with the current
     *          Java application.
     */
    public static Runtime getRuntime() {
        return currentRuntime;
    }

    /** Don't let anyone else instantiate this class */
    private Runtime() {}
}
// 部分展示
```

{{< admonition tip 单例模式注意事项和细节说明 >}}

1. 单例模式保证了系统内存中该类只存在一个对象，节省了系统资源，对于一些需要频繁创建和销毁的对象，使用单例模式可以提高系统性能。
2. 当想实例化一个单例类的时候，必须要记住使用相应的获取对象的方法，而不是使用new。
3. 单例模式使用的场景：需要频繁地进行创建和销毁的对象、创建对象时耗时过多或消耗资源过多（即：**重量级对象**），但又经常用到的对象、**工具类对象**、频繁访问数据库或文件的对象（比如**数据源、session工厂**等）。

{{< /admonition >}}

<br>

### 工厂模式





















