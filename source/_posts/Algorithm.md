---
title: Data Structure & Algorithm
date: 2021-07-09 16:02:50
tags: [Algorithm, Java]
toc: true
categories: Notes
---

## Data Structure

**Data structure** is a particular way of organzing data in a computer so that it can be used efficiently.

<!-- more -->

**Common data structure**

- **Array**
- **Stack**
- **Queue**
- **Linked List**
- **Tree**
- **Heap**
- **Graph**
- Hash Table
- UnionFind
- Trie
- ...

### Time Complexity & Space Complexity

**Big O notation**: algorithm complexity (time complexity(TC), space complexity(SC))

`1 < log2n == log10(n) < sqrt(n) < n < nlog n < n^2 < n^3 < 2^n < 10^n < n! < n^n`

### ArrayList

ArrayList is regarded as a **resizable** array.

```java
import java.util.*;

public class ArrayListDemo {
   public static void main(String[] args) {
      // byte -> Byte
      // short -> Short
      // int -> Integer = int + null
      // long -> Long
      // double -> Double
      // char -> Character
      // boolean -> Boolean
      List<Integer> newList = new ArrayList<Integer>(); // {}
      newList.add(1); // {1}
      newList.add(9); // {1, 9}
      System.out.println(newList.get(1)); // 9
      System.out.println(newList.size()); // 2
   }
}
```

---

## Binary Search

**Principles of Binary Search**

1. We must guarantee that the **search space decreases** over time (after each iteration)
2. We must guarantee that the **target (if exists) cannot be ruled out** accidentally, when we change the value of Left or Right. **(It is critical to define the rule about how to move the range for search)**

> Array has to be sorted.

|                                                    | 可用的循环条件                                                                                                                                                                                        | 不可用的循环条件                                                                                          |
| -------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| case1: right = mid - 1 <br/> case2: left = mid + 1 | 1. left <= right 退出循环时一定是 empty <br/> 2. left < right 退出循环时一定是一个元素或者 empty -> 需要 postprocessing <br/>3. left + 1 < right 退出循环时一定是两个元素或者一个元素，不可能是 empty |                                                                                                           |
| case1: right = mid - 1 <br/> case2: left = mid     | left + 1 < right 退出循环时一定是两个元素或者一个元素，不可能是 empty                                                                                                                                 | 1. left <= right 因为 length = 1 或 2 时，会出死循环 <br/> 2. left < right 因为 length = 2 时，会出死循环 |
| case1: right = mid <br/> case2: left = mid + 1     | 1. left + 1 < right 退出循环时一定是两个元素或者一个元素，不可能是 empty <br/> 2. left < right 退出循环时一定是一个元素，不可能是 empty                                                               | left <= right 因为 length = 1 时，会出死循环                                                              |
| case1: right = mid <br/> case2: left = mid         | left + 1 < right 退出循环时一定是两个元素或者一个元素，不可能是 empty                                                                                                                                 | 1. left <= right 因为 length = 1 或 2 时，会出死循环 <br/> left < right 因为 length = 2 时，会出死循环    |

---

## Recursion, Queue & Stack

### Recursion 与计算的结合

1. 表象上：function calls itself
2. 实质上：Boil down a big problem to smaller ones (size n depends on size n-1, or n-2 or ... n/2)
3. **Implementation 上**：
   a. **Base case**: smallest problem to solve
   b. **Recursion rule**

**<font color=#3273DC>Example.1 Fibonacci Sequence</font>**

f(n)
0 1 2 3 4 5 6 7 ...
0 1 1 2 3 5 8 13 ...

f(n)

1.  subproblem: f(n - 1), f(n - 2), f(n - 3), ..., f(1), f(0)
2.  recursion rule: f(n) = f(n - 1) + f(n - 2) = A + B
3.  base case: f(0) = 0, f(1) = 1

```java
public long fib(int n) {
   if (n == 0 || n == 1) {
      return n;
   }
   return fib(n - 1) + fib(n - 2);
}
```

- TC: (2<sup>n</sup> - 1) \* T(node) = (2<sup>n</sup> - 1) \* O(1) = O(2<sup>n</sup> - 1) = O(2<sup>n</sup>)
- SC: n \* S(node) = n \* O(1) = O(n)

levels: n
total number of nodes: 1 + 2 + 4 + ... + 2<sup>n-1</sup> = 2<sup>n</sup> - 1

1. number of branches = number of recursion calls = number of used subproblems
2. leaf nodes = base cases
3. one node = function call

**<font color=#3273DC>Example.2 Power</font>**

a<sup>b</sup>

**method 1: loop**

- TC: O(b)
- SC: O(1)

**method 2: recursion**
f(a, b)

1.  subproblem: f(a, b - 1), f(a, b - 2), ... f(a, 0)
2.  recursion rule: f(a, b) = f(a, b - 1) \* a
3.  base case: f(a, 0) = 1

```java
public long power(int a, int b) {
   if (b == 0) {
      return 1;
   }
   return a * power(a, b - 1);
}
```

- TC: O(b)
- SC: O(b)

**method 3: recursion**
f(a, b)

1.  subproblem: f(a, b /2), f(a, b / 4), ..., f(a, 0)
2.  recursion rule:
    a. b is even: f(a, b) = f(a, b / 2)<sup>2</sup>
    b. b is odd: f(a, n) = f(a, b / 2)<sup>2</sup> \* a
3.  base case: f(a, 0) = 1

```java
public long power(int a, int b) {
   if (b == 0) {
      return 1;
   } else if (b % 2 == 0) {
      return power(a, b / 2) * power(a, b / 2);
   } else {
      return power(a, b / 2) * power(a, b / 2) * a;
   }
}
```

- TC: (1 + 2 + 4 + ... + 2<sup>logb - 1</sup>) = (2<sup>logb</sup> - 1) \* O(1) = (b - 1) \* O(1) = O(b)
- SC: logb \* O(1) = O(logb)

<br/>

```java
public long power(int a, int b) {
   if (b == 0) {
      return 1;
   }

   long half = power(a, b / 2);
   if (b % 2 == 0) {
      return half * half;
   } else {
      return half * half * a;
   }
}
```

- TC: total number of nodes \* T(node) = logb \* O(1) = O(logb)
- SC: logb \* O(1) = O(logb)

### Recursion 与 1D or 2D Array 的结合

**<font color=#3273DC>Example.1. 1D Array</font>**
二分法比较常见

1. MergeSort
2. QuickSort

**<font color=#3273DC>Example.2.1 8 queen</font>**
![](/assets/algorithm/12.png)

```java

```

**<font color=#3273DC>Example.2.2 How to print 2D Array in spiral order.</font>**

<!-- TODO: -->

### Recursion 与 LinkedList 的结合

**<font color=#3273DC>Example.1 Reverse a linked list. (pair by pair)</font>**
![](/assets/algorithm/13.png)
![](/assets/algorithm/14.png)

```java
ListNode reverseLinkedList(ListNode head) {
   if (head == null || head.next == null) {
      return head;
   }
   ListNode next = head.next;
   ListNode newHead = reverseLinkedList(head.next.next);

   head.next = newHead;
   next.next = head;
   return next;
}
```

**<font color=#3273DC>Example.3 Reverse a binary tree upside down.</font>**
Given a binary tree where all the right nodes are leaf nodes, flip it upside down and turn it into a tree left nodes. For example, turn these:
![](/assets/algorithm/15.png)
![](/assets/algorithm/16.png)

```java
TreeNode reverseTree(TreeNode root) {
   if (root == null || root.left == null) {
      return root;
   }

   TreeNode newRoot = reverseTree(root.left);

   root.left.left = root.right;
   root.left.right = root;
   root.left = null;
   root.right = null;
   return newRoot;
}
```

### Recursion 与 String 的结合

### Recursion 与 Tree 的结合

#### Recursion + Tree 第一类问题：从下往上反值

**<font color=#3273DC>Example.1 How to store how many nodes in each node's left-subtree?</font>**
**<font color=#3273DC>Example.2 Find the node with the max difference in the total number of descendents in its left subtree and right subtree.</font>**

```java
int maxDiffNode(TreeNode root, int[] globalMax, TreeNode[] solu) {
   if (root == null) {
      return 0;
   }

   int leftTotal = maxDiffNode(root.left, globalMax, solu);
   int rightTotal = maxDiffNode(root.right, globalMax, solu);

   if (Math.abs(leftTotal - rightTotal) > globalMax[0]) {
      globalMax[0] = Math.abs(leftTotal - rightTotal);
      solu[0] = root;
   }

   return leftTotal + rightTotal + 1;
}
```

