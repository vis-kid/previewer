---
layout: post
title: Git Rebase Mixtape
date: 2016-11-02 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Git]
---

The last video, merge vs rebase, gave you a solid basis to integrate changes from one branch into another. But unsurprisingly, Git is a deep tool and there is more to learn about rebase.

We need to cover a bit more ground to get you ready for some real action. 

You will need to understand when you can and when you definitely cannot do a rebase. I also want to show you how to cherry pick a few selected commits instead of whole branches.

Understanding interactive rebasing and doing a git pull --rebase rounds off what you should understand to step up your game.

Let’s start with the dangers of rebase first and work up our appetite and save cherry picking for last.

# Rebasing a pushed remote

## Animation
**Danger Skull**

If you are not careful with rebasing, you can quickly enter a world of pain.

If you rebase commits that have been outside your own repository, you have a high likelyhood of making people hate you—or wanting to put funny stuff in your coffee.
## Animation
**Poo in a cup of coffee**

## Animation
**Rebasing from a third branch that has an cloud / GitHub icon

Don’t ever do that! You will potentially mess with the history of other people’s forks if they already have build on top of the work you did before you rewrote history with a rebase.

That means you can play back to the future at home, within your own repo, but when it comes to work that was already pushed to a remote repo that others have access to, the rules change. Never, ever do that! A world of pain will wait for you, and worse, potentially for everybody involved!

If you need to git push --force something with a rebase, it should be warning sign that you need to stop and probably think what you are doing!

Just remember to rebase only local changes and you and everybody else’s work will be safe.


