---
title: Algorithms Collections
date: 2021-09-01 21:32:51
tags: [Algorithm, Java]
toc: true
categories: Collections
cover: /assets/algorithms-collections/cover.png
---

This is a Collections for Algorithms in Java.

<!-- more -->

## Recursion

### <font color=#3273DC>Maximum Path Sum Binary Tree I (leaf node to leaf node)</font>

<span class="tag is-medium">Medium</span>

Given a binary tree in which each node contains an integer number. Find the maximum possible sum from one leaf node to another leaf node. If there is no such path available, return Integer.MIN_VALUE(Java)/INT_MIN (C++).

**Examples**

<img src="/assets/algorithms-collections/recursion-02.png" style="height: 250px"/>

The maximum path sum is 6 + 11 + 14 = 31.

```java
class Solution {
  public int maxPathSum(TreeNode root) {
    int[] res = new int[]{Integer.MIN_VALUE};

    return res[0];
  }

  private int maxPathSum(TreeNode root, int[] max) {
    if (root == null) {
      return 0;
    }

    // root is leaf node, we just need to return the value of itself
    if (root.left == null && root.left == null) {
      return root.key;
    }

    int left = maxPathSum(root.left, max);
    int right = maxPathSum(root.right, max);

    // when root.left != null && root.right != null
    // it can be a node which on the path, then we can set the value of max[0]
    // at the meantime, we also return the maximum path to the node's parent node
    if (root.left != null && root.right != null) {
      max[0] = Math.max(max[0], left + right + root.key);
      return Math.max(left, right) + root.key;
    }

    // root.left == null || root.right == null
    // In this case, the node is of leaf node, and it cannot be the root node in the path
    // So we have to pass the value to it's parent
    return root.left == null ? right + root.key : left + root.key;
  }
}
```

### <font color=#3273DC>Maximum Path Sum Binary Tree II (node to node)</font>

<span class="tag is-hard">Hard</span>

Given a binary tree in which each node contains an integer number. Find the maximum possible sum from any node to any node (the start node and the end node can be the same).

**Assumptions**

- The root of the given binary tree is not null

**Examples**

<img src="/assets/algorithms-collections/recursion-03.png" style="height: 250px"/>

one example of paths could be -14 -> 11 -> -1 -> 2
another example could be the node 11 itself
The maximum path sum in the above binary tree is 6 + 11 + (-1) + 2 = 18

```java
class Solution {
  public int maxPathSum(TreeNode root) {
    int[] res = new int[]{Integer.MIN_VALUE};
    maxPathSum(root, res);
    return res[0];
  }

  private int maxPathSum(TreeNode root, int[] max) {
    if (root == null) {
      return 0;
    }

    int left = maxPathSum(root.left, max);
    int right = maxPathSum(root.right, max);

    // if left or right < 0, we'd like abandon it by setting the value to 0
    left = left < 0 ? 0 : left;
    right = right < 0 ? 0 : right;

    max[0] = Math.max(max[0], left + right + root.key);

    return root.key + Math.max(left, right);
  }
}
```

### <font color=#3273DC>N Queens</font>

Get all valid ways of putting N Queens on an N \* N chessboard so that no two Queens threaten each other.

**Assumptions**

- N > 0

**Return**

- A list of ways of putting the N Queens
- Each way is represented by a list of the Queen's y index for x indices of 0 to (N - 1)

**Examples**
N = 4, there are two ways of putting 4 queens:

[1, 3, 0, 2] --> the Queen on the first row is at y index 1, the Queen on the second row is at y index 3, the Queen on the third row is at y index 0 and the Queen on the fourth row is at y index 2.

[2, 0, 3, 1] --> the Queen on the first row is at y index 2, the Queen on the second row is at y index 0, the Queen on the third row is at y index 3 and the Queen on the fourth row is at y index 1.

![](/assets/algorithms-collections/recursion-01.png)

```java
class Solution {
  public List<List<Integer>> nQueens(int n) {
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    // cur means the current way
    List<Integer> cur = new ArrayList<Integer>();

    hepler(n, cur, res);
    return res;
  }

  private void helper(int n, List<Integer> cur, List<List<Integer>> res) {
    if (cur.size() == n) {
      res.add(new ArrayList<Integer>(cur));
      return;
    }

    for (int i = 0; i < n; i++) {
      // to check is this column(i) is vaild to put a queen
      if (vaild(cur, i)) {
        cur.add(i);
        helper(n, cur, res);
        cur.remove(cur.size() - 1);
      }
    }
  }

  private boolean vaild(List<Integer> cur, int col) {
    int row = cur.size();
    for (int i = 0; i < row; i++) {
      // cur.get(i) == n to avoid two queens are on the same column
      // Math.abs(cur.get(i) - col) == row - i to avoid two queens are on the same slash
      if (cur.get(i) == n || Math.abs(cur.get(i) - col) == row - i) {
        return false;
      }
    }

    return true;
  }
}
```

---

## DFS

### <font color=#3273DC>All Subsets I</font>

<span class="tag is-medium">Medium</span>

Given a set of characters represented by a String, return a list containing all subsets of the characters.

**Assumptions**

- There are no duplicate characters in the original set.

**Examples**

- Set = "abc", all the subsets are [“”, “a”, “ab”, “abc”, “ac”, “b”, “bc”, “c”]
- Set = "", all the subsets are [""]
- Set = null, all the subsets are []