**<font color=#3273DC>Example.3 Lowest Common Ancestor</font>**
![](/assets/algorithm/17.png)
![](/assets/algorithm/18.png)

```java
TreeNode LCA(TreeNode root, TreeNode a, TreeNode b) {
   // base case
   if (root == null || root == a || root == b) {
      return root;
   }

   TreeNode left = LCA(root.left, a, b);
   TreeNode right = LCA(root.right, a, b);

   if (left != null && right != null) {
      return root;
   }
   return left == null ? right : left;
}
```

**<font color=#3273DC>Example.4 Max Path Sum Binary Tree II (path from any node to any node)</font>**
Given a binary tree in which each node contains an integer number. Find the maximum possible sum from any node to any node (the start node and the end node can be the same)
![](/assets/algorithm/19.png)

1. **What do you expect from your lchild / rchild?** (usually it is the return type of the recursion funciton)
   (1) left: 直上直下的 path
   (2) right: 直上直下的 path
2. **What do you want to do in the current layer?**
   sum of 人字形 path = left 的一撇 + right 的一捺 + root.value
3. **What do you want to report to your parent?**
   root.value + Math.max(left, right)

```java
private int helper(TreeNode root, int[] max) {
   if (root == null) {
      return 0;
   }

   int left = helper(root.left, max);
   int right = helper(root.right, max);

   left = left < 0 ? 0 : left;
   right = right < 0 ? 0 : right;

   max[0] = Math.max(root.key + left + right, max[0]);

   return root.key + Math.max(left, right);
}
```

#### Recursion + Tree 第二类问题：Path Problem in Binary Tree

#### Recursion + Tree 第三类问题：Tree Serialization Problem

**Serialization**

- pre
- in
- post
- level

#### Recursion + Tree 第四类问题：Tree De-serialization Problem

### Queue

**FIFO** (first in first out) 先进先出

```java
Queue<Integer> queue = new LinkedList<Integer>();
// also could
Queue<Integer> queue = new ArrayDeque<Integer>();
```

**APIs:**

- `offer()`: insert an element in the tail of queue
- `poll()`: get an element from the head of queue
- `peek()`: read the value of head element of queue
- `size()`: how many elements are in the queue
- `isEmpty()`: if this queue is empty

### Stack

**LIFO** (last in first out) 后进先出

```java
Deque<Integer> stack = new LinkedList<Integer>();
// also could
Deque<Integer> stack = new ArrayDeque<Integer>();
```

**APIs:**

- `offerFirst()`: insert an element in the top of stack
- `pollFirst()`: get an element from the top of stack
- `peekFirst()`: read the value of top element stack
- `size()`: how many elements are in the stack
- `isEmpty()`: if this stack is empty or the size of this stack is 0

### Deque

**FIFO & LIFO**

**APIs:**

- `offerFirst()` / `offerLast()`
- `pollFirst()` / `pollLast()`
- `peekFirst()` / `peekLast()`

### Queue, Stack & Deque

| 数据结构(逻辑层面) | 内存里的存放方法    | 对应 java class         | 对应 java interface |
| ------------------ | ------------------- | ----------------------- | :-----------------: |
| queue (FIFO)       | array / linked list | ArrayDeque / LinkedList |        Queue        |
| stack (LIFO)       | array / linked list | ArrayDeque / LinkedList |       Deque?        |
| deque              | array / linked list | ArrayDeque / LinkedList |        Deque        |

**Queue** and **Deque** are **interfaces**

**Common APIs**

**Queue**

| Type of operation | throw exception | return special value (null/false) |
| ----------------- | --------------- | --------------------------------- |
| insert            | `add()`         | `offer()`                         |
| remove            | `remove()`      | `poll()`                          |
| examine           | `element()`     | `peek()`                          |

- `element()` is the method introduced by interface Queue, `add()` and `remove()` follows the definition of interface Collection
- `add()` and `offer()` return true when operation causes changes

**Deque**

<table>
   <tr>
      <th rowspan="2">Type of operation</th>
      <th colspan="2" align="center">First Element</th>
      <th colspan="2" align="center">Last Element</th>
   </tr>
   <tr>
      <th>throw exception</th>
      <th>return special value</th>
      <th>throw exception</th>
      <th>return special value</th>
   </tr>
   <tr>
      <td>insert</td>
      <td><code>addFirst()</code></td>
      <td><code>offerFirst()</code></td>
      <td><code>addLast()</code></td>
      <td><code>offerLast()</code></td>
   </tr>
   <tr>
      <td>remove</td>
      <td><code>removeFirst()</code></td>
      <td><code>pollFirst()</code></td>
      <td><code>removeLast()</code></td>
      <td><code>pollLast()</code></td>
   </tr>
   <tr>
      <td>examine</td>
      <td><code>getFirst()</code></td>
      <td><code>peekFirst()</code></td>
      <td><code>getLast()</code></td>
      <td><code>peekLast()</code></td>
   </tr>
</table>

> All the operations' cost is O(1)

### Stack by Linked List

**How to write an OOP?**

1. 根据 use case 设计 public API (包括 signature)
2. 思考实现算法，写出 class field
3. 实现 API，先实现最简单的，再实现比较复杂的

```java
class Stack {
   private ListNode head;
   private int length;

   public Stack() { }

   public Integer pop() {
      if (head == null) {
         return null;
      }

      ListNode node = head;
      head = head.next;
      length--;

      res.next == null; // best practice
      return node.value;
   }

   public Integer peek() {
      if (head == null) {
         return null;
      }

      return head.value;
   }

   public boolean push(int val) {
      ListNode newHead = new ListNode(val);
      newHead.next = head;
      head = newHead;
      length++;

      return true;
   }

   public int size() {
      return length;
   }

   public boolean isEmpty() {
      return length == 0;
   }
}
```

### Queue by Linked List

```java
class Queue {
   private ListNode head;
   private ListNode tail;
   private int length;

   public Queue() { }

   public Integer poll() {
      if (head == null) {
         return null;
      }

      ListNode node = head;
      head = head.next;
      if (head == null) { // check
         tail == null;
      }
      node.next == null;
      length--;

      return node.value;
   }

   public Integer peek() {
      if (head == null) {
         return null;
      }

      return head.value;
   }

   public boolean offer(int val) {
      if (head == null) {
         head = new ListNode(ele);
         tail = head;
      } else {
         tail.next = new ListNode(val);
         tail = tail.next;
      }
      length++;

      return true;
   }

   public int size() {
      return length;
   }

   public boolean isEmpty() {
      return length == 0;
   }
}
```

### Queue by Array

**Circle Array (ring buffer):** we can connect the start and end of the array, so that it is a cycle.

```java
class Queue {
   private int[] arr;
   private int head;
   private int tail;

   public Queue(int length) {
      arr = new int[length + 1];
      tail = 1;
   }

   public boolean offer(int element) {
      if (isFull()) return false;
      arr[tail] = element;
      tail = (tail + 1) % arr.length;
      return true;
   }

   public Integer peek() {
      return isEmpty() ? null : arr[(head + 1) % arr.length];
   }

   public Integer poll() {
      return isEmpty() ? null : arr[head = (head + 1) % arr.length];
   }

   public int size() {
      int size = tail - head - 1;
      return size < 0 ? size + arr.length : size;
   }

   public boolean isEmpty() {
      return (head + 1) % arr.length == tail;
   }

   public boolean isFull() {
      return head == tail;
   }
}
```

---

## Linked List

`ListNode Class` 的定义:

```java
class ListNode {
   public int value; // the storage value
   public ListNode next; // it is reference, or, it is an address

   public ListNode(int x) {
      this.value = x;
   }
}
```

`ListNode1 -> ListNode2 -> ListNode3 -> null`

**key points:**

1. When you want to access value/next of a ListNode, **make sure it is not null pointer**.
2. Never ever lose the control of the head pointer of the LinkedList.

### Array vs. LinkedList

- Memory Layout
  - Array: consecutive allocated memory space, no overhead (额外开销)
  - Linked List: non-consecutive, overhead of multiple objects with the "next"
- (Random) access time

