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
   while (cur.next && cur.next.value < value) {
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

**LeafNode:** both rigth and left are nul

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
      // 1. find adn delete smallest node in root right
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
      // cur is the smallest one, adn prev is its parent
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

**用途：** 维护一个变化的数据集的最优值

**性质：** 堆的数显通过构造二叉堆 (binary heap)， 这种数据结构具有以下性质

1. 堆总是一颗**完全二叉树 (complete binary tree)**
2. 根节点最小的堆叫做**MIN HEAP**，根节点最大的堆叫做**MAX HEAP**
   - MIN HEAP: 任意节点**小于(等于)** 它的所有后裔 (descendent) (堆序性)
   - MAX HEAP: 任意节点**大于(等于)** 它的所有后裔 (descendent) (堆序性)

**Index of `parent = i`, what is the index of the two child nodes?**

- left child of index `- = 2 * i + 1`
- right child of index `i = 2 * i + 2`
- parent of index `i = (i - 1) / 2`

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
   compare with the **largest** element of the previous smallest k candidates 1. case 1: new element >= top: **ignore** 2. case 2: new element < top: **update (top -> new element)**
   O((n - k)logk)

- TC: O(k + (n - k)logk)

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

1. ```java
   PriorityQueue<Cell> heap = new PriorityQueue<Cell>();
   ```
   - initialize the internal array with default capacity(11)
   - **class `Cell` must implements `Comparable<Cell>`**
2. ```java
   PriorityQueue<Cell> heap = new PriorityQueue<Cell>(16);
   ```
   - initialize the internal array with specified capacity(16)
   - **class `Cell` must implements `Comparable<Cell>`**
3. ```java
   PriorityQueue<Cell> heap = new PriorityQueue<Cell>(16, new MyComparator());
   ```
   - initialize the internal array with specified capacity(16)
   - **class `MyComparator` must implements `Comparator<Cell>`**
4. ```java
   PriorityQueue<Cell> heap = new PriorityQueue<Cell>(new MyComparator());
   ```
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

**<font color=#3273DC>Example. Smalest k elements in unsorted array.</font>**
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

#### Implementaion of Min Heap

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
         throw new
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
   finSubset(input, index +１, solutionPrefix);
   solutionPrefix.deleteCharAt(solutionPrefix.length() - 1);

   // case 2: do not add input[index] to the solution prefix
   findSubset(inout, index + 1, solutionPrefix);
}
```

- TC: O(2<sup>n</sup>)
- SC: O(n)

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
   if (l > n) {
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
   4 levels, each level considers on type of coin
2. **How many different states should we try to put on this level?**
   dynamically changes

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
   3 levels, each level represents on position
2. **How many different states should we try to put on this level?**
   remaining unused letter

```java
void permutation(char[] inout, int index) {
   if (index == inout.length) {
      System.out.println(innput);
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

**<font color=red>Conclusion:</font>** whenever every single permutation contains all elements in the initial input, the we should consider **SWAP** and **SWAP**
