---
layout: post
title: Distributed Git
date: 2016-11-22 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Git]
---

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 01 Animation: Intro image with a benevolent dictator. It has face of donald trump 

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Intro

Git is pretty awesome when it comes to sharing code. I think its fair to say that next to simple and inexpensive branching, the convenient way of distributing code with Git made it the powerhouse it is today.

Git allows us to have multiple repos, unlike centralized systems. Every repo is a node of equal power—their permissions can differ though. Read and write access can be distributed differently.

There is more than one distributed workflow. In fact, the possible variations are quite extensive. The flexibility to adapt workflows to the preferences of teams or to the size of projects is quite impressive. Daunting for beginners maybe, but also very enabling once you face production environments or bigger open source projects for example.


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 02 Animation: Repo being node and hub at the same time.

Repo creating multiple repos below and contributing new code to a repo above

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Git’s power comes from being decentralized, making every copy of the code equally important. That means that every contributor is potentially a node and a hub at the same time. Being able to easily contribute code offline is another cool aspect we take for granted. 

# Blessed Repo

Usually we deal with canonical repos, which are like the official versions the other repos lead into. Develpers clone or fork from these master repos, work on new commits and push these back into it.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 03 Animation: Blessed Repo

green cloud with blessed label

???
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

I think its best to see these changes as code suggestions to the canonical repo. Through a pull request we can discuss if such suggestions are appropriate or if they need to be improved or even declined.

This main repo often has one or multiple maintainers that check the quality of the code that people contribute.

We should discuss two major variations that are important for understanding most modern workflows we encounter these days. The integration-manager workflow and the benevolent dictator model. Both are variations of a multi-repo workflow.

# Integration-Manager Workflow

![Alt text](/images/integration-manager-workflow.png)

The blessed repo in this scenario has one or multiple integration managers who have the final say what goes into the blessed repo. It is public in the sense that contributors can clone repos from that version. They have read access. For pushing new code into that main repo, they have to to through an integration manager though.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 04 Animation: Blessed Repo has an integration manager.

yellow cloud with integration manager label

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

They check the code from other developers that they suggested via their publicised branches. If they find the new code acceptable, they check out the new  branch, test it and merges it into their own copy first. That way the blessed repo only gets new code after it has been checked and tested. 





++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 07 Animation: developers fork repo from blessed copy that is public

Laptops accepts cloned version from blessed copy

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 07 Animation: developers work on their own private copies

Laptop pushing to private repo label

arrow coming out of laptop repo

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 08 Animation: developers push from private  repo label to public one.

orange cloud accepts commit into its repo symbol
Laptop pushing to public repo label

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++



++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 05 Animation: integration manager accepts new code into his repo.

cloud accepts commit into its repo symbol

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

As a final step, the integration manager pushes the code they merged into their repo into the blessed copy.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 06 Animation: integration manager pushes newly accepted code into blessed repo

cloud accepts commit into its repo symbol

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This workflow will become very  familiar to you when you start working with a tool like GitHub. This service lets you fork copies from a main repo and lets project maintainers integrate these changes through these published copies of your code.

Everybody can work asynchronously and nobody needs to wait to continue their work. Pretty great!



# Benevolent Dictator

![Alt text](/images/benevolent-dictator-workflow.png)


