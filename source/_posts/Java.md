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

## List, Queue, Deque

![](/assets/java/3.png)

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

## Set & Map

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
- `boolean containsKey(Object key)` -> O(n)
- `boolean containsValue(Object value)`
- `void clear()`
- `int size()`
- `boolean isEmpty()`

**Time Complexity**

| Operation                                                                 | Average | Worst <font color=red>(<= JDK 7)</font> |
| ------------------------------------------------------------------------- | ------- | --------------------------------------- |
| search: <br/> `boolean containsKey(Object key)` <br/> `V get(Object key)` | O(1)    | O(n)                                    |
| insert / update: <br/> `V put(K key, V value)`                            | O(1)    | O(n)                                    |
| delete: <br/> `V remove(Object key)` <br/> `V get(Object key)`            | O(1)    | O(n)                                    |

#### A HashMap Implementation

- A table of **buckets** (an array of buckets), using the array index to denote each bucket
- For each <key, value>, it goes to one of the buckets, the bucket index is determined by a **hash function** applied on **key** and the size of array

![](/assets/java/4.png)

**Collision Control**

- Collision - two keys mapped to the same bucket
- **Separate Chaining** (Close Addressing) - the element of each of the buckets is actually a single linked list
- **Open addressing** - put the key-value pair into the "next" available bucket
  - How to define next? linear / quadratic / exponential probing, hash again
  - Challenge: handing removed keys in the map
  - Not used by Java, but by some real life systems
- If different keys are determined to use the same bucket, they will be chained in the list

#### `==`, `equals()` & `hashCode()`

- `==`
  - determine if two primitive types have the same value
  - determine of two references are pointed to the same object
- `equals()`, `hashCode()`
  - defined in `Object` calss, `Object` is the root class from any Java class
  - any Java class **implicity extends** Object class, so if in the subclass these two methods are not overridden, it inherits what it defines in Object class
  - the defalut implementation of `equals()` is check if the two references are pointed to the same object `==`
  - the defalut implementation of `hashCode()` returns a "unique hash value" for the object based on its memory address

---

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

---

## Nested Class, Iterators, Generics & Enum

### Nested Class (嵌套类)

**Definition**
A nested class is any class whose declaration occurs within the body of another class or interface. A top level class is a class that is not a nested class.

```java
class Demo {
   class InnerClass {
      // ...
   }
   static class NestedStaticClass {
      // ...
   }
}
```

- It is a way of logically classes that are only used in one place
- It increases encapsulation
- It can lead to more readable and maintainable code

> Prefer nested static class to inner class (use inner class only when you need to access non-static member)

### Iterators

Iterate through a Collection object (List, etc.)

- `next()` returns the next element in the iteration
- `hasNext()` returns true if the iteration has more elements
- `remove()` removes from the underlying collection the last element returned by this iterator (optional operation)

### Generics (泛型)

**Definition**
Types are parameters
A type or method to operate on objects of various types while providing compile-time type safety

Java **Generic methods** and **Generic classes** enable programmers to specify

- with a single method declaration, **a set of related methods**
- with a single class declaration, **a set of related types**

#### Generic Method

> Object only

**Syntax**

- `public static <E, ...> void demoMethod(E[] input, ...)`

- `public static <E extends Integer & Comparable<E> & Iterable<E>> void demoMethod(E[] input)`

#### Generic Class

```java
class Demo<T> {
   private T value;
   public Demo(T v) {
      value = v;
   }
   // ...
}
```

#### 通配符 (Wildcard)

- `? extends` (Upper Bounded Wildcard)
- `? super` (Lower Bounded Wildcard)

### Enum

- `values()` can be used to return all values present inside Enum
- Order is important in enums. By using `ordinal()`, each enum constant index can be found, just like array index.
- `valueOf()` returns the enum constant of the specified string value, if exists.

---

## Basic Java File Read / Write

### Stream

A stream can be defined as **a sequence of data**.

- inputStream: The InputStream is used to read data from a source
- outputStream: The OutputStream is used from writing data to a destination

---

## C++ vs. Java (vs. Python), Grabage collection

### Different between Java & C++

1. **Platform compatible, write once, compile once, run everywhere on <font color=red>JVM</font>**

   - C++: write once, **compile** everywhere
   - JVM: Java Virtual Machine
   - JRE vs. JDK
     - JRE (Java Runtime Environement) is the JVM program, Java application need to run on JRE
     - JDK contains the tools for development Java programs rnuning on JRE, for example, it provides the conpile "javac"

   **Compiler -- 编译器**

   **Interpreter -- 解释器**

2. Compiles to Java **byte code** can be recongnized by JVM, independent to underline OS.
   - **JVM (虚拟机)** -- The Java Virtual Machine (JVM) is an **abstract computing machine** The JVM is a program that looks a machine to the programs written to execute in it.
   - **JRE (java 运行时环境)**
   - **JDK (java 开发工具包)**

