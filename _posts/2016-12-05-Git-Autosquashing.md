---
layout: post
title: Git Auto-Squashing
date: 2016-12-05 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Git]
---

# Topics

+ Squash
+ Fixup
+ Automating things
+ No more SHAs

# Squash

If we have a new feature, we often need to squash it down to a couple of commits before it can be merged. That way the repo ends up with more meaningful commits overall.

Why should we squash at all? Squashing is cool because you can clean up your work. Some commits better don’t end up in a projects history. Not necessarily because they are embarassing but because they create unneeded noise in your commit history.

So far so good. What we also need to fix frequently are small mistakes that we don’t wanna spread all over our git history. Commits that address small mistakes are just noise really.

For example, commits that fix a little typo is nothing you wanna put in front of our colleagues or open source contributors. Giving everybody a concise and focused history of commits benefits everybody.


## Screen 00
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git log --oneline

Show history before fix commit
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Git gives us a few cool options to make our committing lives a bit more convenient. Especially when we need to clean things up.

Once you are a bit more comfortable with git, rebasing and squashing will become part of your daily routine. Autosquashing is one of these conveniences you should include in your workflow.

Squash and Fixup are two options for interactive rebasing. Using both of them together with autosquashing is super cool for easily removing the noise of small commits that only fix minor issues or mistakes.

This workflow starts with a new commit that we want to mark for later squashing. git commit offers two options to make our lives a bit easier. squash and fixup

## Screen 01
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git commit --fixup hash

git commit --squash hash

Showing the difference between  them?
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Both options will only work with interactive rebasing. The difference is how the commit messages are handled in the rebase session.

The resulting commit message will consist of the subject line of the specified commit—plus the prefix of fixup or squash with a bang. This automatically marks the new commit for the interactive rebase session. Both change the action from pick to squash or fixup in the interactive rebase session.

# Fixup


Typos or white space issues are good candidates for commits we better squash instead of keeping them around on their own.  Keeping track of such commits is unecessary if we mark them as FIXUPS.

With a fixup you tell git that there is no need to modify the original commit message. You just pass the hash of the commit that has the content plus the commit message you need.

With a squash, you tell git the you want to modify the commit message as well. At the end of the interactive session, you will be asked to compose a new commit message.

git commit fixup is simply a preparation for that fix to be squashed into another commit-without the need to change the commit message of that squash target.

## Screen 02
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Show history of commits with typo fix

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

## Screen 03
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git add .

git commit --fixup hash

git log 

show new commit on top
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

As you can see, this prefixes the commit message with fixup!(bang) and uses the commit message from the specified hash.

All it does is letting Git write a specific commit message for us. One that is triggering some automation during an interactive rebase session.

Both options must an include an existing commit to be squashed into. It like pre-selecting a squash option from the interactive rebase session during the commit itself.

A fixup is convenient because we don’t need to remember which commit is just a fix and needs to be squashed. Of course, we can do it manually in an interactive rebasing session as well. But remembering and reordering commits by hand gets boring and repetitive very quickly.

Especially if you work on a feature branch for a while before you squash it, this can be very handy. Manually reordering these commits on a branch that is days or weeks old is not exactly super convenient.

It’s easy to trip up as well. Instead we can mark such commits simply when we commit them. Easy as pie. 

When we are ready to rebase we can make use of the autosquash option. This saves us the time to think which commit gets squashed into another.

Without the auotsquash option, we cant take full advantage of the preparation we did by prefixing the commits with squash or fixup.

## Screen 04
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git rebase --interactive --autosquash master

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

This will trigger an interactive rebase session in your editor of choice. That means who have all the options available to fine tune your rebasing needs.

The main difference to a normal interactive rebase is that fixup commits are already in the right place, prepped to be squshed into the correct commit that also has the commit message you need. No need to rearrange things on your own

In the interactive rebase session, the commits are already organized automatically for us.

## Screen 05
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

pick aaa1111 A first commit

pick bbb2222 A second commit

fixup ddd4444 fixup! A second commit

pick ccc3333 A third commit

Show that fixup commit is now reordered right after the commit it is squashed into. It is already in the right place. 

Compapre to git log from before.
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Since we start with the oldest commit at the top, the commits to be squashed need to come right after the specified commit they get squashed into.

As you can see, the fixup commit is now already in that new position. It comes right after the specified commit in which it will be squashed into.

## Screen 06
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Highlight fixup!, squash!

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

We could have done this purely by hand in any interactive rebase session. But instead of fumbling around like this, Git is doing this automatically for us. Even nicer, Git already chose the fixup option for us as well.

## Screen 07
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

show normal rebase interactive

show rebase interactive autosquash 

compare

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

During the interactive rebase session, we can still move things around. What we have done so far was just a preparation. You still have the full power and flexibility of an interactive rebase session at your disposal.

## Screen 08
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

pick aaa1111 A first commit

pick bbb2222 A second commit

fixup ddd4444 fixup! A second commit

pick ccc3333 A third commit

Move things around
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Let’s quickly run through the second option autosquash option. We commit a little fix via git commit --squash hash.

## Screen 09
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git commit --squash hash

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

We will be prompted in our editor of choice to possibly change the commit message. Leave it as is.

When both messages of the fix and the squash target are the same, the interactive rebase session will pick the squash option for us automatically. If you change the message here, it won’t.

When we are ready to sqash our work we run again

## Screen 10
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git rebase --interactive --autosquash master

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

As you can see, the new commit is now in the right place to be squashed and has the squash option already selected for us.

## Screen 11
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

pick aaa1111 A first commit

pick bbb2222 A second commit

squash ddd4444 fixup! A second commit

pick ccc3333 A third commit

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

As a final step, after we save and close this session, we will be asked to compose a new commit message for the two squashed commits. We delete what we don’t need and enlighten our colleagues with our concise git poetry. 

That’s it. A quick look at the log tells us that everything is in order, cleaned up and has a new commit message.

## Screen 12
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git log

output

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Automating things

## Screen 13
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git rebase --interactive --autosquash

with autosquash option

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Since git rebase interactive autosquash is only focused on commits that has a message that begins with “fixup!” or “squash!”.

Why shouldn’t we autosquash all the time by default when we do an interactive rebase? Exactly!

## Screen 14
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git rebase --interactive

without autosquash option

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Lucky for us, we can set this as the default via the rebase.autosquash setting

## Screen 15
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

rebase.autosquash

$git config --global rebase.autosquash true

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

That makes your interactive rebasing session do an autosquash by default. Since it only picks up on squash! and fixup! it has no side effects that you need to be concerned about.

Now we can save ourselves from typing the extra --autosquash option all the time.

# No more SHAs

One minor pain is left that we can optimize. Instead of typing or copy pasting the specific SHA for the commit we wanna squash into, wouldn’t it be cool to just type part of the commit message instead? 

## Screen 16
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

git commit --fixup :/second

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

If we use this syntax, Git will find the most recent commit that has that string in the first line of the commit message. The most recent one being key here. If you need to go back further in history, a commit hash might offer more efficient results.
