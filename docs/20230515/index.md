# 剑指Offer-个人题解


剑指Offer

<!--more-->

## **简单**

### **斐波那契数列**

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

### **数组中重复的数字**

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



### **替换空格**

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



### **从尾到头打印链表**

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

<br>



### **用两个栈实现队列**

#### 描述

![用两个栈实现队列](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-16%2009.03.02.png "用两个栈实现队列")

#### 示例1

```markdown
输入：["PSH1","PSH2","POP","POP"]
返回值：1,2
说明：
"PSH1":代表将1插入队列尾部
"PSH2":代表将2插入队列尾部
"POP“:代表删除一个元素，先进先出=>返回1
"POP“:代表删除一个元素，先进先出=>返回2     
```

#### 代码实现

```Java
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
     public void push(int node) {
        stack1.push(node);
    }
     
    public int pop() {
        if (stack2.size() <= 0){
            while (stack1.size() != 0){
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }
}
```

<br>



### **旋转数组的最小数字**

#### 描述

![旋转数组的最小数字](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-16%2009.05.07.png "旋转数组的最小数字")

#### 示例1

```markdown
输入：[3,4,5,1,2]
返回值：1
```

#### 示例2

```markdown
输入：[3,100,200,3]
返回值：3
```

#### 代码实现

```Java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int[] array) {
        if (array.length == 0) {
            return 0;
        }
        int min = array[0];
        for (int e : array) {
            if (e < min) {
                min = e;
            }
        }
        return min;
    }
}
```

<br>



###  **二进制中1的个数**

#### 描述

![二进制中1的个数](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-16%2009.06.53.png "二进制中1的个数")

#### 示例1

```markdown
输入：10
返回值：2
说明：
十进制中10的32位二进制表示为0000 0000 0000 0000 0000 0000 0000 1010，其中有两个1。       
```

#### 示例2

```markdown
输入：-1
返回值：32
说明：
负数使用补码表示 ，-1的32位二进制表示为1111 1111 1111 1111 1111 1111 1111 1111，其中32个1    
```

#### 代码实现

```Java
import java.util.*;
public class Solution {
    public int NumberOf1(int n) {
        Scanner in = new Scanner(System.in);
        int count = 0;
        for (int i = 0; i < 32; i++) {
            if ((n & (1 << i)) != 0) {
                count++;
            }
        }
        return count;
    }
}
```

<br>



### **打印从1到最大的n位数**

#### 描述

![打印从1到最大的n位数](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-16%2009.09.20.png "打印从1到最大的n位数")

#### 示例1

```markdown
输入：1
返回值：[1,2,3,4,5,6,7,8,9]
```

#### 代码实现

```Java
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     *
     * @param n int整型 最大位数
     * @return int整型一维数组
     */
    public int[] printNumbers (int n) {
        // write code here
        int length = (int)Math.pow(10, n);
        int []nums = new int[length-1];
        for(int i=1;i<length;i++){
            nums[i-1]=i;
        }

        return nums;
    }
}
```

<br>



### **删除链表的节点**

#### 描述

![删除链表的节点](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-16%2010.25.39.png "删除链表的节点")

#### 示例1

```markdown
输入：{2,5,1,9},5
返回值：{2,1,9}
说明：
给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 2 -> 1 -> 9   
```

#### 示例2

```markdown
输入：{2,5,1,9},1
返回值：{2,5,9}
说明：
给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 2 -> 5 -> 9   
```

#### 代码实现

```Java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 *   public ListNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param head ListNode类 
     * @param val int整型 
     * @return ListNode类
     */
    public ListNode deleteNode (ListNode head, int val) {
        // write code here
        if(head.val == val) {
            return head.next;
        }
        ListNode newHead = head;
        while(newHead.next != null) {
            if(newHead.next.val == val) {
                newHead.next = newHead.next.next;
            }
            newHead = newHead.next;
        }
        return head;
    }
}
```

<br>



### **链表中倒数最后k个结点**

#### 描述

![链表中倒数最后k个结点](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-16%2010.33.11.png "链表中倒数最后k个结点")

#### 示例1

```markdown
输入：{1,2,3,4,5},2
返回值：{4,5}
说明：返回倒数第2个节点4，系统会打印后面所有的节点来比较。 
```

#### 示例2

```markdown
输入：{2},8
返回值：{}
```

#### 代码实现

```Java
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 *   public ListNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     *
     * @param pHead ListNode类
     * @param k int整型
     * @return ListNode类
     */
    public ListNode FindKthToTail (ListNode pHead, int k) {
        // write code here
        // 快慢指针思想
        ListNode fast = pHead;
        ListNode slow = pHead;
        int step=0;
        if (pHead == null) {
            return null;
        }

        // 快指针先走k步
        while (fast != null && step != k) {
            fast = fast.next;
            step++;
        }

        if (step < k) {
            return null;
        }

        // 慢指针与快指针同步
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```

<br>



### **反转链表**

#### 描述

![反转链表](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%88%AA%E5%B1%8F2023-05-16%2010.36.20.png "反转链表")

#### 示例1

