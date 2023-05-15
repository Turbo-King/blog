---
weight: 5
title: "剑指Offer-个人题解"
date: 2023-05-15T21:33:35+08:00
draft: false
author: "Turbo-King"
authorLink: "https://turbo-king.github.io/"
description: "剑指Offer"
featuredImage : "https://cdn.jsdelivr.net/gh/Turbo-King/images/code-wallpaper.jpg"

tags: ["算法","Java"]
categories: ["Markdown"]

lightgallery: true
---

剑指Offer

<!--more-->

## 简单

### 斐波那契数列

#### 描述

![斐波那契数列](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-15%2021.47.52.png '斐波那契数列')

#### 输入描述：

一个正整数n

#### 返回值描述：

输出一个正整数。

#### 示例1

```markdown
输入：4
返回值：3
说明：根据斐波那契数列的定义可知
fib(1)=1,fib(2)=1,fib(3)=fib(3-1)+fib(3-2)=2,fib(4)=fib(4-1)+fib(4-2)=3，所以答案为3。  
```

#### 示例2

```markdown
输入：1
返回值：1
```

#### 示例3

```markdown
输入：2
返回值：1
```

#### 代码实现

```Java
public class Solution {
    public int Fibonacci(int n) {
        if(n==1||n==2)return 1;
        int[] a=new int[n+1];
        a[1]=a[2]=1;
        for(int i=3;i<=n;i++){
            a[i]=a[i-1]+a[i-2];
        }
        return a[n];
    }
}
```

<br> 

### 数组中重复的数字

#### 描述

![数组中重复的数字](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-15%2022.09.23.png "数组中重复的数字")

#### 示例1

```markdown
输入：[2,3,1,0,2,5,3]
返回值：2
说明：2或3都是对的    
```

#### 代码实现

```Java
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param numbers int整型一维数组 
     * @return int整型
     */
    public int duplicate (int[] numbers) {
        // write code here
        int tep = 0;
        for(int i = 0;i < numbers.length;i++){
            tep = numbers[i];
            int count = 0;
            for(int j = 0; j< numbers.length;j++){
                if(tep==numbers[j]){
                    count++;
                    if(count>1){
                        return tep;
                    }
                }
            }
        }
        return -1;
    }
}
```

<br>



### 替换空格

#### 描述

![替换空格](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-15%2022.16.06.png "替换空格")

#### 示例1

```markdown
输入："We Are Happy"
返回值："We%20Are%20Happy"
```

#### 示例2

```markdown
输入：" "
返回值："%20"
```

#### 代码实现

```Java
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param s string字符串 
     * @return string字符串
     */
    public String replaceSpace (String s) {
        // write code here
        return s.replace(" ", "%20");
    }
}
```

<br>



### 从尾到头打印链表

#### 描述

![从尾到头打印链表](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-15%2022.23.20.png "从尾到头打印链表")

#### 示例1

```markdown
输入：{1,2,3}
返回值：[3,2,1]
```

#### 示例2

```markdown
输入：{67,0,24,58}
返回值：[58,24,0,67]
```

#### 代码实现

```Java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
          ArrayList<Integer> list = new ArrayList<>();
        ListNode tmp = listNode;
        while(tmp!=null){
            list.add(0,tmp.val);
            tmp = tmp.next;
        }
        return list;
    }
}
```