```java
class Solution {
  public List<String> subSets(String set) {
    List<String> res = new ArrayList<String>();
    if (set == null) {
      return res;
    }

    char[] arr = set.toCharArray();
    subSets(arr, res, 0, new StringBuilder());

    return res;
  }

  private void subSets(char[] arr, List<String> res, int index, StringBuilder sb) {
    // base case
    if (index == arr.length) {
      res.add(sb.toString());
      return;
    }

    // case 1: add arr[index] to the string
    sb.append(arr[index]);
    subSets(arr, res, index + 1, sb);
    sb.deleteCharAt(sb.length() - 1);

    // case 2: not add arr[index] to the string
    subSets(arr, res, index + 1, sb);
  }
}
```

### <font color=#3273DC>All Valid Permutations Of Parentheses I</font>

<span class="tag is-medium">Medium</span>

Given N pairs of parentheses “()”, return a list with all the valid permutations.

**Assumptions**

- N > 0

**Examples**

- N = 1, all valid permutations are ["()"]
- N = 3, all valid permutations are ["((()))", "(()())", "(())()", "()(())", "()()()"]

```java
class Solution {
  public List<String> validParentheses(int n) {
    if (n <= 0) {
      return null;
    }

    List<String> res = new ArrayList<String>();
    validParentheses(n, res, new StringBuilder(), 0, 0);

    return res;
  }

  private void validParentheses(int n, List<String> res, StringBuilder sb, int left, int right) {
    // base case
    if (left == n && right == n) {
      res.add(sb.toString());
      return;
    }

    // case 1: add '(' in this level
    if (left < n) {
      sb.append('(');
      validParentheses(n, res, sb, left + 1, right);
      sb.deleteCharAt(sb.length() - 1);
    }

    // case 2: add ')' in this level
    if (right < left) {
      sb.append(')');
      validParentheses(n, res, sb, left, right + 1);
      sb.deleteCharAt(sb.length() - 1);
    }
  }
}
```

---

## Dynamical Programming

### <font color=3273DC>Max Product Of Cutting Rope</font>

<span class="tag is-medium">Medium</span>

Given a rope with positive integer-length n, how to cut the rope into m integer-length parts with length p[0], p[1], ...,p[m-1], in order to get the maximal product of p[0]_p[1]_ ... \*p[m-1]? m is determined by you and must be greater than 0 (at least one cut must be made). Return the max product you can have.

**Assumptions**

- n >= 2

**Examples**

- n = 12, the max product is 3 \* 3 \* 3 \* 3 = 81(cut the rope into 4 pieces with length of each is 3).

```java
class Solution {
  public int maxProduct(int length) {
    if (length < 2) {
      reutrn length >= 0 ? 0 : -1;
    }

    int[] M = new int[length + 1];
    M[0] = 0;
    M[1] = 0;

    // i means the length of the rope
    for (int i = 2; i <= length; i++) {
      // j means the demarcation of left and right
      for (int j = 0; j < i; j++) {
        // Math.max(M[i],  Math.max(j, M[j]) * (i - j))
        // the M[i] means no cut
        M[i] = Math.max(M[i], Math.max(j, M[j]) * (i - j));
      }
    }

    return M[length];
  }
}
```

### <font color=#3273DC>Largest SubArray Sum</font>

Given an unsorted integer array, find the **subarray** that has the greatest sum. Return the sum.
**Assumptions**

- The given array is not null and has length of at least 1.

**Examples**

- {2, -1, 4, -2, 1}, the largest subarray sum is 2 + (-1) + 4 = 5
- {-2, -1, -3}, the largest subarray sum is -1

```java
class Solution {
  public int largestSum(int[] arr) {
    if (arr == null || arr.length == 0) {
      return -1;
    }

    int[] M = new int[arr.length];
    M[0] = arr[0];
    int sum = M[0];
    for (int i = 1; i < arr.length; i++) {
      M[i] = Math.max(M[i - 1] + arr[i], arr[i]);
      sum = Math.max(sum, M[i]);
    }

    return M[arr.length - 1];
  }
}
```

- TC: O(n)
- SC: O(n)

### <font color=#3273DC>Dictionary Word I</font>

<span class="tag is-medium">Medium</span>

Given a word and a dictionary, determine if it can be composed by concatenating words from the given dictionary.

**Assumptions**

- The given word is not null and is not empty
- The given dictionary is not null and is not empty and all the words in the dictionary are not null or empty

**Examples**
Dictionary: {“bob”, “cat”, “rob”}

- Word: “robob” return false
- Word: “robcatbob” return true since it can be composed by "rob", "cat", "bob"

```java
class Solution {
  public boolean canBreak(String input, String[] dict) {
    if (dict == null || dict.length == 0) {
      return false;
    }

    Set<String> set = toSet(dict);
    // M[0] represent empty string ""
    boolean[] M = new boolean[input.length() + 1];
    M[0] = true;

    // [0, i] means the substring of input
    for (int i = 0; i <= input.length(); i++) {
      // [0, j] means the left
      // (j, i] means the right
      for (int j = 0; j < i; j++) {
        if (M[j] && set.contains(input.substring(j, i))) {
          M[i] = true;
          break;
        }
      }
    }

    return M[input.length];
  }

  private Set<String> toSet(String[] dict) {
    Set<String> set = new HashSet<String>();

    for (String str: dict) {
      set.add(str);
    }

    return set;
  }
}
```