```markdown
输入：{1,2,3}
返回值：{3,2,1}
```

#### 示例2

```markdown
输入：{}
返回值：{}
说明：空链表则输出空
```

#### 代码实现

```Java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
        ListNode pre = null;
        ListNode nextOne = null;
        while (head != null) {
            nextOne = head.next;
            head.next = pre;
            pre = head;
            head = nextOne;
        }
        return pre;
    }
}
```

<br>



### **合并两个排序的链表**

#### 描述

![合并两个排序的链表](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%90%88%E5%B9%B6%E4%B8%A4%E4%B8%AA%E6%8E%92%E5%BA%8F%E7%9A%84%E9%93%BE%E8%A1%A81.jpg "合并两个排序的链表")

#### 示例1

```markdown
输入：{1,3,5},{2,4,6}
返回值：{1,2,3,4,5,6}
```

#### 示例2

```markdown
输入：{},{}
返回值：{}
```

#### 示例3

```markdown
输入：{-1,2,4},{1,3,4}
返回值：{-1,1,2,3,4,4}
```

#### 代码实现

```Java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode Merge(ListNode list1, ListNode list2) {
        if (list1 == null && list2 == null) {
            return null;
        }
        ListNode head = new ListNode(-1);
        ListNode pre = head;
        while (list1 != null && list2 != null) {
            if (list1.val < list2.val) {
                pre.next = list1;
                list1 = list1.next;
            } else if (list1.val > list2.val) {
                pre.next = list2;
                list2 = list2.next;
            } else {
                pre.next = list1;
                list1 = list1.next;
                pre=pre.next;
                pre.next = list2;
                list2 = list2.next;
            }
            pre=pre.next;
        }
        pre.next = list1 == null ? list2 : list1;
        return head.next;
    }
}
```

<br>



### **二叉树的镜像**

#### 描述

![二叉树的镜像](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%95%9C%E5%83%8F1.jpg "二叉树的镜像")

#### 示例1

```markdown
输入：{8,6,10,5,7,9,11}
返回值：{8,10,6,11,9,7,5}
说明：如题面所示    
```

#### 示例2

```markdown
输入：{}
返回值：{}
```

#### 代码实现

```Java
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     *
     * @param pRoot TreeNode类
     * @return TreeNode类
     */
    public TreeNode Mirror (TreeNode pRoot) {
        // write code here
        if (pRoot == null) {
            return null;
        }

        TreeNode tmp = pRoot.left;
        pRoot.left = pRoot.right ;
        pRoot.right = tmp;
        Mirror(pRoot.left);
        Mirror(pRoot.right);
        return pRoot;
    }
}
```

<br>



### **对称的二叉树**

#### 描述

![对称的二叉树](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%AF%B9%E7%A7%B0%E7%9A%84%E4%BA%8C%E5%8F%89%E6%A0%911.jpg "对称的二叉树")

#### 示例1

```markdown
输入：{1,2,2,3,4,4,3}
返回值：true
```

#### 示例2

```markdown
输入：{8,6,9,5,7,7,5}
返回值：false
```

#### 代码实现

```Java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/

import java.util.*;
public class Solution {

    boolean isSymmetrical(TreeNode pRoot) {
        if (pRoot == null) {
            return true;
        }

        Queue<TreeNode> left = new LinkedList();
        Queue<TreeNode> right = new LinkedList();

        left.offer(pRoot.left);
        right.offer(pRoot.right);

        while (!left.isEmpty() && !right.isEmpty()) {
            TreeNode tn_left = left.poll();
            TreeNode tn_right = right.poll();

            if (tn_left == null && tn_right == null) {
                continue;
            }
            if (tn_left == null || tn_right == null || tn_left.val != tn_right.val) {
                return false;
            }

            left.offer(tn_left.left);
            left.offer(tn_left.right);

            right.offer(tn_right.right);
            right.offer(tn_right.left);

        }

        return  true ;
    }
}
```

<br>



### **顺时针打印矩阵**

#### 描述

![顺时针打印矩阵](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E9%A1%BA%E6%97%B6%E9%92%88%E6%89%93%E5%8D%B0%E7%9F%A9%E9%98%B5.png "顺时针打印矩阵")

#### 示例1

```markdown
输入：[[1,2,3,4],[5,6,7,8],[9,10,11,12],[13,14,15,16]]
返回值：[1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10]
```

#### 示例2

```markdown
输入：[[1,2,3,1],[4,5,6,1],[4,5,6,1]]
返回值：[1,2,3,1,1,1,6,5,4,4,5,6]
```

#### 代码实现

```Java
import java.util.*;
public class Solution {
    public ArrayList<Integer> printMatrix(int [][] matrix) {
        ArrayList<Integer> nums = new ArrayList();
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return nums;
        }

        int up = 0, down = matrix.length - 1, left = 0, right = matrix[0].length - 1;

        while (true) {
            // 往右走
            for (int index = left; index <= right; index++) {
                nums.add(matrix[up][index]);
            }
            up++;
            if (up > down) {
                break;
            }

            // 往下走
            for (int index = up; index <= down; index++) {
                nums.add(matrix[index][right]);
            }
            right--;
            if (left > right) {
                break;
            }

            // 往左走
            for (int index = right; index >= left; index--) {
                nums.add(matrix[down][index]);
            }
            down--;
            if (up > down) {
                break;
            }

            // 往上走
            for (int index = down; index >= up; index--) {
                nums.add(matrix[index][left]);
            }
            left++;
            if (left > right) {
                break;
            }
        }
        return nums;
    }
}
```

