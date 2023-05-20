# Lambda常见场景用法


<!--more-->

## :memo:介绍

1. Lambda 表达式，也可称为**闭包**，它是推动 **Java 8** 发布的最重要**新特性**。
2. Lambda 允许**把函数作为一个方法的参数**（函数作为参数传递进方法中）。
3. 使用 Lambda 表达式可以**使代码变的更加简洁紧凑**。

{{< admonition tip Lambda表达式特性 >}}

- **可选类型声明**：不需要声明参数类型，编译器可以统一识别参数值。
- **可选的参数圆括号**：一个参数无需定义圆括号，但多个参数需要定义圆括号。
- **可选的大括号**：如果主体包含了一个语句，就不需要使用大括号。
- **可选的返回关键字**：如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定表达式返回了一个数值。

{{< /admonition >}}

## :rainbow:使用场景

### 遍历

❌ 常规方法

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
for (String fruit : list) {
    System.out.println(fruit);
}
```

<br>

:white_check_mark: 使用Lambda表达式

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
list.forEach(fruit -> System.out.println(fruit));
```

<br>

### 排序

❌ 常规方法

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
Collections.sort(list, new Comparator() {
    public int compare(String s1, String s2) {
        return s1.compareTo(s2);
    }
});
```

<br>

:white_check_mark: 使用Lambda表达式

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
Collections.sort(list, (s1, s2) -> s1.compareTo(s2));
```

<br>

### 过滤

❌ 常规方法

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
List filteredList = new ArrayList();
for (String fruit : list) {
    if (fruit.startsWith("a")) {
        filteredList.add(fruit);
    }
}
```

<br>

:white_check_mark: 使用Lambda表达式

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
        List filter = list.stream()
                .filter(fruit -> fruit.startsWith("a"))
                .collect(Collectors.toList());
```

<br>

### 映射

❌ 常规方法

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
List lengths = new ArrayList();
for (String fruit : list) {
    lengths.add(fruit.length());
}
```

<br>

:white_check_mark: 使用Lambda表达式

```java
List<String> list1 = Arrays.asList("apple", "banana", "cherry");
        List<Integer>lengths=list1.stream()
                .map(String::length)
                .collect(Collectors.toList());
```

<br>

### 规约

❌ 常规方法

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
int sum = 0;
for (int i : list) {
	sum += i;
}
```

<br>

:white_check_mark: 使用Lambda表达式

```java
List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
        int sum = list.stream().reduce(0, Integer::sum);
```

<br>

### 分组

❌ 常规方法

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
Map<Integer, List<String>> grouped = new HashMap<Integer, List<String>>();
for (String fruit : list) {
    int length = fruit.length();
    if (!grouped.containsKey(length)) {
        grouped.put(length, new ArrayList());
    }
    grouped.get(length).add(fruit);
}
```

<br>

:white_check_mark: 使用Lambda表达式

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
        Map<Integer, List<String>> grouped = list.stream()
                .collect(Collectors.groupingBy(String::length));
```

<br>

### 函数式接口实现

❌ 常规方法

```java
public interface MyInterface {
    public void doSomething(String input); 
}

MyInterface myObject = new MyInterface() {
	public void doSomething(String input) {
		System.out.println(input);
	}
};

myObject.doSomething("Hello World");

```

<br>

:white_check_mark: 使用Lambda表达式

```java
public interface MyInterface {
    public void doSomething(String input); 
}

MyInterface myObject = input -> System.out.println(input);
myObject.doSomething("Hello World");
```

<br>

### 线程创建

❌ 常规方法

```java
Thread thread = new Thread(new Runnable() {
    public void run() {
    	System.out.println("Thread is running.");
    }
});
thread.start();
```

<br>

:white_check_mark: 使用Lambda表达式

```java
Thread thread = new Thread(() -> System.out.println("Thread is running."));
thread.start();
```

<br>

### Optional操作

❌ 常规方法

```java
String str = "Hello World";
if (str != null) {
	System.out.println(str.toUpperCase());
}
```

<br>

:white_check_mark: 使用Lambda表达式

```java
Optional.of("hello")
                .map(String::toUpperCase)
                .ifPresent(System.out::println);
```

<br>

### Stream流水线操作

❌ 常规方法

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
List<String> filteredList = new ArrayList();
for (String fruit : list) {
    if (fruit.startsWith("a")) {
        filteredList.add(fruit.toUpperCase());
    }
}
Collections.sort(filteredList);
```

<br>

:white_check_mark: 使用Lambda表达式

```java
List<String> list = Arrays.asList("apple", "banana", "cherry");
        List<String> filteredList = list.stream()
                .filter(fruit -> fruit.startsWith("a"))
                .map(String::toUpperCase)
                .sorted()
                .collect(Collectors.toList());
```

<br>

## :trophy:总结

当你需要一个**仅在一个地方有效**的函数, 并且只做一件事情，那么就用 lambda。比如，lambda经常用在 **sorted 函数的 key 参数**中。所以，可以认为，**lambda的主要目的是为了减少单行函数的定义。**

**lambda不会提高代码执行效率**，它只是定义了一个**匿名函数**，使我们的**代码更加简洁**，而且在某种程度上**可读性更高**。

{{< admonition >}}

- **如果可以使用for...if来完成的，坚决不用lambda。**
- 如果使用lambda，**lambda内不要包含循环**，否则，最好定义函数来完成，使代码获得可重用性和更好的可读性。

{{< /admonition >}}

