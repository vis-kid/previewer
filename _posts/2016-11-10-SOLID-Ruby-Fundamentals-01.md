---
layout: post
title: SOLID Ruby Fundamentals 01-SRP
date: 2016-11-10 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Mechanic, Nokogiri, Ruby]
---

# Topics

+ SOLID?
+ SRP: Single Responsibility Principle

# SOLID?

SOLID is an acronym for five design principles in object oriented design in programming.

+ SRP, or the “Single Responsibility Principle”.
+ Open/Closed Principle.
+ Liskov Substitution Principle.
+ Interface Segregation Principle.
+ Dependency Inversion Principle.

A lot of principles, I hear ya! Don’t let you discourage by their academic sex appeal. You are new to this and are maybe still dealing with a lot of basic programming concepts. The last thing you wanna stuff your brain with are coding principles. Fair point! I would say though that you are maybe missing out on an opportunity to step up your game from the very beginning.

minimal entanglements

It is not uncommon to think that you need to wait until you are ready, until you are more advanced to jump into more advanced waters. And you may be right in some regards. These design principles are different though. On the one hand, I feel like they are beginner-friendly to grasp and on the other hand, they will improve the quality of your code from day one. They will help you to design your code the way professionals do—at least you will start looking at your code as the big girls do. What’s not to like?

If I would attempt to sum up the SOLID principles in one sentence, I would probably fail—miserably. What question do these principles address? This might be easier to answer. They were a result of thinking about how we can better manage dependencies. How can we prevent to create fast-rotting software? These are questions that are of equal importance for beginners and pros alike.

Are these principles appropriate for a dynamic language like Ruby? After all, when SOLID was formed, it was in an era of statically typed Java and C++ programming. Good question! Yes, static languages have stronger dependencies but there is no reason to throw these principles out the window when you write Ruby code. You will learn basic techniques how you can modularize and stabilize your code.

Stabilize? Yes, in the sense that you will be netter equipped to write code that is less brittle and better prepared for the inevitable change down the line. The ease with which you are able to change a code base is in direct correlation to its quality. I recommend that you start early in your developer career to think about this by starting to give these OO design principles an early look.

# SRP: Single Responsibility Principle

> A class should have only one reason  to change

