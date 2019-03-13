---
title: 'SOLID Principles - Notes'
date: 2018-12-27
permalink: /posts/solidprinciplesnotes
tags:
  - Object Oriented Programming
  - SOLID Principles
---


## Notes on SOLID Principles

The document on SOLID principles by Robert Martin (Uncle Bob) is [here](https://fi.ort.edu.uy/innovaportal/file/2032/1/design_principles.pdf).

We are looking at software modules and their interconnections. Packages, Components and classes are the topics of discussion.

### Symptoms of Rotting Design

There are four main symptoms - Rigidity, fragility, immobility and viscosity.

**Rigidity** - A simple change takes a long time to implement and requires changes in miltiple modules in a cascading way.

**Fragility** - A change in one module affects multiple modules which are not connected conceptually.

**Immobility** - Inability to use parts of the software. Maybe from a different software or from the same software. The functionality is tied with dependencies. The cost of rewriting the required part is less than separating the module from its dependencies.

**Viscosity** - When design preserving methods are harder to implement than hack fixes, the viscosity is high.

## SOLID Principles

### The Open Closed principle (OCP)

**A module should be open for extension but closed for modification.**

The techniques to achieve OCP are based on abstraction. Code needs to be changed every time a new type of input needs to be handled. This is the usual case of applying OCP. 

This is solved using Classes and Subtypign polymorphism. All the types of input handled can be modelled as instance of a class. Then the behavior of the input type is handled in the class. So the specifics of the type of the input is not propogated everywhere and closed for the outside world. However this can also be solved using static/parametric polymorphism or Generics.

### The Liskov Substitution Principle

The instance of base class should be substitutable by an instance of the derived class.

