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

# Local Branches

Lets talk about local branches first.

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

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 06 Animation: Many branches.

three branches: small-fix branch, big-feature with a lot of commits, medium sized some-issue branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++



The order in which you integrate your various topic branches is super flexible as well. You can work for a few hours on one branch but maybe take weeks for a bigger feature on another.

Even branching off from other branches is no biggie. When you think about it, we do it all the time from master.

![Alt text](/images/branching-off-branch.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 07 Animation: Branching off from one topic-branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Remote Branches

So far, we have only discussed branches that are local to your computer. We can expand our branches into the cloud via remote branches.

Remote branches point to the state of branches that are part of your remote repo. Someting like GitHub for example. 

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 08 Animation: Code editor 

git push

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You can see these remote branches as bookmarks and backups. Services like GitHub are much more sophisticated and powerful but at their core they just function as Git servers. 

That way you can communicate and share your commits through a centralized hub that you share with your team.

The remote repo has a default name called origin and is followed by its branch name. You can name it anything you like of course.

So your local branch might have the name new-feature and the remote version would be origin/new-feature.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 10 Animation: Code editor 

remote / branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You can even check them out and see if the updates on a particular remote branch are to your liking.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 11 Animation: Check out remote branch.

Branch has octocat. HEAD moves to latest commit on that branch.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

How do you create a such remote branches? When you fork or clone a repo, you will have automatically access to the branches that are on that remote repo. Git creates these references for you.

All the data such remote repo hold are pulled down. Git creates a local master for you that has all the info from the remote repo. That way you can immediately build upon that.

![Alt text](/images/local-and-remote-branches.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 12 Animation: Both remote and local branch after git clone. Remote branch has cat HEAD instead of Max Headroom.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Git also gives you access to all the remote branches of that remote repo.

Through the remote branches you gain access to the work of others—and vice versa of course. The pointer of a remote branch doesn’t move unless you synchronize with the remote repo.

![Alt text](/images/local-and-remote-branch-diverge.png)

![Alt text](/images/local-and-remote-branch-diverged-fetch.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 13 Animation: Remote branch falls behind local branch with new commits

??? Fetch in changes that you don’t have
??? Push master, and update remote branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You update these branches by pushing to them. They don’t automatically update when you commit locally. That way you share your work with your team as well.

You can only push if you have write access to that remote repo of course. Once branches are up on a Git server, they are ready for collaboration.


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 14 Animation: Code editor

git push origin branch-name

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You specify the name of the git server, here origin and the branch-name you wanna push. That pushes the local branch-name to the remote repos branch-name.

From now on, you can push to that remote branch  simply by using git push when you checked out that local version of it.  

After that, when somebody fetches from this shared repo, they will have a reference to that new branch of yours. They can fetch it down and check it out as well.

So the branches that you wanna share get pushed up to some remote repo. The branches that stay on your machine are not only private, other team members have no access to them. They don’t even know they exist.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 15 Animation: Collaborators can get access to new remote branch???

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++



++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 16 Animation: Push to remote branch ??

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You can merge changes from remote braches after fetching their data. All you need is specify the remote branch that you want to merge into your currently checked out branch.

Even cooler, you can create a new local branch that is based on the remote version that you fetched. This new local branch will have the latest commits from the remote branch and is ready to synchronize your new work on the local branch.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 17 Animation: Code editor

merge origin/some-branch-name

git checkout -b some-branch-name origin/some-branch-name

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++



server communication

# Tracking Branches
