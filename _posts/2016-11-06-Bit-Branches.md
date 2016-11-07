---
layout: post
title: Git Branches
date: 2016-11-06 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Git]
---

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 01 Animation: Tokyo Subway map

Small HEADs moving along like trains??

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Development always has a main line for development, the master branch. Branching in Git is not only very easy, it is also cheap. You can create an endless number of branches without any serious side effects—except that housekeeping is a good idea.

With a branch, we can savely diverge from the main line of development and work on new features that we want to propose to be integrated into the master branch. If acceptable, it is just a merge or a rebase away from being pushed into production.

If on the other hand, the work on the branch does not fullfill the requirements needed, you can simply continue your work on that branch or abandom it altogether.

Branching is a killer feature of Git and as a beginner it is important to understand its power and potential.

Since branching is so cheap and fast, you 

What is a branch? It is a moveable pointer to a commit. A bit less cryptic, a branch is a plain old file that contains the 40 character hash of the commit it points to. That’s why people say that branches are cheap and easy to create. All it takes is 41 bytes. Very efficient, isn’t it?

Being able to branch that easy and therefore that often makes our coding lives a lot more efficient and stable.

Master is nothing else than a branch itself. It’s just a convention to call that particular branch master. When you run `git init`, this master branch gets created for you as a default.


# Basic Branching

The whole Git workflow is around creating and fusing branches together.

Switching between branches is not only super fast, it is easy as well.


You create a new branch simply by typing git branch with a branch-name. This creates a new pointer that can be occupied by HEAD.

You only created the branch though and didn’t switch to it. That is accomplished via the checkout command. 

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 02 Animation: Laptop with code editor

git branch branch-name
git checkout branch-name

git checkout -b branch-name

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The shortcut to create a branch and check to it at the same time is git checkout -b branch-name. One step instead of two.

 We have covered HEAD in another short video but I should say this. In this context, Git uses HEAD to know which branch we are on. So when you switch to a new branch, HEAD is coming along.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 03 Animation: Checkout branch: HEAD moving between tips of two branches

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

When you continue your work on that new branch, HEAD will move forward to its latest commit.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 04 Animation: New commit on feature branch and HEAD moves forward.
Switch branch back to master as well.
back and forth??

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Although you moved forward on the new branch, you can easily switch back. The cool thing to understand as a beginner is that your file system are changing all the time when you check out various branches.

The files adapt to their respective state represented by the latest commit HEAD is pointing at at any given moment. That way you can recreate all kinds of different points in time of development. So far, so good.

The different versions of your work are separated by the branches you create. Every branch is a silo of its own until you fuse them into each other—by a merge or rebase of course.

What is cool about branching is that you can do a lot of different work in parallel. Say you are working on some database fix on one branch and some extraction for your front-end code in another.

Both of these, or more, can be in place similataneously wihtout affecting each other in any way. You just switch between branches whenever you decide to work on something specifically—and however long it takes I might add.

# Branch Management

The git branch command shows you a list of all branches in your repo.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 05 Animation: Laptop w/ code editor

git branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

In a real world scenario, you will most likely not only have to deal with two branches. It is likely that you have at least one branch between a new feature branch and master. Something to test your changes and see if they are stable before you are merged into master.

![Alt text](/images/silo-branches.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 05 Animation: 3 branches. Master, Develop, topic-branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Each of them are contained silos that gives the team of working in parallel without interrupting each other. All of these silos represent different levels of stability—at least ideally. 

You can take this a lot further of course. It simply depends on the size of your team, the complexity of the project and the preferences you agree upon.

I should clear up two terms you will often hear. The distinction between topic branches and long running branches is simple. master and branches that are basically set up to test new features are long running ones.

Topic branches on the other hand are most of the time much shorter lived ones. You work on some new feature, get it ready to be integrated and you get rid of them after that. They represent the bleeding edge so to speak.
