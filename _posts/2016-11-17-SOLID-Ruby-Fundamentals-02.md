---
layout: post
title: SOLID Ruby Fundamentals 02-Open/Closed-Principle
date: 2016-11-17 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Mechanic, Nokogiri, Ruby]
---

# Topics

+ OCP
+ Open/Closed?
+ Dependency Injection
+ Composition over inheritance

??
+ Composite?
+ Decorator?

You should be able to extend a class’s behavior without modifying it. Open for extension, closed for extension.

subclassing. dont affect anybody else that way. you dont need to overwrite functionality

prefer subclassing over reopening classes

A class, module or function should be open for extension but closed for modification.

Extend them via child classes and inheritance

ocp helps with attaning SRP

Ideally, when you put a class into production, you don’t want to change it. You want to avoid chances of introducing bugs. The same goes for other clients who rely on a class. Changing them endangers possibly that relationship.

changing behaviour of a class clients rely on would impact all of the clients. The ripple effects can be that they themselves have clients and so on. Not good!

OCP was thought of in the context of inheritance. But we learned that composition over inheritance is the way to go.

# Composition over inheritance

Extension can redefine parts of the behaviour of a class.

Are you allowed to bug fix? Of course, that is always ok. In that case you have to change the class.

It reduces churn??

similar to immutable datasstructures??

Code that never changes

change the behavior is ok, extend the behavior is ok. We just want to be able to do that without changing existing code.

The downside to ocp is indirection

Idealistic scenario that you probably can’t achieve completely.

The classes stay the same. we can achieve this with abstraction.

# Dependency Injection
The use of dependency injection is pretty typical to achieve OCP.