<br>



### **包含min函数的栈**

#### 描述

![包含min函数的栈](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E5%8C%85%E5%90%ABmin%E5%87%BD%E6%95%B0%E7%9A%84%E6%A0%88.jpg "包含min函数的栈")

#### 示例1

```markdown
输入：["PSH-1","PSH2","MIN","TOP","POP","PSH1","TOP","MIN"]
返回值：-1,2,1,-1
```

#### 示例2

```markdown
输入：" "
返回值："%20"
```

#### 代码实现

```Java
import java.util.Stack;

public class Solution {

    Stack<Integer>num = new Stack<>();
    Stack<Integer>min = new Stack<>();

    public void push(int node) {
        num.push(node);
        if (min.isEmpty() || min.peek() > node) {
            min.push(node);
        } else {
            min.push(min.peek());
        }

    }

    public void pop() {
        num.pop();
        min.pop();
    }

    public int top() {
        int top_val = num.peek();
        return top_val;
    }

    public int min() {
        return min.peek();
    }
}
```

<br>



### **从上往下打印二叉树**

#### 描述

![从上往下打印二叉树](https://cdn.jsdelivr.net/gh/Turbo-King/images/从上往下打印二叉树.png "从上往下打印二叉树")

#### 示例1

```markdown
输入：{8,6,10,#,#,2,1}
返回值：[8,6,10,2,1]
```

#### 示例2

```markdown
输入：{5,4,#,3,#,2,#,1}
返回值：[5,4,3,2,1]
```

#### 代码实现

```Java
import java.util.*;
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    ArrayList<Integer> num = new ArrayList();
    Queue<TreeNode>node = new LinkedList();
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root) {
        node.add(root);
        while (!node.isEmpty()) {
            TreeNode tmp = node.poll();
            if (tmp != null) {
                num.add(tmp.val);
                node.add(tmp.left);
                node.add(tmp.right);
            }

        }
        return num;
    }
}
```

<br>



### **数组中出现次数超过一半的数字**

#### 描述

![数组中出现次数超过一半的数字](https://cdn.jsdelivr.net/gh/Turbo-King/images/%E6%95%B0%E7%BB%84%E4%B8%AD%E5%87%BA%E7%8E%B0%E6%AC%A1%E6%95%B0%E8%B6%85%E8%BF%87%E4%B8%80%E5%8D%8A%E7%9A%84%E6%95%B0%E5%AD%97.png "数组中出现次数超过一半的数字")

#### 示例1

```markdown
输入：
[1,2,3,2,2,2,5,4,2]
返回值：2
```

#### 示例2

```markdown
输入：[3,3,3,3,2,2,2]
返回值：3
```

#### 示例3

```markdown
输入：[1]
返回值：1
```

#### 代码实现

```Java
public class Solution {
    public int MoreThanHalfNum_Solution(int [] array) {
        int []count = new int[10000];
        if(array.length==1){
            return array[0];
        }
        for (int i = 0; i < array.length; i++) {
            count[array[i]]++;
        }
        int mid = array.length >> 1;
        int target = 0;
        for (int i = 0; i < count.length; i++) {
            if (count[i] > mid) {
                target = i;
                break;
            }
        }
        return target;
    }
}
```

<br>



###  **连续子数组的最大和**

#### 描述

![ 连续子数组的最大和](https://cdn.jsdelivr.net/gh/Turbo-King/images/%20%E8%BF%9E%E7%BB%AD%E5%AD%90%E6%95%B0%E7%BB%84%E7%9A%84%E6%9C%80%E5%A4%A7%E5%92%8C.png " 连续子数组的最大和")

#### 示例1

```markdown
输入：[1,-2,3,10,-4,7,2,-5]
返回值：18
说明：经分析可知，输入数组的子数组[3,10,-4,7,2]可以求得最大和为18
```

#### 示例2

```markdown
输入：[2]
返回值：2
```

#### 示例3

```markdown
输入：[-10]
返回值：-10
```

#### 代码实现

```Java
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) {
        // 未优化
        // int []dp = new int[array.length];
        // int max = array[0];
        // dp[0] = array[0];
        // for (int i = 1; i < array.length; i++) {
        //     dp[i] = Math.max(array[i], dp[i - 1] + array[i]);
        //     max = Math.max(max, dp[i]);
        // }
        // return max;


        // 优化空间复杂度
        int sum=array[0];
        int max=array[0];
        for(int i=1;i<array.length;i++){
            sum=Math.max(sum+array[i],array[i]);
            max=Math.max(max,sum);
        }
        return max;
    }
}
```

















<br>

**未完待续···**

