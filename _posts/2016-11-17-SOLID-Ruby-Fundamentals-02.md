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

# OCP

The “Open/Closed Principle” simply states that you should be able to extend a class’s behavior without modifying it. Open for extension, closed for modification. At closer distance, this means that a class or module should be cool with extension but avoid modification. To some extend this can be useful for functions as well.

Why, what’s the beef with modification? Ideally, when you put a class into production, you don’t want to change it. We want to avoid taking chances of introducing bugs. The same goes for other clients who rely on our work. Changing existing code frequently endangers possibly such relationships. Changing behaviour of a class that clients rely on could impact all of their clients as well. The ripple effects can go far beyond your code. Not good!

What about bugs? Are we allowed to bug fix without adhering to OCP? Of course, that is always ok. In that case you have to change the class. I guess it’s fair to say that this principle at its core wants to manage change—at least the rate of it. Since this is a constant companion in our work, it is important to keep it in check where we can. Single responsibilities are one thing, not needing to modify them is a different story though. In practice, both SRP and OCP seem to have symbiotic existences. OCP often helps with attaining SRP

# Composition over inheritance

OCP was originally thought of in the context of inheritance. But we learned that composition over inheritance is the way to go.

Extension can redefine parts of the behaviour of a class.

subclassing. dont affect anybody else that way. you dont need to overwrite functionality

prefer subclassing over reopening classes


Extend them via child classes and inheritance






It reduces churn??

similar to immutable datasstructures??

Code that never changes

change the behavior is ok, extend the behavior is ok. We just want to be able to do that without changing existing code.

The downside to ocp is indirection

Idealistic scenario that you probably can’t achieve completely.

The classes stay the same. we can achieve this with abstraction.

# Dependency Injection
The use of dependency injection is pretty typical to achieve OCP.

This technique allows us to extend already existing classes without modifying them. Since this is a frequent strategy to follow OCP, I thought we should give it a quick look.

aids srp

When you inject dependencies you avoid having that object creating these dependencies themselves. It’s like outsourcing that responsibility??? As a result you have more flexibility and less coupling. But for our purposes here, also fewer reasons to change due to less coupling.

Below. it knows the name of another class. It knows a method and its argument on that other class.
``` ruby

class MarkdownComposer

  attr_reader :extracted_data

  def initialize(detail_page_link)
    @extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  ????
  def initialize(extracted_data)
    @extracted_data = extracted_data
  end
  ????


```

The class is not as ignorant as it could be. it knows about pageextractor.

We have a strong link here.

What is obvious above, is that if we change the name of the  PageExtractor class or change the extract_data method, we need to come here as well. 

in the new methods, MarkdownComposer only knows that the injected object responds to the `extract_data` methods???

It allows us to only use the interface of an injected dependency but not coupled to the creation of ...

A dependency can be easily injected within the constructor. We pass an object into the targeted class without letting it know what this object is all about.

Mocking code dependencies in our tests becomes easier with DI as well.

Through DI, we are feeding a class new behavior without changing it.

When we need to refer to another class through its name, we create a stronger relationship, a stronger dependency between them.

What we also reduce is reuseability with these stronger dependencies
