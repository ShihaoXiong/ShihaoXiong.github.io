---
title: Design
date: 2021-08-15 09:01:06
tags: [Algorithm, Java]
toc: true
categories: Notes
---

## Object Oriented Design (OOD)

### Class & Object

**Class:** A blueprint for a data **type**, scheme
**Object:** A specific realization of any class, **instance**

### Data encapsulation: Data Abstraction & Access Labels

1. Providing only essential information to the outside world and hiding their background details
2. Spearate interface and implementation
3. Access labels: `public`, `private`, `protected`, `default`

| Modidier    | Class | Package | Subclass | World |
| ----------- | :---: | :-----: | :------: | :---: |
| public      |   Y   |    Y    |    Y     |   Y   |
| protected   |   Y   |    Y    |    Y     |   Y   |
| no modidier |   Y   |    Y    |    N     |   N   |
| PRIVATE     |   Y   |    N    |    N     |   N   |

**How to select appropriate labels?**

1. API: `public`
2. Internal implementation: `private`
3. Class inheritance: do we need to use `protected` for methods/attributes
   1. Protected methods: sometimes useful when we want to Override an implementation in subclass
   2. Protected attributes: be careful, tyr to use `private` first
4. `default` in java (package-private): declare a class as public only when we define public API in it, otherwise package-private is good enough

### Inheritance, Interface, Abstract Class

### Polymorphism & Overriding

**Polymorphism (多态):** A call to a member function will cause a different function to be executed depending on the type of object that invokes the function. (subtype polymorphism)
**Override:** A subclass or child class to provide a specific implementationof a method that is already provided by one of its superclasses or parent classes.
