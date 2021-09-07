---
title: Java
date: 2021-07-09 16:01:50
tags: Java
toc: true
categories: Notes
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
- **Heap**: Java objects reside in an area called te heap. The heap is created when the JVM starts up and may increase or decrease in size while the application runs. **(FIFO)**

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

---

## APIs

### List

- `add(E e)`
- `add(int index, E e)`
- `remove(int index)`
- `remove(E e)`
- `set(int index, E e)`
- `E get(int index)`
- `int size()`

### Queue

- `offer(E e)`
- `E peek()`
- `E poll()`

### Deque

- `offerFirst(E e)`
- `offerLast(E e)`
- `E peekFirst()`
- `E peekLast()`
- `E pollFirst()`
- `E pollLast()`

### Summary

1. **if you want to use a Queue**
   `Queue<X> queue = new ArrayDeque<>();`
2. **if you want to use a Stack**
   `Deque<X> stack = new ArrayDeque<>();`
3. **if you want to use a two-end Queue**
   `Deque<X> dq = new ArrayDeque<>();`
4. **if you want to use a size-adjustable array**
   `List<X> list = new ArrayList<>();`
5. **if you want to use a priority queue**
   `PriorityQueue<X> pq = new PriorityQueue<>();`

---

## HashMap

### Common API

- `V put(K key, V value)`
- `V get(Object key)`
- `V remove(Object key)`
- `boolean containsKey(Object key)`
- `boolean containsValue(Object value)`
- `void clear()`
- `int size()`
- `boolean isEmpty()`

## Inheritance, Polymorphism, Access Modifier, Exceptions

### Inheritance

A class that is derived from another class is called a **sbuclass** (also a **derived** class, **extended** class, or **child** class). The class from which the sbuclass is derived is called s **superclass** (also a **base** class or a **parent** class).

```java
class Person {
   public final String name;
   private int age;
   public String getName() {
      return name;
   }
   public int getAge() {
      return age;
   }
   public void setAge(int age) {
      this.age = age;
   }
   public Person(String name) {
      this.name = name;
   }
}

class Employee extends Person {
   public String company;
   public void setCompany(String company) {
      this.company = company;
   }
   public String getCompany() {
      return company;
   }
   public Employee(String name) {
      super(name);
   }
}
```

### Polymorphism

使用父类的 reference 来指向子类的实现，并且调用**子类里面 override 了父类的 method**

1. 哪些函数能够调用取决于 reference（声明）的类型 --compile time
2. 对于被 override 的函数，调用哪个版本，取决于实现的类型 -- runtime

#### Override

**Override** is when you redefine a method that has been defined in a parent class (using the same signature). Resolved at runtime.

#### Overload

**Overload** is when you define two methods with the same name, in the same class, **distinguished by their signatures (different)**. Resolved at compile time.

#### abstract vs. interface

A class must be declared abstract when it has one or more abstract methods. A method is declared abstract when it has a method heading, but no boay - which means that an abstract method has no implementation code inside like normal methods do.

**Difference:**

1. An abstract class **has one or more abstract methods**, can have some **non-abstract methods, constructors** and **instance variables** as well, can have **non-public members**. An interface **has only abstract methods**. All methods must be **public**. Any class that implements the interface is responsible for providing the method definition / implementation.
2. Java does not allow multiple inheritance. In Java, a class can only derive from one class, whether it's abstract or not. However, a class can implement multiple interfaces. Java does not allow multiple **extends**, but a class can **implements** multiple interfaces.

**When use abstract class vs. interface?**

1. An abstract class is good if you think you will plan on using inheritance since it provides **a common base class** implementation to derived classes.
2. An abstract class is also good if you want to be able to declare non-public members. In an interface, all methods must be public.
3. If you think you will need to **add non-abstract methods in the future**, then an abstract class is a better choice. Because if you add new method headings to an interface, then all of the classes that already implement that interface will hace to be changed to implement the new methods.
4. Interface are a good choice when you think that the **API will not change for a while**. Interface are also good when you want to hace something similar to multiple inheritance, since you can implement multiple interfaces.

### Access Modifier

- `public`: everyone can access
- `private`: only myself can access (but only at class level, other objects of the same calss can access as well)
- `protected`: only my children and same package can access
- `default`: only the same package can access

| Modifier    | Class | Package | Subclass | World |
| ----------- | :---: | :-----: | :------: | :---: |
| public      |   Y   |    Y    |    Y     |   Y   |
| protected   |   Y   |    Y    |    Y     |   N   |
| no modifier |   Y   |    Y    |    N     |   N   |
| private     |   Y   |    N    |    N     |   N   |

### Exceptions

![](/assets/java/1.png)

```java
class ExceptionTest {
   public void getException() {
      try {
         exception();
      } catch (Exception err) {
         System.out.println(err);
      }
   }

   public void throwException() throws Exception {
      exception();
   }


   public void exception throws Exception {
      throw new Exception();
   }
}
```

#### throw vs. throws

- `throw`: The **throw** keyword in Java is used to explicitly throw an exception from a methods or any block of code.
- `throws`: Throws is a keyword in Java which is used **in the head of method** to indicate that this method might throw one of the listed type exceptions. The caller to these methods **has to** handle the exception if it they are **checked exceptions**.

![](/assets/java/2.png)

---

## How to test code?

- smoke test
- unit test
- functional test
- integration test
- regression test
- performance / load / stress test
- end-to-end test
- balckbox / whitebox test

### JUnit Test

#### Annotations

- `@Test`: The Test annotation tells JUnit that the public void method to which it is attached can be run as a test case.
- `@Before`: Several tests need similar objects created before they can run. Annotating a public void method with `@Before` causes that method to be run **before each** Test method.
- `@After`: If you allocate external resources in a Before method, you need to release them after the test runs. Annotating a public void method with `@After` causes that method to be run after the Test method.
- `@BeforeClass`: Annotating a public **static** void method with `@BeforeClass` causes it to be run once before any of the test methods in the class.
- `@AfterClass`: This will perform the method after all tests hace finished. This can be used to perform clean-up activites.

**Note**

1. `@BeforeClass` runs once, at the very beginning
2. `AfterClass` runs once, at the very end
3. `@Before` runs before **each** test
4. `@After` runs after **each** test
5. Methods with `@BeforeClass` and `@AfterClass` must be **static**

#### Assertions

- `void assertEquals(int expected, int actual)`
  Checks that two primitives / objects are **equal**
  `void assertEquals(double expected, double actual, double delta)`
- `void assertTrue(boolean condition)`
  Checks that a condition is true
- `void assertFalse(boolean condition)`
  Checks that a condition is false
- `void assertNotNull(Object object)`
  Checks that an object isn't null
- `void assertNull(Object object)`
  Checks that an object is null
- `void assertSame(Object a, Object b)`
  The `assertSame()` method tests if two object **references** point ti the same object
- `void assertArrayEquals(expectedArray, resultArray)`
  The `assertArrayEquals()` method will test whether two arrays are equal to each other

### What is a good test case?

1. Should be accurate and tests what it is intended to test.
2. No unnecessary steps should be included in it.
3. It should be resuable.
4. It should be traceable to requirements.
5. It should be independent.
6. It should be simple and clear, any tester should be able to understand it by reading once.
