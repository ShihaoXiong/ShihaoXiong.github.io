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
      // j means the demarcation of lefr and right
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