(https://www.flickr.com/photos/redjar/113974357)

![Alt text](/images/SOLID/swiss-army-knife.jpg)

In short, it is the opposite of spaghetti code I’d say. You decrease your chances of creating unwanted entanglements and hanging your app with code that has too many intermingled responsibilities.

If you would follow only once principle in your coding journey, SRP would be a good candidate. It brings a lot of benefits to the table, at literally little to no cost while encouraging other development in your code that is good. As a byproduct almost.

That means your classes have only one responsibility. If this class is focused on only one job, there can be another reason to change it.

## Clarity

Optimized for readablity

Big classes are very often a smell that they are not following SRP.

You want to avoid classes that have too much going on. If they start to feel blobby and you need to take some time to figure out what’s actually going on, it’s probably a good time to refactor its responsibilities. 

## Reuseability

Ideally, you want your classes to handle one responsibility well so that you can reuse that functionality in other places as well.

A good guideline is to build small components because they increase the chance of being useful to be recycled some place else in your code.

Keeping things isolated, in the own little scope

## Easier to Test

When you don’t have classes that are too involved and frankly all over the place, you can write much more focused and readable tests as well. Your tests are probably a good indicator of your classes quality of state. They run faster as well because of reduced dependencies.

But not only are such tests slower if classes don’t follow SRP, they will also be more brittle along they way. Your classes and your tests will change, this is the one certainty you have. The less responsibilities you encapsulate in classes, the less brittle your code will end up being.

Look out for tests that are hard to set up and involve a lot of dependencies. That’s a good indicator that you should spend some time on simplifying the class design.

## Easy to Change

Classes with tons of responsibilities are a pain to change. Since this is expected future behaviour of your application, you should plan for it. SRP classes are more flexible to change. They are easy to refactor or to completely rewrite if your application goes through growing pains for example.

Flexibility and resiliency are two important qualities you want to have from every piece of your code. The Single Responsibility Principle hands you these benefits on a silver platter without much effort. Rewriting you work becomes less and less of a liability—or even of a necessity when reuse and modularity are built in from the start.

## Code Quality

For senior software writers, the ease of being able to change code when the requirements necessitate that, is one of their highest metric to assess code quality.

## Downloading Context

If the classes are smaller, you will give other team members and your future self an easier time to get familiarized with its behaviour as well. Nobody wan’t to open a class and sit there for 15 minutes before they have a general sense of what’s going on. That will make people—you guessed it—put funny stuff in your coffee at some point.

Also, what developers need to load into their heads in this context, is often what you will end up needing to load as dependencies in your tests. Not exactly ideal in many ways..
## Reasons to Change

Reasons to change roughly equal responsibilities. If you have a class with five different jobs, you have a good chance of ending up with five reasons to change. You class sets a session, sends email that also get validated and does the weather forecast for the next week?

If any of these things change, the rest of the class is possibly affected by that. This is exactly what SRP wants us to avoid. This is the brittleness we talk about.

## Cohesive Classes

Classes should be highly cohesive. You are setting up the dynamite to blow up the house you are living in yourself. O.K., let’s not overdramatize these things, I was just trying to find a strong metaphor that sticks with you. Nobody is gonna loose his life or anything, but some people might loose their minds if these things pile up for years and then something happens. Entangling that large piece of spaghetti code could literally break a product if people loose the ability to rebuild the application with the components at hand.

Every piece of a class should be highly related to other pieces of a class. In our imaginary example from above, the weather forecast is not highly related to email validation, sending email or creating a session. So we end up with weak cohesion which basically let’s your app rot from the inside. You are setting up the dynamite to blow up the house you are living in yourself.

O.K., let’s not overdramatize these things, I was just trying to find a strong metaphor that sticks with you. Nobody is gonna loose his life or anything, but some people might loose their minds if these things pile up for years and then something happens. Entangling that large piece of spaghetti code could literally break a product if people loose the ability to rebuild the application with the components at hand.

So we want to have a class that has everything to do with forecasting the weather, one that is in charge of sending email, one that creates a session and one that handles the responsibility of calculating the weather forecast. Maybe even two for that. One that handles the weather forecast for a week and another who is in charge of the next 24 hours.

In Rails, stuff that people often put in ApplicatioController or ApplicationHelper are often not very cohesive by nature. It’s a top-level object that often collects a bunch of inconvenient stuff. You will often see applications where it acts as a dumpster of stuff that the lazy fix didn’t compose a better home for. This can quickly get out of hand and pile on stuff from all over the place. Just because it’s easy to use these compartments for “global” stuff, doesn’t meant that we should stop there. Also, even as beginners, we already know that global stuff is rarely in our best self-interest.

## Boundries

It’s best to evolve your code to become SRP friendly. Often you don’t know from the beginning how the responsibilities are spread and you should not over-optimize from the start without knowing where the journey is headed.

Some classes will end up being magnets for responsibilities while others are like a lot more lazy in piling them on.

## Gem churn

It will show you how often particular classes have been changed. These statistics can help you to identify which classes might be in need of an SRP treatment. Your god classes and the ones that cover the central business proposition will often end up being high on that list.

When these classes change often it tells you that they have many reasons to change.

This gives you an easy peek into your core models and updates you that the rate of change might be alarming or not.

## Bugs

Intermingling and churn is an open door for bugs. It’s almost like inviting cock roaches into your home by laying out food at your doorstep.

## God Classes and User

Always think hard before you add something to the User model. It is such an attractive magnet for all sorts of things in most applications.

## One ... bullet

You will often have to weigh different principles against each other. SRP is no golden bullet that fits every occasion. It is often a very good starting point though. Figuring out a balanced middle ground will always be part of your job when you design applications. That also expands to applying code quality principles.

## How do we recognize good design?



Dependencies tell you how change is gonna propagate throughout your code.

Many dependencies create a sort of pressure to not change things. Worse if yo don’t know what the extend is that a little change here or there  can cause.

## And / Or

If you need “and” or “or” in the description, the purpose of the functionality in a particular class, you can start thinking about applying SRP. Both scenarios imply that it does more than one focused thing. 

As mentioned above, nothing is set in stone, there is no one truth fits all scenario, but at least it should raise the question if it might be a good idea to refactor your code to follow only one responsibility.

## But it is Hard

I give you that. In the beginning it will be relatively easy to break models into focused classes that do one thing. That will be your foundation to move forward into more complex waters. Take the time to build this base with smarts. Once complexity kicks in, it becomes a lot harder to break objects apart. I mean, there is no way around it. It will happen anyway, but with a good foundation, not only buildings get to be built taller and more stable. It will get harder over time, that is probably for sure. But at the same time, the problems also get more interesting, more challenging. It is a fun process that you might actually enjoy.

## Benefits

Breaking classes into focused components have a few benefits that can easily go unnoticed among the upside mentioned above. A few benefits that I can see are the following: 

+ Method names can often stay simpler due to fewer dependencies referenced.
+ Better organization because you grouped functionality more coherently.
+ Namespace clashes are easier to avoid.
+ Classes are easier to read and digest.
+ Same might apply for variable names.
+ Simpler, more readable APIs.
+ Smaller objects.
+ Faster objects.

## Changes

The question about inevitable change is, how would you like the ride to be? Crazy as hell or fun and interesting. Do you wanna feel like the world is doomed or just another day at the office with an interesting problem. The design of your application will strongly influence the experience of change. If you have an unflexible, rigid system in place, you can count on creating a cascade of related changes that pop up during that process. Fragile systems are not your friend.

We don’t want application where everything is connected to everything else. That is a nightmare for anybody to work with. It’s basically a fancier way of saying it’s “Spaghetti code”. That does not sound like a four star 20 course menu, does it? You don’t want to build incredible long domino lines that all fall by tripping over a single piece.

https://commons.wikimedia.org/wiki/File:Toppledominos.jpg

![Alt text](/images/SOLID/Toppledominos.jpg)



https://www.flickr.com/photos/baccharus/5817342671

![Alt text](/images/SOLID/cable-mess.jpg)

image. hammer arcade game

## Payoff

Good design will pay off. As with having a test suite around. It might cost you a little bit of extra effort in the beginning, but you will be glad going forward to have built on solid ground. Once you start to feel the need to fight your app all the time, you will wish back the good times where the application was young and perfect where you had an easy opportunity to apply solid design.

Good design takes time in the beginning, as does TDD. But you can count on being better prepared for changing conditions down the line. Actually, it will most likely cost you extra money to not pay attention to design in your application. Cleaning up spaghetti code takes time and costs big bucks.

## SRP Symtoms

When your code is

+ loosely coupled
+ highly cohesive
+ easily composeable
+ context independent


It keeps your classes and methods small. That directly leads to better maintainability

Coupling is dependency

Managing dependencies is a huge chunk of your actual work as a software writer.

## What should I do?

As a consequence of applying SRP, you will very often extract classes. If you have a bigger class with many responsibilities, you will end up needing more classes that have smaller, more focused jobs.

# Extract Class




You don’t go fast by writing bad code. That’s for sure. Chances are pretty good that the opposite is adequate.

Rigidity in your code means that you need to modify a bunch of seemingly unrelated stuff when you touch one thing. You can only create a new state of consistency once the other dpendencies are taken care of. There are various degrees of course, but the treashold, the line to cross is not that big.

You cannot make an isolated change without changing everything around it. Bad dependencies. 

Fragility. Breaking things in many places that seem unrelated to the one place that actually changed. like butterfly effect.

Don’t put functions that change for different reasons into the same class. That means you can organize you classes around functions that change for the same reason.
