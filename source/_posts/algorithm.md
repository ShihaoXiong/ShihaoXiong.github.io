---
title: Data Structure & Algorithm
date: 2021-07-09 16:02:50
tags: [Algorithm, Java]
toc: true
categories: Note
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

---

## Recursion, Queue & Stack

### Recursion

1. 表象上：function calls itself
2. 实质上：Boil down a big problem to smaller ones (size n depends on size n-1, or n-2 or ... n/2)
3. **Implementaion 上**：
   a. **Base case**: smallest problem to solve
   b. **Recursion rule**

#### Example 1: <u>Fibonacci Sequence</u>

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

- TC: (2<sup>n</sup> - 1) _ T(node) = (2<sup>n</sup> - 1) _ O(1) = O(2<sup>n</sup> - 1) = O(2<sup>n</sup>)
- SC: n _ S(node) = n _ O(1) = O(n)

levels: n
total number of nodes: 1 + 2 + 4 + ... + 2<sup>n-1</sup> = 2<sup>n</sup> - 1

1. number of branches = number of recursion calls = number of used subproblems
2. leaf nodes = base cases
3. one node = function call

#### Example 2: <u>Power</u>

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

### Queue

**FIFO** (first in first out) 先进先出

```java
Queue<Integer> queue = new LinkedList<>();
```

**APIs:**

- offer(): insert an element in the tail of queue
- poll(): get an element from the head of queue
- peek(): read the value of head element of queue
- size(): how many elements are in the queue
- isEmpty(): if this queue is empty

### Stack

**LIFO** (last in first out) 后进先出

```java
Deque<Integer> stack = new LinkedList<>();
```

**APIs:**

- offerFirst(): insert an element in the top of stack
- pollFirst(): get an element from the top of stack
- peekFirst(): read the value of top element stack
- size(): how many elements are in the stack
- isEmpty(): if this stack is empty or the size of this stack is 0

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

#### Example 1: Given a linked list, find the index - k element of it.

```java
public class FindKth {
   public static ListNode findKIndex(ListNode head, int k) {
      if (head == null || k < 0) {
         return null;
      }

      ListNode cur = head;
      int index = 0;

      while (index != 0 && cur != null) {
         cur = cur.next;
         index++;
      }
      return cur;
   }
}
```

#### Example 2: How to find the middle node of a linked list ?

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

#### Example 3: Insert a node in a sorted linked list

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

   while (cur.next && cur.next.value < value) {
      cur = cur.next;
   }
   newNode.next = cur.next;
   cur.next = newNode;

   return head;
}
```

#### Example 4: How to merge two sorted LinkedList into one long sorted LinkedList ?

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

#### Example 5: Remove nodes with target value in the LinkedList.

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

#### Example 6: Reverse a LinkedList.

##### Iterative

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

##### Recursion

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
      int[] rightRes = mergeSort(arr, right, mid);

      // recursion rule
      return merge(leftRes, rightRes);
   }

   private int[] merge(int[] leftRes, int[] rightRes) {
      int res = new int[leftRes.length + rightRes.length];
      int i = 0;
      int j = 0;
      int k = 0;

      while (i < leftRes.length && j < rightRes.length) {
         if (leftRes[i] < rightRes[j]) {
            res[k] = leftRes[i++];
         } else {
            res[k] = right[j++];
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

      int i = 0, j = 0,k = arr.length - 1;
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

**LeafNode:** both rigth and left are nul

**Tree Traverse:**

1. pre-order: current node before its subtress (self-left-right)
2. in-order: current node in its subtrees (left-self-right)
3. post-order: current node after its subtrees (left-right-self)

**Base Concept:**

1. **Height of binary tree:** The distance between the root with the deepest leaf node.
2. **Balanced binary tree:** is commonly defined as a bnary tree in which the depth (alse known as height) of the left and right subtrees of **every node** differ by 1 or less.
   1. for each of the nodes in this binary tree
   2. satisfyL the height of lesf subtree, right subtree at most diff by 1.