![](/assets/java/5.png)

3. Interpreter to routines on systems with different machine codes.

   - Compile / Interpret Language
     **Platform compatibility:** language virtual machine <- Java, Python, JavaScript ...
     **Better performance:** compile to native language (machine language) <- C, C++, GO ...
     **C++:** The source code need to be compiled to directly the machine instruction on which machine the program is rnuning on. The compiled byte code is different on different machines (machine need to recongnize the compiled code).
     **Java:** The source code just need to be compiled once and it can run on any machine with JRE installed.
     **Python:** 1. Interpreter: Python 2. Compiler: pyc

4. Strongly encouraged **Object-Oriented Programming Paradigm**, everything in Java is class / object.

5. All types (reference types, primitive types) are always **passed by value**

6. Java does not support unsigned numbers

7. **Pointers vs. References**

   - no pointer arithmetics
     Difference between pointers in C++ and reference in Java:
     - references in Java are **strongly typed**
     - There is no pointer arithmetic on references

8. No operator overloading

9. Classes / Objects are always allocated on the Heap, there is no way to allocate objects on Stack.

10. **Grabage Collection**

11. **Single inheritance, multiple inheritance only be done by implementing multiple interfaces.**

### Grabage Collection

![](/assets/java/6.png)
The **_heap_** is where your object data is stored. <font color=red>This area is then managed by the grabage collertor selected at startup.</font>

**Step 1: Marking**
The first step in the process is called marking. This is where the garbage collector identifies which pieces of memory are in use and which are not. Referenced objects are shown in blue. Unreferenced objects are shown in gold. All objects are scanned in the marking phase to make this determination. This can be a very time consuming process if all objects in a system must be scanned.

**Step 2: Normal Deletion**
Normal deletion removes unreferenced objects leaving referenced objects and pointers to free space. The memory allocator holds references to blocks of free space where new object can be allocated.

**Step 2a: Deletion with Compacting**
To further improve performance, in addition to deleting unreferenced objects, you can also compact the remaining referenced objects. By moving referenced object together, this makes new memory allocation much easier and faster. (fragment)

**There are four kinds of GC roots in Java**

1. **Local variable** are kept alive by the stack of a thread.
2. **Active Java threads**
3. **Static Variables**
4. **JNI (Java Native Interface) References**

---

## Concurrency

### Concurrency vs. Parallel

- **Concurrency:** multiple tasks run _simultaneously_

- **Parallel:** multiple tasks _physically_ run simultaneously

### Multiprocess vs. Multi-thread

**Process:** An independent execution of instructions with independent memory space, stack, heap, and 0S respurce .
Each process sees a complete memory space (pretend to be the only task of a system. )
Different processes communicates through interprocess communication (explicit IPC).

**Thread:** An independent execution of instruction with shared memory space.
Each thread has its private: stack, program counter and register states.
Thread in the same process has shared: heap, static memory segment, os resource.

Semantically, the fundamental difference between a "process" and a "thread" is **if they have independent memory space.** lmplementation-wise, there are corner cases. Be careful about your wording here.

### Java Thread

```java
Thread t = new Thread();

// Schedule the created thread and make ready to go
t.start();

// Make sure the thread finished after this line. (Waits for thsi thread to die.)
t.join();
```

> When the JVM will exit? -> no alive non-daemon threads

A thread's life cycle:
`start()` -> mark it as "read to run" stat -> put into the ready to run queue and wait for the scheduler to assign its time frame (quota) for running -> end of quota or stat change by different event:
`sleep()` -> remove from the ready to run queue, and move to sleeping thread pool
`wait()` -> removed from the ready to run queue, and moce to the conditions' queue for singal to put back to the ready queue
`yield()` -> did not change the ready to run stat, but preempt itself the current quota, put back to the ready-to-run queue again for the next quota

### Synchronization & Race

- **Synchronization:** the coordination of events to operate a system in unison

If two“conflicting operations" are in different threads and are not properly synchronized (concurrent), they will introduce data races. In general, two operations conflict with each other if they operate on the same memory location, and at least one of them is a write. Races are mostly treated as bugs in Java programs.

So to form a data race there should be 3 factors.

1. More than one operations work on the same memory location
2. At least one operation is a write
3. At least two of those operations are concurrent

### Mutual exclusion (mutex), critical section, and locks

Conditions to form a deadlock:

1. Mutual Exclusion: At least one resource must be held in a non-shareable mode. Only one process can use the resource at any given instant of time.
2. Hold and Wait or Resource Holding: A process is currently holding at least one resource and requesting additional resources which are being held by other processes.←Avoid nesting critical sections!
3. NoPreemptin: a resoure can be rleased ony vluntarly by the poces hlding it
4. Circular Wait