**<font color=#3273DC>Example.1 Given a linked list, find the index - k element of it.</font>**

```java
public class FindKth {
   public static ListNode findKIndex(ListNode head, int k) {
      if (head == null || k < 0) {
         return null;
      }

      ListNode cur = head;
      int index = 0;

      while (index != k && cur != null) {
         cur = cur.next;
         index++;
      }
      return cur;
   }
}
```

**<font color=#3273DC>Example.2 How to find the middle node of a linked list ?</font>**

快慢指针法：slow 每次走一步，fast 每次走两步

```java
public ListNode findMidNode(ListNode head) {
   if (head == null) {
      return head;
   }

   ListNode slow = head;
   ListNode fast = head;

   while (fast.next != null && fast.next.next != null) {
      slow = slow.next;
      fast = fast.next.next;
   }
   return slow;
}
```

**<font color=#3273DC>Example.3 Insert a node in a sorted linked list.</font>**

```java
public ListNode insert(ListNode head, int value) {
   ListNode newNode = new ListNode(value);
   if (head == null) {
      return newNode;
   }

   if (head.value >= value) {
      newNode.next = head;
      return newNode;
   }

   ListNode cur = head;
   while (cur.next ！= null && cur.next.value < value) {
      cur = cur.next;
   }
   newNode.next = cur.next;
   cur.next = newNode;

   return head;
}
```

**<font color=#3273DC>Example.4 How to merge two sorted LinkedList into one long sorted LinkedList ?</font>**

```java
public ListNode merge(ListNode head1, ListNode head2) {
   ListNode dummyHead = new ListNode(0);
   ListNode cur1 = head1;
   ListNode cur2 = head2;
   ListNode curr = dummyHead;

   while (cur1 != null && cur2 != null) {
      if (cur1.value < cur2.value) {
         curr.next = cur1;
         cur1 = cur1.next;
      } else {
         curr.next = cur2;
         cur2 = cur2.next;
      }
      curr = curr.next;
   }

   if (cur1 == null) {
      curr.next = cur2;
   }
   if (cur2 == null) {
      curr.next = cur1;
   }

   return dummyHead.next;
}
```

- TC: O(n)
- SC: O(1)

**<font color=#3273DC>Example.5 Remove nodes with target value in the LinkedList.</font>**

```java
public ListNode removeNodes(ListNode head, int target) {
   ListNode dummyHead = new ListNode(0);
   dummyHead.next = head;
   ListNode pre = dummyHead;
   ListNode cur = head;
   while (cur != null) {
      if (cur.value == target) {
         pre.next = cur.next;
      } else {
         pre = cur;
      }
      cur = cur.next;
   }

   return dummyHead.next;
}
```

- TC: O(n)
- SC: O(1)

**<font color=#3273DC>Example.6 Reverse a LinkedList.</font>**

**method 1: iterative**

```java
public ListNode reverseIterative(ListNode head) {
   ListNode cur = head;
   ListNode pre = null;
   while (cur != null) {
      ListNode next = cur.next;
      cur.next = pre;
      pre = cur;
      cur = next;
   }

   return pre;
}
```

**method 2: recursion**

- subproblem: reverse(head) -> reverse(head.next)
- base case: only 1 node or 0 node

```java
public ListNode reverseRecursion(ListNode head) {
   // base case
   if (head == null || head.next == null) {
      return head;
   }

   ListNode newHead = reverseRecursion(head.next);
   head.next.next = head;
   head.next = null;
   return newHead;
}
```

- TC: O(n)
- SC: O(n)

---

## Sorting Algorithms

- **Selection sort**
- **Merge sort**
- **Quick sort**
- Bubble sort
- Bucket sort
- Patient sort
- Smooth sort
- Cocktail sort
- ...

### Selection Sort

