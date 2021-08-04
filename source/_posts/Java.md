---
title: Java
date: 2021-07-09 16:01:50
tags: Java
toc: true
categories: Note
---

## Primitive Types & Basic Operations

**byte**: 计算机最小存储单元

<!-- more -->

```
2's complement: 按位取反再加1

0000 = 0
0001 = 1
0010 = 2
...
0111 = 7     -> overflow
1111 = -1
1110 = -2
...
1001 = -7
1000 = -8    -> underflow
```

**Conclusions (n bits):**

- total number of represented integers: **2<sup>n</sup>**
- number of positives: **2<sup>(n-1)</sup> - 1** , number of negatives: **2<sup>(n-1)</sup>**
- range: [-8, 7]

| Primitive Type | Sotres                | Range                                                       |
| -------------- | --------------------- | ----------------------------------------------------------- |
| byte           | 8-bit integer         | -128 to 127                                                 |
| short          | 16-bit integer        | -32,768 to 32,767                                           |
| int            | 32-bit integer        | -2,147,483,648 to 2,147,483,647                             |
| long           | 64-bit integer        | -2<sup>63</sup> to 2<sup>63</sup> - 1                       |
| float          | 32-bit floating-point | 6 significant digits (10<sup>-46</sup>, 10<sup>38</sup>)    |
| double         | 64-bit floating-point | 15 significant digits (10<sup>-324</sup>, 10<sup>308</sup>) |
| char           | Unicode character     |                                                             |
| boolean        | Boolean variable      | false and true                                              |

### 取整

**遵循向零取整**

### Operators & Precedence

| Operators            | Precedence                                         |
| -------------------- | -------------------------------------------------- |
| postfix              | expr++, expr--                                     |
| unary                | ++expr, --expr, +expr, -expr, ~, !                 |
| multiplicative       | \*, /, %                                           |
| additive             | +, -                                               |
| shift                | <<, >>, >>>                                        |
| relational           | <, >, <=, >=, instanceof                           |
| equality             | ==, !=                                             |
| bitwise AND          | &                                                  |
| bitwise exclusive OR | ^                                                  |
| bitwise inclusive OR | \|                                                 |
| logical AND          | &&                                                 |
| logical OR           | \|\|                                               |
| ternary              | ? :                                                |
| assignment           | =, +=, -=, _=, /=, %=, &=, _=, \|=, <<=, >>+, >>>= |

**widening (精度)** \
byte -> short -> int -> long -> float -> double

高精度 -> 低精度需要进行显示转换

```java
double x = 1.9
int y = (int)x
```

---

## Control Flow & Methods

### Control Flow

- `if...else if...else`\
   _In Java, the expression in "()" must bt a bollean expression_
- `for`
- `while`\
  `break` & `continute`
  - break: jumps out of the entire loop
  - comtiunte: skip the current iteration

### Methods / Functions

```java
[modifier] [return type] [name of the method](arguments) {
   // ...
}
```

#### Signature

- **Name of the method**
- **The list of the parameter <u>types</u>**

**_Q: What uniquely identifies a function?_**\
_A: function name + the list of parameter <u>types</u>_

#### Method Overloading(重载)

函数名相同，但是 parameter type list 不同的 functions

1. different types
2. different num of parameters
3. different order

#### Return Type

It can be

- a primitive type: int, double, char, boolean
- a class: TreeNode, ListNode, ...
- void: no return value

---

## Array, Class & Objects

### 1D/2D Array

#### 1D Array

- Array is a form of **data collection**. It can be a collection of **anything**.
- Array elements occupy consecutive memory space.

```java
int[] empty = new int[5];
int[] numbers = new int[]{4, 20, 7, 5, 10};
int num = numbers[2];
numbers[4] = 15;

// traverse an array
for(int i = 0; i < numbers.legnth; i++) {...}
for(int num: numbers) {...}
```

#### 2D Array

```java
int[][] matrix = new int[][]{{2, 1}, {3, 4}, {8, 9}}
```

### Main Function

Main function is the entry point the Java program; all other functions are directly or indirectly invoked by the main function.

```java
public static void main(String[] args) {
   // ...
}
```

### The Object-Oriented Paradigm in Java

- **存储**：和 primitive types 的对比
- **构造函数**
- **更多关键词**：static, final, public/private, this
- null basics -> NullPointerException (NPE)

### Basic concept

- **class**
- **object**
- **reference**: the business card of object, find the object by ref.

### Working with Objects

