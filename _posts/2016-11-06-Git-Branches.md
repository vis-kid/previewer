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

### 01 Animation: Branches with Tree top

Small HEADs moving along like trains??
Branches that make up trunk with bonsai leaves on top??

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Intro

The whole Git workflow is around creating and fusing branches together. Branching is a killer feature of Git. As a beginner it is important to understand its power and potential.

Development always has a main line for development, the master branch. Branching in Git is not only very easy, it is also very cheap.

You can create an endless number of branches without any serious side effects—except that housekeeping is a good idea.

With a branch, we can savely diverge from the main line of development and work on new features that can be proposed to be integrated into the master branch. If acceptable, it is just a merge or a rebase away from getting into production.

If on the other hand, the work on the branch does not fullfill the requirements needed, you can simply continue your work on that branch or abandon it altogether.

<!--
???
Since branching is so cheap and fast, you 
???
-->

# Branch?

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 02 Animation: A large white file with a long 40 character hash.

A tree with white files with hash??

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

What is a branch? A branch is a plain old file that contains the 40 character hash of the commit it points to.

That’s why people say that branches are cheap and easy to create. All it takes is 41 bytes. 

Being able to branch that easy and therefore that often makes our coding lives a lot more efficient and stable.

Also, a branch is a moveable pointer to a commit. You can switch easily between branches using the git checkout command. Switching between branches is not only super fast, it is easy as well.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 03 Animation: show pointer moving between master and feature branches

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Master for example is nothing else than a branch itself. It’s just a convention to call that particular branch master. When you run `git init`, this master branch gets created for you as a default.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 04 Animation: Focus on master. Jumping HEAD??

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Local Branches

We can have local and remote branches. Lets talk about local branches first.

You create a new branch simply by typing git branch with a branch-name. This creates a new pointer that can be occupied by HEAD.

You only created the branch though and didn’t switch to it. That is accomplished via the checkout command. 

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 05 Animation: Laptop with code editor

git branch branch-name

git checkout branch-name

git checkout -b branch-name

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The shortcut to create a branch and check to it at the same time is git checkout -b branch-name. One step instead of two.

We have covered HEAD in another short video but I should say this. In this context, Git uses HEAD to know which branch we are on. So when you switch to a new branch, HEAD is coming along.


When you switch branches, you also change the index and working directory underneath. It adjusts to the history of a particular branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 06 Animation: Checkout branch: HEAD moving between tips of two branches

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This is important to understand as a beginner. Your working directory and your index are changing all the time when you check out various branches.

The files adapt to their respective state represented by the latest commit HEAD is pointing at at any given moment. That way you can easily switch between contexts and recreate all kinds of different points in time for development. 

Branches also act as discrete silos for your work. They are not interrupting each other.

The different versions of your work are separated by the branches you create. Every branch is a silo of its own until you fuse them into each other—by a merge or rebase of course.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 07 Animation: Background of branches become highlighted with yellow black to imply they are locked from each other.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

So far, so good.

What is also cool about branching is that you can do a lot of different work in parallel. Say you are working on some database work on one branch and some front-end code in another.

Both of these, or even more branches, can exist simultaneously without affecting each other in any way. You just switch between branches whenever you decide to work on something specifically. Branches can be kept around as long as you need them.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 08 Animation: New commit on feature branch and HEAD moves forward.

Switch branch back to master as well.
back and forth??

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

When you continue your work on that new branch, HEAD will move forward to its latest commit.

# Remote Branches

So far, we have only discussed branches that are local to your computer. We can expand our branches into the cloud via remote branches.

Remote branches point to the state of branches that are part of your remote repo. Something like GitHub for example. You can setup your own Git server as well of course. 


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 09 Animation: 3 branches. local, github, own server(server icon with git logo)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 10 Animation: Code editor 

git push

pushing a single commit that moves between local and remote branch with github icon

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You can see these remote branches as bookmarks and backups. Services like GitHub are much more sophisticated and powerful but at their core they function as Git servers. 

That way you can communicate and share your commits through a centralized hub that you share with your team.

Your own remote repo has a default name. It’s called origin. You can name it anything you like of course.

We identify a branch by prefixing it with that server name. Like origin / master for example. 

<!--
???
So your local branch might have the name new-feature and the remote version would be origin/new-feature.
???
-->

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 11 Animation: Code editor 

remote / branch

git fetch origin

git checkout origin / new-feature

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You can even check them out and see if the updates on a particular remote branch are to your liking.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 12 Animation: Check out remote branch.

Branch has octocat. HEAD moves to latest commit on that branch.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

How do you create a such remote branches? When you fork or clone a repo, you will have automatically access to the branches that are on that remote repo. Git creates the necessary references for you.

