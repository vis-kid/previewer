---
layout: post
title: Git Rebase Mixtape
date: 2016-11-02 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Git]
---

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 01 Animation: Cassette tape that has  Git Rebase handwritten on it. Nobs are spinning.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

My last video, merge vs rebase, gave you a solid basis to integrate changes from one branch into another. But unsurprisingly, Git is a deep tool and there is more to learn about rebase.

We need to cover a bit more ground to get you ready for some real action in the big leagues. I thought I’d make you a quick mixtape with a few highlights. 

<!--
???
You will need to understand when you can and when you definitely cannot do a rebase. I also want to show you how to cherry pick a few selected commits instead of whole branches.
???
-->

Understanding interactive rebasing, cherry picking and doing a git pull --rebase rounds off what you should understand to step up your game.

But let’s start with the dangers of rebase first and work up our appetite for some cherry picking at the end.

# Rebasing a pushed remote


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 02 Animation Danger Skull

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

If you are not careful with rebasing, you can quickly enter a world of pain.

If you rebase commits that have been outside your own repository, you have a high likelyhood of making people hate you—or wanting to put funny stuff in your coffee.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 03 Animation Poo in a cup of coffee

Maybe with image of that weird laughing dog

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Lets say you have a local branch that is already pushed to a remote that other team members had access to. This branch is off hands for any rebase action.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 04 Animation Rebasing from a third branch that has an cloud / GitHub icon
One branch with repo icon

One branch with github / cloud icon

Both are the same.

Local branch gets rebased

Bit red X??

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

If you then rewrite history, you will potentially mess with the history of other people’s forks if they already have build on top of your work. Never, ever do that. You might enter a world of pain, you and potentially everybody else.

If you need to git push --force something with a rebase, it should be warning sign that you need to stop and make sure you know what you are doing!

Just remember to rebase only local changes and you and everybody else’s work will be safe.

# Update PR

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 05 Animation Cloud with fork

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Say you work on a fork for an open source project and you can’t push your fork because the remote branch you are branched off from has moved forward. In that case you will be often asked to do a rebase.
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 06 Animation Show branches with master branch that moved forward

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

(fetch in changes. git rebase upstream/master)

What you  need to do is fetch in these changes and rebase your work on top of the newest changes that come from the upstream branch of the open source repo. 

Your branch moves forward as a whole, as if it was split off from that latest commit on master. After that you force push it to your remote from which you start or continue your pull request.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 07 Animiation. Branch moves forward, highlighting the new point from which the branch emerges. 

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# git pull - -rebase

#### pull rebase
![Alt text](/images/git-pull-rebase.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 08 Merge bubble and rebase side by side. No animation necessary


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++



#### pull merge
![Alt text](/images/git-pull-merge.png)


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 09 Laptop with code editor.

git pull = git fetch + git merge.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

A normal git pull consists of two operations: git fetch which is automatically followed by a git merge.

There is another option though. Some say a more elegant way since it does not create merge bubbles like here:

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 10 Branch with merge bubble

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

What we can do isntead is a git pull - - rebase.

This also fetches the newest changes and rebases the current branch on top of the upstream branch.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 11 Laptop with code editor.

git pull - -rebase = git fetch + git rebase.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


git pull rebase undos your latest commits, pulls down the newest changes and replays them on top of these pulled in commits.

The end result looks like your newest work happened after the work you pulled in. In reality, your work could be much older. This will result in a cleaner timeline of commits and avoids to create merge bubbles??

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 12 Show clean line of commits without merge bubble.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Your local branch and the remote version of it are actually two different branches. when you do a classic git pull, you merge the commits from the remote branch into your local one.

with git pull rebase, we can pretend they are one and the same branch and commit our local changes on top of the remote branch without a messy merge. As if they are one and the same.

<!--
!?!
lets say you want to push your master up to a shared repo but somebody else made changes in the meantime that you don’t have. That push will be rejected. When you do a git pull --rebase origin master, then your last commits will be rewinded, while the new commits from your team mates will be pulled in. After that, your newest commits that you wanted to push will be reapplied on top of that pulled in change set. Just like it was always there. No merge commit necessary. Then you do a git push origin master and your changes are up in the cloud.
!?!
-->

<!--
???
that means that it will run a fetch and then a rebase instead of a merge.
???
-->

<!--
???
this fetches new commits from a tracked remote branch and rebases them onto your branch
???
-->

<!--
???
In case you or somebody else did rebase a non local branch that other are affected by, there is one magic bullet you can give a shot.
???
-->

<!--
??
you can even change the setting so that when you type git pull, you are actually doing a git pull --rebase behind the scenes.
???
-->

<!--
???
Especially when you work on the same branch as other members of your team, this can be very helpful in maintaining a cleaner history with fewer merge bubbles.
???
-->


# Interactive Rebase


We have another, more fine grained way to rewrite history with interacting rebasing.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 13 Animation Laptop w/ code editor: git rebase -i HEAD~5

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

-i flag initiates what we call an interactive rebasing session. The underlying logic of a rebase stays the same with an interactive one. You just have more options to fine tune the process.

We can change multiple commit messages at once, we can reorder commits, easily delete them and we can squash multiple commits together.

The commits will pop up in your editor of choice and are ready to be changed. Git lists the oldest commit at the top.

Why? Because this one will be replayed first. It goes through the list, from oldest to newest and replays them as you committed them.


![Alt text](/images/interactive-rebase.png)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 14 Animation Squashing commits with a hammer

Squash from three to one and move forward. Old brach with unsquashed commits is still visible at 50% opacity. Arrow pointing forward to squashed commit.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

When is squashing a good idea? If you have a messy history that you want to clean up. Squashing let’s unnecessary, insignificant commits disappear. It gives you an opportunity to polish your work before you hand it in.

<!--
???
animation:
laptop with code editor that has a list of sqash, pick, etc.
????
-->


<!--
????
animation:
laptio with code editor: moving commits around,changing their order
????
-->


<!--
???
You can for example reorder them by simply switching their lines around.
???
-->


<!--
???
You can alter individual commits and their messages and clean up the history like a git wizard. You just have to define a range for how many commits you want to do an interactive rebase.
???
-->

<!--
???
As a reminder, you should of course not do an interactive rebase to commits that have already been pushed to a remote that other developers have access to.
???
-->

<!--
???
Interactive rebase comes in handy when you want to modify commits that are beyond your recent history. They are easily accessible that way.
???
-->


# Cherry picking

Animation: cherry icon on top of selected commits

We saved the cherry for last. Cherry picking is a form of rebase that lets you fine tune individual commits you need.

Say you have a branch that only has one or a couple of useful commits before you discard the rest of the branch. An ideal scenario for cherry picking.


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 15 Animation Laptop w/ Text Editor

git cherry-pick commit-hash

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

### 16 Animation

Cherry picking one commit and reapplying on master
Cherry picking two commits and reapplying on master


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

![Alt text](/images/cherry-pick.png)
![Alt text](/images/cherry-pick02.png)

You can target a single commit by its hash number of define a range of commits that you wanna pick. Each picked commit get recreated on the for the rebase. You can observe that when git is changing their hash number.

As you can see, cherry picking is easy as pie.