The `new` keyword: create an object

```java
Student firstStudent = new Student("Tom");
```

There are actually three important operations that happened in this statement.

1. **Declaration:** associate a variable name with an object type

   ```java
   Student firstStudent
   ```

2. **Instantiation:** the `new` keyword is a Java operator that creates the object
   ```java
   firstStudent = new Student();
   ```
3. **Initialization:** the new operator is followed by a call to a `constructor`, which initializes the new object
   ```java
   Student firstStudent = new Student("Tom");
   ```

### Object Memory Layout

Memery spaces in a Kava program: `Stack` and `Heap`

- **Stack**: In computer science, a call stack is a stack data structure that stores information about the active subroutines of a computer program (stack frame). This kind of stack is also known as an execution stack, program stack, control stack, run-time stack, or machine stack, and is often shortened to just "the stack". **(LIFO)**
- **Heap**: Java objects reside in an area called te heap. The heap is created when the JVM starts up adn may increase or decrease in size while the application runs. **(FIFO)**

### Call Stack

> _no function call, no call stack_

### Stack + Heap

1. Stack + Heap are memory (RAM) segments
2. RAM: Random Access Memory
3. Difference
   - space: stack ~MB, heap ~GB
   - read/write speed: srack (1 CPU instruction) >>> heap("bookkeeping")
4. Stack frame
   - 1 function invocation -> 1stack frame "pushed" onto <u>the call stack (a pile of books)</u>
   - <u>if a function is never called, its stack frame is never created</u>

### Variable Scope

1. Scope: **lifetime and accessibility/visibility** of a variable. How large the scope depends on where a variable is <u>declared</u>.
2. Simply, the scope is the **innermost** {} warpping up the **decalration**.
3. <u>Local (local to function) variables</u>: the variables whose lifetime is strictly tied with a function.

### Default Values

- It's not always necessary to assign a value when a field is declared. Fields that are declared but not initailized will be set to a reasonable defalut by the compiler.
- The following chart summarizes the default values for the above data types.

  | Data Type  | Default Value (for fields) |
  | ---------- | -------------------------- |
  | byte       | 0                          |
  | short      | 0                          |
  | int        | 0                          |
  | long       | 0L                         |
  | float      | 0.0f                       |
  | double     | 0.0d                       |
  | char       | '\u0000'                   |
  | any object | null                       |

### Java Parameter Passing

**Java function call is always pass by value (copy)**

- primitive type: copy of the <u>value itself</u>
- reference: copy of the <u>reference</u>

### Variables and their scope

**Local variables must be initialized before use.**
_One local variable has the **lifetime** of their own scope._

In general, each pair of **curly brackets** defines a scope. Each variable is associated with the scope it defined in.

#### Instance Variable (filed)

- instance filed == instance variable == member variable
- **non-static** variables defined **within a class**, but not within any methods
- instance variables have the same life cycle as the instance
- instance variables' visibility is defined by their access modifiers
- instance variables hasve default values f they're not initaialized by a constructor

|               | Local Variable       | Insrance Variable             |
| ------------- | -------------------- | ----------------------------- |
| Location      | Stack                | Heap                          |
| Lifetime      | Method call          | Instance lifetime             |
| Visibility    | Scope                | Modifier                      |
| Default value | N / A                | 0, false, null                |
| Initialzation | Must set before read | In declaration or constructor |

### Objects, References & Null

### Static

Members (filed, methods) belong to class, not object (instance).

```java
class Demo {
   public static isTrue = false;
   public static viod change() {
      // cannot use `this`
      isTrue = true;
   }
}

Demo.change();
System.out.println(Demo.isTrue); // true
```

- cannot use `this` to access isTrue in here
- `this` usually been used as a **Object**
- variable with static means variables belong to class not object
  <br/>
- **Static methods can (directly) access only static variables / methods**
- **Non-static method can access both static and non-static variables / methods**

### Final

constans, once assigned, cannot be changed.

- **final class:** A class that cannot be derived
- **final method:** A method that cannot be overridden
- **final variable:** Avariable that once assigned, cannot be assigned again

### Array & Object

#### arrays are objects

- the elements in the array still occupy consecutive memory space on the heap
- **"length" is a filed of the array object**
- **array length can _not_ be changed after the array object is created. (final)**

**Occupies consecutive memory and the elements in the array can be referenced via the index in O(1) time**

#### Exceptions / Error

- ArrayIndexOutOfBoundExpecption
- NullPointerException
