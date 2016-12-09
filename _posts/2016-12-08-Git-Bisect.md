---
layout: post
title: Git Bisect
date: 2016-12-08 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Git]
---

# Topics

+ Bisect
+ Manual Bisect
+ Automatic Bisect

# Bisect

git bisect is handy for identifying a troublesome commit. When we encounter bugs, we sometimes need to find out when it was introduced into the codebase. 

It’s more of an advanced feature for sure, but one that can give you a good amount of street cred if you need to hunt down some bug.

I prepared a file that has some pseudo Bug in it. I commited this file a couple of times and introduced the bug somewhere along the way. Nothing surprising here I hope, just standard stuff really.

## Screen 00
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Various commits until we end up with horrible bug

start method puts 'Everything is fine'

another method

puts 'Everything is super duper'

method with puts 'Horrible Bug'

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Bisecting simply means dividing something into two equal parts. For our purposes, this means it will provide us with a binary search.

For big projects with tons of contributors and commits, this can make a tremendous difference in finding that bug faster.

We wanna figure out at which point in the commit history, a certain file or feature was introduced a bug for the first time. We narrow it down by splitting the revisions in half. But don’t worry too much about the underlying mode of operation, the binary search.

# Manual Bisect

Let’s talk about the manual mode first. In that case, we have to check the code ourselves every step of the way.

Bugs are tracked down by giving git bisect feedback. Git checks out commits and we tell it if they are good or bad. In the end, we are presented with the commit that first introduced the troublesome code that was causing our problems.
  
Behind the scenes, git checks out various commits and gives you the opportunity to look at the code at these various stages in your history.

Doing this by hand without git bisect, using git log combined with a git show, would make us going down the commit history ourself. This is most likely not something we wanna do with our time. 

## Screen 01
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git log

git show hash

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

First we need to find a commit that doesn’t have the bug in it. In the real world, we would start with a certain release or something. So we are looking for a commit that is save to start with. In our case, we start with the initial commit.

## Screen 02
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git log

copy hash 

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


## Screen 02
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect start

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The process already started, even if you don’t see feedback in the shell. Next we feed git bisect with the commit that we know is not having the bug.


## Screen 03
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect good hash

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

To delimit our search we can also provide bisect with a bad target. ??? If we don’t provide a hash for a bad target, git will just assume that you need the most current commit.

## Screen 04
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect bad other-hash

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

When it starts to run, git will checkout commits between these specified delimiters. We can check our code and see if the code is buggy or not.

## Screen 07
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

See code example in markdown file in another pane.

ruby code

some method that has BUG written in  it

method with puts 'Horrible Bug'

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

If the currently checked out commit bisect shows you is problematic, we need to tell Git and move to the next commit. Here, we can see that this code has bug written all over it. 

## Screen 05
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect bad 

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

We move on and Git shows us also how many possible steps are left.

## Screen 06
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Output of git bisect in action

Highlight the (roughly 5 steps)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


## Screen 07
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

See code example in markdown file in another pane.

ruby code

some method that has BUG written in  it

method with puts 'Horrible Bug'

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

As we can see here, the text is ok here. NO bug to be found. Therefore, we can provide git bisect with some feedback about the current bug search. All good.

## Screen 08
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect good

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

## Screen 09
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Output showing (roughly 4 steps)

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

We check our code again and see if we got lucky.

## Screen 10
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Reload pane 
Find BUG text, maybe weird characters, or cursing from comics

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This one seems to be the perpetrator we were looking for. Again, we need to provide git with some feedback.

## Screen 11
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect bad

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

We need to continue to get to the bottom of this.

## Screen 12
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Update pane to check for bug

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The code still has the bug

## Screen 13
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect bad

we check the code again in the refreshed pane

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

## Screen 14
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect good

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This checks out another commit and we have no bug in our code as well. Zero steps left and git bisect is done.

## Screen 16
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Final output with statistics

highlight commit

highlight author

highlight time

highlight files changed

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This tells us everything we wanted to know. It shows us which commit is responsible for the bug, who the author was and the date when it happened.

Also, below, we see the files that were changed during that commit. Now we know who deserves some funny stuff in his coffee.


## Screen 17
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git status

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

We should check our status. We are in a detached HEAD state. Why? Because git has that commit checked out—not the last commit HEAD was pointing at when we last committed.

There is only one thing to do, we need to reset.

## Screen 18
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect reset

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

That means things go back to normal and can continue our work. Git will checkout the latest commit HEAD was pointing at on your current branch.

# Automatic Bisect

Would be cool to automate that hole bug checking business, wouldn’t it? We can bypass the manual aspect of this binary search by providing git with a little script.
  
This script should be able to tell if the current version that is checked out by git bisect is good or bad. Git bisect will use the exit status of the script to decide if the commit was in order or not.

## Screen 18
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect start

git bisect good hash

git bisect bad hash

git bisect run some_script

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


It’s important to note that this script should return either exit with 0 or 1 for example—depending on the success of the check in the script. Otherwise, git bisect will abort the whole process. 

If you use tests like Rspec specs for this purpose, this is automatically done for you. When the test fails, it returns a status that is non-zero.

## Screen 18
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git bisect start

git bisect good hash

git bisect bad hash

git bisect run spec/bisect_spec.rb:4

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The git bisect process is the same, but Git will handle the process alone. So for each commit git checks out, it will run the test without your involvement and record the response. 

In this end, it will show you again the commit that introduced the problem plus all the metadata. Pretty neat!
