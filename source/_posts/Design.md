---
title: Design
date: 2021-08-15 09:01:06
tags: [Algorithm, Java]
toc: true
categories: Notes
---

## Object Oriented Design (OOD)

<!-- more -->

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

---

## 设计实现 BlackJack (21 点)

**Description**
2 - 10: face
J Q K: 10 points
Ace (A): 1 or 11 points
total score > 21: busted
special: BlackJack > Ace + any of 10 score cards

52 cards (no joker)

<br/>
棋牌类游戏(OOD)

1. 对游戏道具和游戏状态的描述
2. 游戏规则和游戏流程

**Process**

1. Design a general card

```java
enum Suit {
   Club,
   Diamond,
   Heart,
   Spade
}

enum FaceValue {
   // Ace, Two, Thress, ...
}
```

```java
class Card {
   private final FaceValue faceValue;
   private final Suit suit;

   public Card(FaceValue faceValue, Suit suit) {
      this.faceValue = faceValue;
      this.suit = suit;
   }

   public FaceValue value() {
      return faceValue;
   }

   public Suit suit() {
      return suit;
   }
}
```

```java
class Deck {

}
```

```java
class Hand {
   protected final List<Card> cards = new ArrayList<>();
}
```