- sorted range: `[0, i)`
- unsorted range: `[i, length - 1]`
- stop condition: `i < n - 1` (we don't have to process the last element)

```java
class SelectionSort {
   public void selectionSort(int[] arr) {
      if (arr == null || arr.length < 1) {
         return;
      }

      for (int i = 0; i < arr.length; i++) {
         int min = i;
         for (int j = i; j < arr.length; j++) {
            if (arr[j] < arr[min]) {
               min = i;
            }
         }
         swap(arr, i, min);
      }
   }

   private void swap(int[] arr, int x, int y) {
      int temp = arr[x];
      arr[x] = arr[y];
      arr[y] = temp;
   }
}
```

- TC: O(n<sup>2</sup>)
- SC: O(1)

### Merge Sort

```java
class MergeSort {
   public int[] mergeSort(int[] arr) {
      // corner case
      if (arr == null || arr.length <= 1) {
         return arr;
      }

      return mergeSort(arr, 0, arr.length - 1);
   }

   private int[] mergeSort(int[] arr, int left, int right) {
      // base case
      if (left == right) {
         return new int[] {arr[left]};
      }

      // subproblem
      int mid = left + (right - left) / 2;
      int[] leftRes = mergeSort(arr, left, mid);
      int[] rightRes = mergeSort(arr, mid + 1, right);

      // recursion rule
      return merge(leftRes, rightRes);
   }

   private int[] merge(int[] leftRes, int[] rightRes) {
      int[] res = new int[leftRes.length + rightRes.length];
      int i = 0;
      int j = 0;
      int k = 0;

      while (i < leftRes.length && j < rightRes.length) {
         if (leftRes[i] < rightRes[j]) {
            res[k] = leftRes[i++];
         } else {
            res[k] = rightRes[j++];
         }
         k++;
         // res[k++] = leftRes[i] < rightRes[j] ? leftRes[i++] : rightRes[j++];
      }

      // case1: left res has some mot merge, right res doesn't
      // case2: right res has some mot merge, left res doesn't
      while (i < leftRes.length) {
         res[k++] = leftRes[i++];
      }
      while (j < rightRes.length) {
         res[k++] = rightRes[j++];
      }

      return res;
   }
}
```

- subproblem: mergeSort(arr, left, mid), mergeSort(arr, mid + 1, right)
- recursion rule: merge(left, right)
- base case: left == right
  <br/>
- TC: O(n) + O(nlogn) = O(nlogn)
- SC: stack + heap = O(logn) + O(n) = O(n)

![](/assets/algorithm/01.png)

### Quick Sort

```java
class QuickSort {
   private Random random = new Random();

   public void quickSort(int[] arr) {
      // corner case
      if (arr == null || arr.length <= 1) {
         return;
      }
      quickSort(arr, 0, arr.length - 1);
   }

   public void quickSort(int[] arr, int left, int right) {
      // base case
      // [a] -> left = right
      // [] -> left > right (pivot is the first or the last element of the array)
      if (left >= right) {
         return;
      }

      // step 1: choose pivot
      // random.nextInt(x) -> a random integer in [0, x) -------
      // goal: a random number (pivot index) in [left, right]  |
      // [left, right] -> left + [0, right - left]             |
      //               -> left + [0, right - left + 1) <--------
      int pivotIndex = left + random.nextInt(right - left + 1);
      swap(arr, pivotIndex, right);

      // step 2: partition
      int i = left;
      int j = right - 1;
      while (i <= j) {
         if (arr[i] < arr[right]) {
            i++;
         } else {
            swap(arr, j, i);
            j--;
         }
      }

      // step 3: put the pivot back
      swap(arr, i, right);

      // recursion rule
      quickSort(arr, left, i - 1);
      quickSort(arr, i + 1, right);
   }
}
```

- TC:
  - worst case: O(n<sup>2</sup>)
    ![](/assets/algorithm/02.png)
  - average case: O(nlogn)
    ![](/assets/algorithm/03.png)
- SC:
  - worst case: O(n)
  - average case: O(logn)

### Rainbow Sort

Dutch Flag Problem 荷兰旗问题 (only 3 kinds of numbers)
![](/assets/algorithm/04.png)

```java
class RainbowSort {
   public void rainbowSort(int[] arr) {
      // corner case
      if (arr == null || arr.length <= 1) {
         return;
      }

      int i = 0, j = 0, k = arr.length - 1;
      while (j <= k) {
         if (arr[j] == 1) {
            swap(arr, i, j);
            i++;
            j++;
         } else if (arr[j] == 2) {
            j++:
         } else {
            swap(arr, j, k);
            k--;
         }
      }
   }
}
```

- TC: O(n)
- SC: O(1)

---

## Binary Tree

<u>at most two children node</u>

```java
class TreeNode {
   int value; // the storage value
   TreeNode left; // by default = null
   TreeNode right; // by default = null
}
```

**LeafNode:** both rigth and left are null

**Tree Traverse:**

1. pre-order: current node before its subtress **(self -> left -> right)**
2. in-order: current node in its subtrees **(left -> self -> right)**
3. post-order: current node after its subtrees **(left -> right -> self)**

**Base Concept:**

1. **Height of binary tree:** The distance between the root with the deepest leaf node.
2. **Balanced binary tree:** is commonly defined as a bnary tree in which the depth (alse known as height) of the left and right subtrees of **every node** differ by 1 or less.
   1. for each of the nodes in this binary tree
   2. satisfyL the height of lesf subtree, right subtree at most diff by 1.
      > **If a tree has n number of nodes and it is balanced, the the heght (level of the tree = O(logn)).**
3. **Complete binary tree:** is a binary tree in which every level, _except possibly the last_, is completely filled, and all nodes are as far left as possible.
   > **If a tree is a complete tree, then it must be a balanced tree.**
4. **Binary Search Tree:** for every single node in the tree, the values in its left subtree are all smaller than its value, and the values in its right subtree are all larger than its value.
   > **If we print the value of the nodes in BST in in-order sequence, then it must from an ascending order.**

<br/>

**<font color=#3273DC>Example.1 Get the height of a binary tree.</font>**

```java
int getHeight(TreeNode root) {
   if (root == null) {
      return 0;
   }

   int leftHeight = getHeight(root.left);
   int rightHeight = getHeight(root.right);

   return Math.max(leftHeight, rightHeight) + 1;
}
```

**<font color=#3273DC>Example.2 How to determine whether a binary tree is a balanced binary tree ?</font>**

```java
boolean isBalanced(TreeNode root) {
   if (root == null) {
      return true;
   }

   if (Math.abs(getHeight(root.left) - getHeight(root.right) > 1) {
      return false;
   }

   return isBalanced(root.left) && isBalanced(root.right);
}
```

**<font color=#3273DC>Example.3 How to determine whether a binary tree is symmetric ?</font>**

```java
boolean isSymmetric(TreeNode left, TreeNode right) {
   if (left == null && right == null) {
      return true;
   } else if (left == null || right == null) {
      return false;
   } else if (left.value != right.value) {
      return false;
   }

   return isSymmetric(left.left, right.right) && isSymmetric(right.left, left.right);
}
```

- TC: O(n)
- SC: O(height)

**<font color=#3273DC>Example.4 LCA</font>**
Given two nodes in a binary tree (with parent pointer available), find their lowest common ancestor.

- There is parent pointer for the nodes in the binary tree
- The given two nodes **are not guaranteed to be** in the binary tree

```java
class Solution {
   public TreeNode findLCA(TreeNode one, TreeNode two) {
      if (one == null || two == null) {
         return null;
      }

      int depth1 = getDepth(one), depth2 = getDepth(two);
      if (depth1 > depth2) {
         return findLCA(one, two, depth1 - depth2);
      } else {
         return findLCA(two, one, depth2 - depth1);
      }
   }

   private int getDepth(TreeNode node) {
      int depth = -1;
      while (node != null) {
         depth++;
         node = node.parent;
      }

      return depth;
   }

   private TreeNode findLCA(TreeNode longer, TreeNode shorter, int diff) {
      while (diff > 0) {
         longer = longer.parent;
         diff--;
      }

      while (longer != shorter) {
         longer = longer.parent;
         shorter = shorter.parent;
      }

      return longer;
   }
}
```

### Binary Tree Iteration (Traversal)

---

## Binary Search Tree (BST)

For every single node in the tree, the values in its left subtree are all smaller than its value, and the values in its right subtree are all larger than its value.

- search() - O(h), worst case O(n), best O(logn)
- insert() - O(h), worst case O(n), best O(logn)
- remove() - O(h), worst case O(n), best O(logn)

**<font color=#3273DC>Example.1 Search in BST.</font>**

**method 1: iteration**

```java
TreeNode search(TreeNode root, int target) {
   TreeNode cur = root;
   while (cur != null && cur.value != targer) {
      if (target < cur.value) {
         cur = root.left;
      } else if (target > cur.value) {
         cur = root.right;
      }
   }

   return cur;
}
```

- TC: O(h)
- SC: O(1)

**method 2: recursion**

```java
// tail recursion
TreeNode search(TreeNode root, int target) {
   if (root == null || root.value == target) {
      return root;
   }

   if (target < root.value) {
      return search(root.left, target);
   }

   return search(root.right, target);
}
```

- TC: O(h)

**<font color=#3273DC>Example.2 Insert in BST.</font>**

**method 1: iteration**

```java
TreeNode insert(TreeNode root, int targer) {
   if (root == null) {
      return new TreeNode(targer);
   }
   TreeNode cur = root;
   TreeNode pre = null;
   while (cur != null) {
      if (target < cur.value) {
         if (cur.left != null) {
            cur = cur.left;
         } else {
            cur.left = new TreeNode(target);
            break;
         }
      } else {
         if (cur.right != null) {
            cur = cur.right;
         } else {
            cur.right = new TreeNode(target);
         }
      }
   }
   return root;
}
```

**method 2: recursion**

```java
TreeNode insert(TreeNode root, int targer) {
   if (root == null) {
      return new TreeNode(target);
   }

   if (target < root.value) {
      root.left = insert(root.left, target);
   } else if (target > root.value) {
      root.right = insert(root.right, target);
   }
   return root;
}
```

**<font color=#3273DC>Example.3 Delete in BST.</font>**

- case 1: The node to be deleted has no child
- case 2: The node to be deleted has no left child
- case 3: The node to be deleted has no right child
- case 4: The node to be deleted has both left and right child. We need to move some nodes from left / right subtree to replace it.
  - case 4.1: node.right does not have left child, meaning itself is the smallest node in this case, we just move node.right up
  - case 4.2: node.right has left child, we need to find the smallest node, and move it up

```java
class Solution {
   public TreeNode delete(TreeNode root, int target) {
      if (root == null) {
         return null;
      }

      // find target node
      if (root.value > target) {
         root.left = delete(root.left, target);
         return root;
      } else {
         root.right = delete(root.right, target);
         return root;
      }

      //guarantee root != null && troot.value == target
      if (root.left == null) { // case 1&2
         return root.right;
      } else if (root.right == null) { // case 3
         return root.left;
      }

      //guarantee root.left != null && rot.right != null
      //case 4.1
      if (root.right.left == null) {
         root.right.left = root.left;
         return root.left;
      }

      //case 4.2
      // 1. find and delete smallest node in root right
      TreeNode smallest = deleteSmallest(root.right);
      // connect the smallest node with root.left and root.right
      smallest.left = root.left;
      smallest.right = root.right;
      // return the smallest node
      return smallest;
   }

   private TreeNode deleteSmallest(TreeNode cur) {
      TreeNode prev = cur;
      cur = cur.left;
      while (cur.left != null) {
         prev = cur;
         cur = cur.left;
      }
      // cur is the smallest one, and prev is its parent
      // Invariance: cur (prev.left) does not have left child
      prev.left = prev.left.right; // cur.right
      return cur;
   }
}
```

- TC: O(h) find(upper part of height) + findSmallest(lower part of height)
- SC: O(h) on call stack

---

## Graph

### General Tree

Each node can have an arbitrary number of children

```java
class TreeNode {
   int key;
   List<TreeNode> children;
   public TreeNode(int key) {
      this.key = key;
      children = new ArrayList<TreeNode>();
   }
}
```

We use the `root` to represent the general tree.

> G = V + E

### Graph

Tree is a special kind of **Graph**

```java
class GraphNode {
   int key;
   List<GraphNode> neighbors;
   public GraphNode(int key) {
      this.key = key;
      neighbors = new ArrayList<GraphNode>();
   }
}
```

We use the `List<GraphNode>` to represent the general tree.

---

## Heap & Graph Search Algorithms

### Heap

**堆**，亦被称为**优先队列**

**用途：** 维护一个**变化**的数据集的**最优值**

**性质：** 堆的数显通过构造二叉堆 (binary heap)， 这种数据结构具有以下性质

1. 堆总是一颗**完全二叉树 (complete binary tree)**
2. 根节点最小的堆叫做**MIN HEAP**，根节点最大的堆叫做**MAX HEAP**
   - MIN HEAP: 任意节点**小于(等于)** 它的所有后裔 (descendent) (堆序性)
   - MAX HEAP: 任意节点**大于(等于)** 它的所有后裔 (descendent) (堆序性)

**Index of `parent = i`, what is the index of the two child nodes?**

- left child of index `i = 2 * i + 1`
- right child of index `i = 2 * i + 2`
- parent of index `i = (i - 1) / 2`

**Operations:**

1. **insert:** 向堆中插入一个新元素；TC: O(logn)
2. **update:** 将新元素提升使其符合堆的性质；TS: O(logn)
3. **get / top:** 获取当前堆顶元素的值；TC: O(1)
4. **pop:** 删除堆顶元素；TC: O(logn)
5. **heapify:** 使得一个 unsorted array 变成一个堆；TC:O(n)

**<font color=#3273DC>Example. Find smallest k elements from an unsorted array of size n.</font>**

**method 1: sort**

- TC: O(nlogn)

**method 2: min heap**

1. Heapify all elements -> O(n)
2. Call `pop()` k times to get the k smallest elements -> O(klogn)

- TC: O(n + klong)

**method 3: max heap**

1. Heapify the first k elements to form a MAX HEAP of `size = k` -> O(k)
   Alternatively, call `insert()` k times instead of heapify -> O(klogk)
2. Iterate over the reaining (n - k) elements one by one.
   When we tracerse a new element:
   compare with the **largest** element of the previous smallest k candidates
   1. case 1: new element >= top: **ignore**
   2. case 2: new element < top: **update (top -> new element)**
      O((n - k)logk)

- TC: O(k + (n - k)logk)

### Breadth-First Search (BFS-1)

**BFS 的操作过程 & How to describe a BFS's action during an interview?**

- **<font color=red>Data Structure:</font>** Maintain a **FIFO queue**, put all traversed nodes that haven't been expanded.
- **Initial state**
- **Expand** a node
- **Generate** s's neighbor nodes: reach out to its neighboring nodes
- **termination condition:** do a loop until the queue is empty
- Optionally **deduplicate** visited nodes (typically for graph not for tree)

### Best First Search (BFS-2)

1. **Usages:** Find the **shortest path cost** from a single node (source node) to <font color=red>any other nodes in that graph</font> (点道面(==所有点)的最短距离算法)
2. **Data Structure:** `PriorityQueue` (minHeap)

**解题思路**

1. Initial state: (start node)
2. Node expansion / Generation rule
3. Termination condition: 所有点都计算完毕才停止 -> PriorityQueue 为空

### Heap in Java (PriorityQueue)

#### PriorityQueue

It is a heap with same Queue interface with `offer(), peek(), poll()`.
But, it is not FIFO, when `poll()` or `peek()` we always look at the smallest / largest element (min heap / max heap).
Internally it is implemented using an array.

#### 概念区分

| 数据结构 (逻辑层面)  | 内存里的存放方法      | 对应 java class         | 对应 java interface |
| -------------------- | --------------------- | ----------------------- | ------------------- |
| queue (FIFO)         | array / linked list   | ArrayDeque / LinkedList | Queue               |
| stack (LIFO)         | array / linked list   | ArrayDeque / LinkedList | Deque               |
| deque (double-ended) | array / linked list   | ArrayDeque / LinkedList | Deque               |
| heap (tree like)     | array (abstract tree) | PriorityQueue           | Queue               |

#### Order

The PriorityQueue need to know how to compare the elements and determine which one is smaller / larger.

- **The element type implementing Comparable interface**
  The element's class can implement `Comparable interface` and thus implement the required method `compareTo()`, PriorityQueue will use this method to compare any tow elements.

```java
interface Comparable<E> {
   int compareTo(E ele);
}
```

```java
// part of the Integer class implementation...
class Integer implements Comparable<Integer> {
   private int value;

   public Integer(int value) {
      this.value = value;
   }

   @Override
   public int compareTo(Integer another) {
      if (this.value == another.value) {
         return 0;
      }
      return this.value < another.value ? -1 : 1;
   }
}
```

- **Provide an extra Comparator object to compare the elements**
  There is another interface `Compartor`, it is used to compare two elements with same type E.

```java
interface Comparator<E> {
   int compare(E o1, E o2);
}
```

```java
class Cell {
   public int row;
   public int col;
   public int value;
   public Cell(int row, int col, int value) {
      this.row = row;
      this.col = col;
      this.value = value;
   }
}

class MyComparator implements Comparator<Cell> {
   @Override
   public int compare(Cell c1, Cell c2) {
      if (c1.value == c2.value) {
         return 0;
      }
      return c1.value < c2.value ? -1 : 1;
   }
}
```

- **MIN HEAP & MAX HEAP**
  There is a utility method `Collections.reverseOrder()`, it will return a comparator that **reverses the natural order**.

```java
// min heap
PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>();

// max heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(Collections.reverseOrder());
```

#### Most frequently used constructors of PriorityQueue

1. `PriorityQueue<Cell> heap = new PriorityQueue<Cell>();`
   - initialize the internal array with default capacity(11)
   - **class `Cell` must implements `Comparable<Cell>`**
2. `PriorityQueue<Cell> heap = new PriorityQueue<Cell>(16);`
   - initialize the internal array with specified capacity(16)
   - **class `Cell` must implements `Comparable<Cell>`**
3. `PriorityQueue<Cell> heap = new PriorityQueue<Cell>(16, new MyComparator());`
   - initialize the internal array with specified capacity(16)
   - **class `MyComparator` must implements `Comparator<Cell>`**
4. `PriorityQueue<Cell> heap = new PriorityQueue<Cell>(new MyComparator());`
   - **class `MyComparator` must implements `Comparator<Cell>`**
   - **Java 8+**

#### Nested Classes

A class within another class:

- It is a way of logically grouping classes that are only used in one place
- It increases encapsulation
- It can lead to more readable and maintainable code

```java
class LinkedList {
   // Nested class
   class ListNode {
      // ...
   }
}
```

#### Anonymous inner class (define in a method with just new and no definition)

```java
class Solution {
   PriorityQueue<Cell> pQueue = new PriorityQueue<>(16, new Comparator<Cell>() {
      @Override
      public int compare(Cell c1, Cell c2) {
         if (c1.value == c2.value) {
            return 0;
         }
         return c1.value < c2.value ? -1 : 1;
      }
   })
}
```

1. 实现一个接口
2. 定义一个类
3. 创建一个 instance
4. call constructor

**Similar to:**

```java
class MyComparator implements Comparator<Cell> {
   @Override
   public int compare(Cell c1, Cell c2) {
      if (c1.value == c2.value) {
         return 0;
      }
      return c1.value < c2.value ? -1 : 1;
   }
}

PriorityQueue<Cell> pQueue = new PriorityQueue<>(16, new MyComparator())
```

**Lambda (Java 8)**

```java
PriorityQueue<Cell> pQueue = new PriorityQueue<>(16,
   (Cell c1, Cell c2) -> {
      if (c1.value == c2.value) {
         return 0;
      }
      return c1.value < c2.value ? -1 : 1;
   }
)
```

**<font color=#3273DC>Example. Smallest k elements in unsorted array.</font>**
Find the K smassles numbers in an unsorted integer array A. The returned numbers should

**method 1: MIN HEAP**

```java
public int[] findKSmallest(int[] arr, int k) {
   if (k > arr.length) {
      return null;
   }

   PriorityQueue<Integer> minHeap = new PriorityQueue<Integer>();
   // TC: O(nlogn)
   for (int num: arr) {
      minHeap.offer(num);
   }

   int[] res = new int[k];
   // TC: O(klogn)
   for (int i = 0; i < k; i++) {
      res[i] = minHeap.poll();
   }

   return res;
}
```

- TC: O(nlogn) + O(klogn) = P(nlogn)

**method 2: MAX HEAP**

```java
public int[] findKSmallest(int[] arr, int k) {
   if (k > arr.length) {
      return null;
   }
   PriorityQueue<Integer> maxHeap = new PriorityQueue<Integer>(Collections.reverseOrder());
   for (int i = 0; i < k; i++) {
      maxHeap.offer(arr[i]);
   }

   int[] res = new int[k];
   for (int i = 0; i < k; i++) {
      res[i] = minHeap.poll();
   }

   return res;
}
```

- TC: O((n + k)logk)

### Implementing Heaps

#### Basic Heap Internal Operations

##### `percolateUp()`

- When to use?
  The element need to be moved up to maintain the heap's property, for example, when offering a new element into the heap.
- How?
  Compare the element with its parent, move it up when necessary, do this until the element does not need to be moved.
- **TC: O(logn)**

##### `percolateDown()`

- When to use?
  The element need to be moved down to maintain the heap's property, for example, when poll the root element from the heap.
- How?
  Compare the element with its two children, if the smallest one of the two children is smaller than the element, swap the element with that child, do this until the element does not need to be moved.
- **TC: O(logn)**

##### `heapiyf()`

- Convert an array into a heap in **O(n)** time.
- How?
  For each node that has at least one child, we perform `percolateDown` action, in the order of from the nodes on the deepest level to the root.
- **TC: O(n)**

> 原理： 当一个 node 左子树和右子树都是堆，对它本身做 `percolateDown`，会使得以它为 root 的整颗子树成为堆。

The range of indices nees to perform `percolateDown` is: **[0, n / 2 - 1]**
**Last non-leaf node: parent of last node `index = ((n - 1) - 1) / 2 = n / 2 - 1`**

##### `update()`

- If you know the position of the element you want to update, it will take **O(logn)**
- How?
  Either you need `percolateUp`, or `percolateDown` on that element
- Waht if you do not know the position of the element?
  You need to find the position of the element first, if not asking for help with other additional data structure, this operation is **O(n)**

#### Implementation of Min Heap

```java
public class MinHeap {
   private int[] array;
   private int size;
   public MinHap(int[] array) {
      if (array == null || array.length == 0) {
         throw new IllegalArgumentException("input array can not be null or empty");
      }
      this.array = array;
      size = array.length;
      heapify();
   }

   private void heapify() {
      for (int i = size / 2 - 1; i >= 0; i--) {
         percolateDown(i);
      }
   }

   public MinHeap(int cap) {
      if (cap <= 0) {
         throw new IllegalArgumentException("capacity can not be <= 0");
      }
      array = new int[cap];
      size = 0;
   }

   public int size() {
      return size;
   }

   public boolean isEmpty() {
      return size == 0;
   }

   public boolean isFull() {
      return size == array.length;
   }

   private void percolateUp(int index) {
      if (index < 0 || index >= size) {
         return;
      }

      while (index > 0) {
         int parent = (index - 1) / 2;
         if (array[parent] > array[index]) {
            swap(parent, index);
         } else {
            break;
         }
         index = parent;
      }
   }

   private void percolateDown(int index) {
      while (index <= size / 2 -1) {
         int left = index * 2 + 1;
         int right = index * 2 - 1;
         int toSwap = left;
         if (right < size && array[left] >= array[right]) {
            toSwap = right;
         }
         if (array[index] > array[toSwap]) {
            swap(index, toSwap);
         } else {
            break;
         }
         index = toSwap;
      }
   }

   public Integer peek() {
      if (size == 0) {
         return null;
      }
      return array[0];
   }

   public Integer poll() {
      if (size == 0) {
         return null;
      }
      int res = array[0];
      array[0] = array[size - 1];
      size--;
      percolateDown(0);
      return res;
   }

   public void offer(int ele) {
      if (size == array.length) {
         array = Arrays.copyOf(array, (int)(array.length * 1.5));
      }
      array[size] = ele;
      size++;
      percolateUp(size - 1);
   }

   public int update(int index, int ele) {
      if (index > array.length || index < 0) {
         throw new ArrayIndexOutOfBoundsException("Invalid index range");
      }
      int oldVal = array[index];
      array[index] = ele;
      ele < oldVal ? percolateUp(index) : percolateDown(index);
      return oldVal;
   }

   private void swap(int l, int r) {
      int temp = array[l];
      array[l] = array[r];
      array[r] = temp;
   }
}
```

### Graph Search Algorithms - DFS(Depth-First Search)

DFS 基本方法：

1. What does it store on each level?
2. How many different states should we try to put on this level?

**<font color=#3273DC>Example.1 Find subset.</font>**
E.g. Print all subsets of a set Set = {'a', 'b', 'c'}

1. **What does it store on each level?**
   Three levels. For each level, it makes the decision on whether to put this element into the final set.
2. **How many different states should we try to put on this level?**
   Two different states, each state(case) considers either select or not select

![](/assets/algorithm/05.png)

```java
void findSubset(char[] input, int index, StringBuilder solutionPrefix) {
   if (index == input.length) {
      System.out.println(solutionPrefix);
      return;
   }

   // case 1: add inut[index] to the solution prefix
   solutionPrefix.append(input[index]);
   finSubset(input, index + 1, solutionPrefix);
   solutionPrefix.deleteCharAt(solutionPrefix.length() - 1);

   // case 2: do not add input[index] to the solution prefix
   findSubset(input, index + 1, solutionPrefix);
}
```

- TC: O(2<sup>n</sup>)
- SC: O(n) on heap + O(n) on call stack = O(n)

**<font color=#3273DC>Example.1.1 Insert empty space.</font>**
We can choose to insert either one or zero empty space between each paire of adjacent letters. Please print out all possible results.

```java

```

**<font color=#3273DC>Example.2 Find all valid permutation using the parenthesis provided.</font>**

1. **What does it store on each level?**
   Six levels, each level considers one position (in which there will be onle one parenthesis added in this position).
2. **How many different states should we try to put on this level?**
   Two states,either left or right parenthesis.

![](/assets/algorithm/06.png)

```java
void DFS(int n, int l, int r, StringBuilder solutionPrefix) {
   if (l == n && r == n) {
      System.out.println(solutionPrefix); // base case
      return;
   }

   // case 1: add '(' on this level
   if (l < n) {
      solutionPrefix.append('(');
      DFS(n, l + 1, r, solutionPrefix);
      solutionPrefix.deleteCharAt(solutionPrefix.length() - 1);
   }

   // case 2: add '(' on this level
   if (l > r) {
      solutionPrefix.append(')');
      DFS(n, l, r + 1, solutionPrefix);
      solutionPrefix.deleteCharAt(solutionPrefix.length() - 1);
   }
}
```

- TC: O(2<sup>2n</sup>)
- SC: O(2n) = O(n)

**<font color=#3273DC>Example.3 Print all combinations of coins that can sum uo to total value n.</font>**
E.g. total value n = 99 cents
coin value = 25 10 5 1 cent

1. **What does it store on each level?**
   Four levels, each level considers on type of coin
2. **How many different states should we try to put on this level?**
   Dynamically changes

![](/assets/algorithm/07.png)

```java
void findCombination(int[] coin, int moneyLeft, int index, int[] sol) {
   if (index == coin.length - 1) {
      sol[index] = moneyLeft;
      // print solution and return
   }

   for (int i = 0; i <= moneyLeft / coin[index]; i++) {
      sol[index] = i;
      findCombination(coin, moneyLeft - i * coin[index], index + 1, sol);
   }
}
```

- TC: O(k<sup>n</sup>)

**<font color=#3273DC>Example.4 Given a string with no duplicate letters, how to print out all permutations of the string.</font>**

1. **What does it store on each level?**
   Three levels, each level represents on position
2. **How many different states should we try to put on this level?**
   Rmaining unused letter

![](/assets/algorithm/20.png)

```java
void permutation(char[] input, int index) {
   if (index == input.length) {
      System.out.println(input);
      return;
   }

   // put each letter in the index-th position of the input str
   for (int i = index; i < input.length; i++) {
      swap(input, index, i);
      permutation(input, index + 1);
      swap(input, index, i);
   }
}
```

- TC: O(n!)
- SC: O(n)

**<font color=red>Conclusion:</font>** whenever every single permutation contains all elements in the initial input, the we should consider **SWAP** and **SWAP**

---

## Set, Map & String

### Set

A collection that **can not contain duplicate elements**.

- `HashSet`: Which stores its elements in a **hashtable**, is the best-performing implementation, however it makes **no guarantees concerning the order of iteration**.
- `TreeSet`: Which stores it elements in a red-black tree (balanced binary search tree), orders its elements based on their value
- `LinkedHashSet`: It is a HashSet and also it is a LinkedList, it maintains the order when each of the elements is inserted into the HashSet

### Map

A collection that maps keys to values. A `Map` cannot contain duplicate keys; each key can map to one value.

- `HashMap`
- `TreeMap`
- `LinkedHashMap`

### HashMap

#### Common API

- `V put(K key, V value)`
- `V get(Object key)`
- `V remove(Object key)`
- `boolean containsKey(Object key)`
- `boolean containsValue(Object value)`
- `void clear()`
- `int size()`
- `boolean isEmpty()`

**Time Complexity**

| Operation                                                                 | Average | Worst (<= JDK 7) |
| ------------------------------------------------------------------------- | ------- | ---------------- |
| search: <br/> `boolean containsKey(Object key)` <br/> `V get(Object key)` | O(1)    | O(n)             |
| insert / update: <br/> `V put(K key, V value)`                            | O(1)    | O(n)             |
| delete: <br/> `V remove(Object key)` <br/> `V get(Object key)`            | O(1)    | O(n)             |

#### HashMap Implementation

1. define the class for each entry

   ```java
   class Node<K, V> {
      private final K key;
      private V value;
      Node<K, V> next;
      Node(K key, V value) {
         this.key = key;
         this.value = value;
      }

      public K getKey() {
         return key;
      }

      public V getValue() {
         return value;
      }

      public void setValue(V value) {
         this.value = value;
      }
   }
   ```

2. Maintain an array of entries
   `Node<K, V>[] array;`
3. Hash(key) to get the hash#
   ```java
   private int hash(K key) {
      // return the hash# of the key
      if (key == null) {
         return 0;
      }
      int hashNumber = key.hashCode();
      return hashNumber & 0x7FFFFFFF;
   }
   ```
4. From the hash#, mapped to the entry index.

   ```java
   int getIndex(int hashNumber) {
      // reutrn the corresponding index of array
      return hashNumber % array.length;
   }
   ```

5. When iterate the corresponding entry for the given key, which is actually a singly linked list, we need compare each of the entry in the list, if **the key is the same** as the key we want.
   ```java
   Node<k, V> cur = array[index];
   while (cur != null) {
      K curKey = cur.getKey();
      if (curKey is the same as given key) {
         //...
      }
   }
   ```

**Implementation:**

- APIs: `put`, `get`, `remove`, `size`, `isEmpty`
- Fields: array[], size
- Constructor: capacity, load factor (threshold), no parameter

TODO:

```java
class HashMap<K, V> {
   private int size;
   private Node[] buckets;
   private double loadFactor;

   HashMap() {

   }
}
```

### String

**<font color=#3273DC>Example.1 Remove a/some particular chars from a string in place</font>**
E.g. string input = "student", remove "u" and "n" -> output: "stdet" (in place)

快慢指针，同向而行

```java
void removeChar(StringBuilder input) {
   int slow = 0;
   for (int fast = 0; fast < input.length; fast++) {
      if (input.charAt(fast) != 'u' && input.charAt(fast) != 'n') {
         input.setCharAt(slow, input.charAt(fast));
         slow++;
      }
   }
   input.delete(slow, input.length);
}
```

**<font color=#3273DC>Example.2 Remove all leading/trailing and duplicate empty spaces (only leave onw empty space if duplicated spaces happen) from the input string. (must in place)</font>**
E.g. input = "\_\_\_abc_de\_\_\_" -> output = "abc_de"

```java

```

**<font color=#3273DC>Example.2.1 Remove duplicated and adjacent letters (leave only one letter in each duplicated section) in a string</font>**
E.g. input = "aabbbbazw" -> output = "abazw"

```java

```

**<font color=#3273DC>Example.2.2 Char de-duplication adjacent letters repeatedly</font>**
E.g. input = "abbbazw" -> output = "zw"

**method 1: Stack**

```java

```

**method 2: slow & fast**

```java

```

**<font color=#3273DC>Example.3 Sub-string Finding</font>**
How to determine whether a string is a substring of another string.

```java
public int subStr(String text, String pattern) {
   if (text == null || pattern == null || text.length() < pattern.length()) {
      return -1;
   }
   if (pattern.length() == 0) {
      return 0;
   }

   // i is every possible start index in text to test
   for (int i = 0; i <= text.length() - pattern.length(); i++) {
      int j = 0;
      while (j < pattern.length() && text.charAt(i + j) == pattern.charAt(j)) {
         j++;
      }
      if (j == pattern.length()) {
         return i;
      }
   }
   return -1;
}
```

- TC: O(n<sup>2</sup>)

**<font color=#3273DC>Example.4 String Reversal</font>**
**M1: iteration**

```java

```

**M2: recursion**

```java

```

**<font color=#3273DC>Example.4.1 Reverse the word.</font>**
E.g. input = "I love yahoo" -> output = "yahpp love I"

**M1**
step1: Reverse the whole sentence.
step2: Reverse every single word.

```java

```

**<font color=#3273DC>Example5.Char Replacement</font>**
E.g. input = "student" -> output = "stu**xx**t" (den -> xx)

```java

```

What if we do not know the size relationship between s1 and s2?
case 1: `s1.length() >= s2.length()`
case 2: `s1.length() < s2.length()`

#### Advanced Topics

1. Shuffling
2. permutation
3. Decoding/encoding
4. Sliding windows using slow/fast pointers

##### String Shuffling

**<font color=#3273DC>Example.1 "A1B2C3D4E5" -> "ABCDE12345"</font>**

```java

```

**<font color=#3273DC>Example.2 "ABCDE12345" -> "A1B2C3D4E5"</font>**

**<font color=#3273DC>Example.3 "ABCDEFG1234567" -> "ABC123DEFG4567"</font>**

```java
void convert(char[] a, int left, int right) {
   // base case
   if (rigth - left <= 1) {
      return;
   }
   int size = right - left + 1;
   int mid = left + size / 2;
   int leftMid = left + size / 4;
   int rightMid = left + size * 3 / 4;

   reverse(a, leftMid, mid - 1);
   reverse(a, mid, rightMid - 1);
   reverse(a, leftMid, rightMid - 1);

   convert(a, left, left + 2 * (leftMid - left) - 1);
   convert(a, left + 2 * (leftMid - left), right);
}
```

##### String permutation

**<font color=#3273DC>Example.1 No duplicate letters in the input string.</font>**
**Solution:** DFS

**<font color=#3273DC>Example.2 Maybe duplicate letters in the input string.</font>**
We need to avoid the same type of letter to be swapped to the index-th position more than once (under the same parent node)

```java

```

##### String En/Decoding

**<font color=#3273DC>Example.1 String Encoding</font>**
E.g. input = "aaaabccaaaa5" -> output = "a4b1c2a5"

**step 1:** From left to right, deal with the cases where the adjacent occurrence of the letters >= 2, which will make the original string shorter, in the meantime record the count of single letters and keep it as it was.
**step 2:** Calculate extra space we need for single letters, resize string.
**step 3:** From right to left, deal with single letters.

```java
class Solution {
   public String strEncoding(String str) {
      if (str == null || str.isEmpty()) {
         return str;
      }
      char[] arr = str.toCharArray();
      return encoding(arr);
   }

   private String encoding(char[] input) {
      // step1 & step2
      int slow = 0, fast = 0;
      int newLength = 0;
      while (fast < input.length) {
         int begin = fast;
         while (fast < input.length && input[fast] == input[begin]) {
            fast++;
         }
         input[slow++] = input[begin];
         if (fast - begin == 1) {
            newLength += 2;
         } else {
            int len = copyDigits(input, slow, fast - begin);
            slow += len;
            newLength += len + 1;
         }
      }

      // step 3
      char[] res = new char[newLength];
      fast = slow - 1;
      slow = newLength - 1;
      while (fast >= 0) {
         if (Character.isDigit(input[fast])) {
            while (fast >= 0 && Character.isDigit(input[fast])) {
               res[slow--] = input[fast--];
            }
         } else {
            res[slow--] = '1';
         }
         res[slow--] = input[fast--];
      }

      return String(res);
   }

   private int copyDigits(char[] input, int index, int count) {
      int len = 0;
      while (int i = count; i > 0; i /= 10) {
         index++;
         len++;
      }
      while (int i = count; i > 0; i /= 10) {
         int digit = i % 10;
         input[--index] = digit;
      }
      return len;
   }
}
```

##### Sliding window in a string (slow + fast indices)

**<font color=#3273DC>Example.1 Longest substring that contains only unique char</font>**
Given a string , returns the length of the longest **<font color=red>substring</font>** **without** duplicate characters.

<!-- TODO: -->

**<font color=#3273DC>Example.2 Find all anagrams(同形异构体) of a substring S2 in a long string S1.</font>**

**M1: use two hashMap**

```java

```

**M2: use one hashMap**
We only use one hashMap t store the information of S2.

```java

```

**<font color=#3273DC>Example.3 Givena 0-1 array, you can flip at most k '0's to '1's. Find the longest subarray that consists of all '1's.</font>**
It's actually a sliding window problem.

---

## Bit Representation & Bit Operations

### Bit Operations (TC: O(1))

- bitwise AND (&)

  ```java
     0b11001110 (-50)
  &  0b10011000 (-104)
  =  0b10001000 (-120)

  byte a = -50;
  byte b = -104;
  byte c = a & b; // c = -120
  ```

- bitwise OR (|)
- NOT (~)
  - a = 5 = 0b00000101
  - b = ~a = 0b11111010
  - -a = (~a) + 1, if a >= 0
- XOR (^)

  - 00 -> 0
  - 11 -> 0
  - 01 or 10 -> 1

  ```java
     0b11001110 (-50)
  ^  0b10011000 (-104)
  =  0b01010110 (86)
  ```

  - `x ^ y = y ^ x`
  - `x ^ (y ^ z) = (x ^ y) ^ z`
  - `x ^ x = 0`
  - `x ^ 0 = x`

- left shift (<<) **右侧补充零**
- right shift (>>) **正数左侧补充零，负数左侧补充 1**

### Building Blocks

**<font color=red>Given an integer x, test whether x's k-th bit is one. (bit tester)</font>**

```java
int x = 0b b7 b6 b5 b4 b3 b2 b1 b0
_x    = 0b b7 b7 b7 b6 b5 b4 b3 b2 (x >> k)
&     = 0b 0  0  0  0  0  0  0  1
      = 0b 0  0  0  0  0  0  0  0


if ((x >> k) & 1 == 0) {
   // b2 is 0
} else {
   // b2 is 1
}
```

**<font color=red>Given an integer x, how to set x's k-th bit to 1? (bit setter)</font>**

```java
// k = 2
int x = 0b b7 b6 b5 b4 b3 b2 b1 b0
|     = 0b 0  0  0  0  0  1  0  0  (1 << k)
      = 0b b7 b6 b5 b4 b3 1  b1 b0

int res = x | (1 << k);
```

**<font color=red>Given an integer x, how to set x's k-th bit to 0? (bit resetter)</font>**

```java
// k = 2
int x = 0b b7 b6 b5 b4 b3 b2 b1 b0
&     = 0b 1  1  1  1  1  0  1  1  (~0b00000100) = ~(1 << k)
      = 0b b7 b6 b5 b4 b3 0  b1 b0

int res = x & ~(1 << k);
```

**<font color=#3273DC>Example.1 Determine whether an integer x is a power of 2.</font>**

```java
boolean isPowerTwo(int x) {
   return (x > 0) && (x & (x - 1) == 0);
}

// debug
// 2的幂次
x     = 0b 00010000
x - 1 = 0b 00001111

// 不是2的幂次
x     = 0b 00010010
x - 1 = 0b 00010001
```

**<font color=#3273DC>Example.2 How to determine the number of bits that are different between two integers?</font>**

```java
int numberOfDifferentBits(int a, int b) {
   int count = 0;
   for (int c = a ^ b; c != 0; c = c >> 1) {
      count += (c & 1);
   }
   return count;
}
```

**<font color=#3273DC>Example.3 Conversion between signed and unsigned, bit extension and bit truncation.</font>**

**<font color=#3273DC>Example.4 Determine whether a string contains unique characters.</font>**
**M1: HashSet**

```java
boolean hasUnique(String str) {
   HashSet<Character> set = new HashSet<Character>();
   for (int i = 0; i < str.length(); ++i) {
      if (set.contains(str.charAt(i))) {
         return false;
      }
      set.add(str.charAt(i));
   }
   return true;
}
```

- TC: O(1)
- SC: O(1)

**M2: boolean[]**

```java
boolean hasUnique(String str) {
   // assume ASCII
   // set[i] 表示ascii为i的字符是否出现过
   boolean[] set = new boolean[256];
   for (int i = 0; i < str.length(); ++i) {
      if (set[str.charAt(i)]) {
         return false;
      }
      set[str.charAt(i)] = true;
   }
   return true;
}
```

- TC: O(1)
- SC: O(1)

**M3: bit**

```java
boolean hasUnique(String str) {
   // int set = 0b b31 b30 b29 ... b0
   // b0 代表 'a' 是否出现
   // b1 代表 'b' 是否出现
   // ...
   // b25 代表 'z' 是否出现
   int set = 0;
   for (int i = 0; i < str.length(); ++i) {
      int k = a.charAt(i) - 97;
      if ((set >> k) & 1 == 1) {
         return false;
      }
      // bit setter
      set = set | (1 << k);
   }
   return true;
}
```

**<font color=#3273DC>Example.5 How to reverse all bits of a numver?</font>**

<!-- TODO: -->

```java
void reverseBits(int num) {

}
```

**<font color=#3273DC>Example.6 Given a non-negative integer x, how to get the hexadecimal representation of the number in string type?</font>**

**M1: 除模**
![](/assets/algorithm/08.png)

```java
String toHex(int num) {
   if (num == 0) {
      return "0x0";
   }
   char[] base = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E','F'};

   StringBuilder sb = new StringBuilder();
   while (num > 0) {
      int remainder = num % 16;
      num = num / 16;
      sb.append(base[remainder]);
   }
   sb.append("x0");
   sb.reverse();
   return sb.toString();
}
```

**M2: 四位二进制变为一位十六进制**
![](/assets/algorithm/09.png)

```java
String toHex(int num) {
   char[] base = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'};

   StringBuilder sb = new StringBuilder();
   for (int maskEnd = 28; maskEnd >= 0; maskEnd -= 4) {
      char digit = base[(num >> maskEnd) & 0xF];
      sb.append(digit);
   }
   return sb.toString();
}
```

### Bit Representation for Integers

|                        |    min    |   max    | count |
| ---------------------- | :-------: | :------: | :---: |
| 原码(true)             | 1111 (-7) | 0111 (7) |  15   |
| 反码(one's complement) | 1000 (-7) | 0111 (7) |  15   |
| 补码(two's complement) | 1000 (-8) | 0111 (7) |  16   |

one's complement can only represent **-(2<sup>N-1</sup> - 1)** to **(2<sup>N-1</sup> - 1)** unding N bits
two's complement can represent **-2<sup>N-1</sup>** to **2<sup>N-1</sup>** using N bits

### ">>>" vs. ">>"

- unsigned shift ">>>" - logical
  ![](/assets/algorithm/10.png)
- signed shift ">>" - arithmetical
  ![](/assets/algorithm/11.png)

### Autoboxing & Unboxing

- **Autoboxing** is the automatic conversion that the Java complier makes between the primitive types and their corresponding object wrapper classes.
- **Unboxing** is the reverse operation of autoboxing.

---

## Dynamic Programming

**核心思想**

1. 把一个大问题 (size == n) 的解决方案用比他小的问题来解决，也就是思考从问题 size = n - 1 增加到 size = n 的时候，如何用小问题的 solution 构建大问题的 solution。
2. 与 recursion 的关系：
   - Recursion 从大到小来解决问题，不记录任何 sub-solution 只要考虑
     - base case
     - recursion rule
   - DP 从小到大来解决问题，记录 sub-solution
     - base case
     - 由 size (< n) 的 sub-solution(s) -> size (n)的 solution

**When to use DP vs. DFS?**

1. You are required to print out ALL possible permutations -> DFS
2. You are required to **ONLY** return the final 1 number (min / max cut numbers)

### Linear scan and look back (80% questions)

**<font color=#3273DC>Example.1 Largest sum of subarray</font>**
Given an array, find the **subarray** that has the greatest sum. Return the sum.

1. base case: M[0] = input[0]
2. induction rule:
   - M[i] represents [from 0-th index to i-th index] the largest sum of subarray, **must include the i-th index**
   - M[i] = M[i - 1] + input[i] (if M[i - 1] >= 0)
     input[i] (else)

```java
public int largestSum() {

}
```
