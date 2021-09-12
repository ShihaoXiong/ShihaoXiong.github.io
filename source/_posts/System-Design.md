---
title: System Design
date: 2021-09-11 21:00:46
tags: [Java, Design]
toc: true
categories: Design
---

## Introduction

**System Design** is the process of defining the architecture, modules, interfaces, and data for a system to satisfy specified **requirements**.

- requirements
  - functional
  - non-functional
- architecture
- modules
- interfaces
- data

<!-- more -->

### System Design Interview

Design a product, or a feature of a product (mostly web applications)

- availability
- scalability vs. extendsibility
- performance
- reliability CAP (consistency, availability, partition tolerance)
- robustness
- cost
- ...

### How can I do well in a system design Interview?

- Understand the goal
  - **Scenarious** and **Use-cases**
  - **Ask** the right questions
  - **Clarify** the problem you need to solve
  - Functional vs. Non-Functional
- Understand the **scope**
  - How many DAU(daily active users)
  - What is the QPS level?
- Start with the **Big-picture**, then **Drill-down**
  - Done is better than perfect
- Provide a **functional** solution
  - Simplify the priblem and provide a straw-man design
  - Web application and data storage
- Reiterate and **optimize**
  - Availability
  - Scalability
  - Bottlenecks
  - Single point failures
  - ...
- Trade-offs
- Make it easy for the Interviewr to put down your positive points
- Keep system scaling, performance, and availability in mind

---

## Web Application