All the data such remote repo holds are pulled down. Git creates a local master for you that has all the info from the remote repo. That way you can immediately build upon that. You also have access to the remote version under origin / master

![Alt text](/images/local-and-remote-branches.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 13 Animation: Both remote and local branch after git clone. Remote branch has cat HEAD instead of Max Headroom.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

<!--
???
Git also gives you access to all the remote branches of that remote repo.
???
-->

Through the remote branches you gain access to the work of others—and vice versa of course. The pointer of a remote branch doesn’t move unless you synchronize with the remote repo.

![Alt text](/images/local-and-remote-branch-diverge.png)

![Alt text](/images/local-and-remote-branch-diverged-fetch.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 14 Animation: Remote branch falls behind local branch with new commits

<!--
??? Fetch in changes that you don’t have
??? Push master, and update remote branch
-->

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You update these branches by pushing to them. They don’t automatically update when you commit locally. By pushing to your remote, you share your work with your team as well.

You can only push if you have write access to that remote repo of course. Once branches are up on a Git server, they are ready for collaboration.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 15 Animation: Code editor

git push origin branch-name

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

From now on, you can push to that remote branch  simply by using git push when you checked out that local version of it.  

<!--
???
You specify the name of the git server, here origin and the branch-name you wanna push. That pushes the local branch-name to the remote repos branch-name.
???
-->

After that, when somebody fetches from this shared repo, they will have a reference to that new branch of yours. They can fetch it down and check it out as well.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 16 Animation: Push to remote  branch 

Collaborators can get access to new remote branch???

Using the icons from the rails world train. Different developers get branches you pushed ot git hub branch icon.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

To sum it up, the branches that you wanna share get pushed up to some remote repo. The branches that stay on your machine are private. Other team members have no access to them until you push them as well. They don’t even know they exist.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 17 Animation: Merge changes from remote repo into own local master

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You can merge changes from remote braches after fetching their data. All you need is specify the remote branch that you want to merge into your currently checked out branch.

Even cooler, you can create a new local branch that is based on the remote version that you fetched. This new local branch will have the latest commits from the remote branch and is ready to synchronize your new work on the local branch.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 18 Animation: Code editor

git merge origin/some-branch-name

git checkout -b some-branch-name origin/some-branch-name

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 19 Animation: Create new branch from remote one.

Remote branch copies itself and moves over to master and docks onto it.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


<!--

# Tracking branches
???
git branch -u origin/serverfix???
Branch serverfix set up to track remote branch serverfix from origin

That tells the already existing branch you are on to track the remote origin/serverfix branch.
???

That in effect establishes something called a tracking branch. That means that the local branch and the remote branch have a direct relationship. git pull and git push will work without referencing the branch names themselves.

When you clone or fork a repo, master is already set up to be tracking its remote version.
If you want to fetch updates for all branches at once you can run 
git fetch -- all

If you want to see more info about each branch, how many commits you are behind or ahead of each remote branch, you can run
git branch -vv

How can we delete a remote branch. Once you have no need for a remote branch that is on yyour Git server, you should clean them up. Having tons of remote branches around that are dead isn’t very useful. The same goes for local branches of course.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 18 code editor

git push origin --delete serverfix


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

You provide the git push command with the name of the server, the branch name and the --delete option.

server communication
-->






<!--

# Branch Management

The git branch command shows you a list of all branches in your repo.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 20 Animation: Laptop w/ code editor

git branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

In a real world scenario, you will most likely not only have to deal with two branches. It is likely that you have at least one branch between a new feature branch and master. Something to test your changes and see if they are stable before you are merged into master.

![Alt text](/images/silo-branches.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 21 Animation: 3 branches. Master, Develop, topic-branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Each of them are contained silos that gives the team of working in parallel without interrupting each other. All of these silos represent different levels of stability—at least ideally. 

You can take this a lot further of course. It simply depends on the size of your team, the complexity of the project and the preferences you agree upon.

I should clear up two terms you will often hear. The distinction between topic branches and long running branches is simple. master and branches that are basically set up to test new features are long running ones.

Topic branches on the other hand are most of the time much shorter lived ones. You work on some new feature, get it ready to be integrated and you get rid of them after that. They represent the bleeding edge so to speak.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 22 Animation: Many branches.

three branches: small-fix branch, big-feature with a lot of commits, medium sized some-issue branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++



The order in which you integrate your various topic branches is super flexible as well. You can work for a few hours on one branch but maybe take weeks for a bigger feature on another.

Even branching off from other branches is no biggie. When you think about it, we do it all the time from master.

![Alt text](/images/branching-off-branch.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 23 Animation: Branching off from one topic-branch

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-->

