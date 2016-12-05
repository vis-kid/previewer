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

git log --oneline

Git gives us a few cool options to make our committing lives a bit more convenient. Autosquashing is one of them. Once you are a bit more comfortable with git, rebasing and squashing will become part of your daily routine.

Squash and Fixup are two options for interactive  rebasing. Using them both together is super convenient for removing the noise of small commits that only fix minor issues or mistakes.

git commit offers two options to make our lives a bit easier.

###################################################################################################
###################################################################################################

git commit --fixup
git commit --squash?? --autosqaush??

###################################################################################################
###################################################################################################

If we have a new feature, it is often asked of you to squash it down into one or two commits—three tops before it can be merged. That way the repo ends up with more meaningful commits overall.

Commits that address small mistakes are just noise.

git commit --fixup will omit its own commit message and uses the one from the hash specified.

With a fixup you tell git that there is no need to modify the original commit message. You just pass the hash of the commit that has the content and commit message you wanna squash this fix into.

# Squash

Why should we squash? Squashing is cool because you can clean up your work. Some commits better don’t end up in a projects history. Not necessarily because they are embarassing but because they create unneeded noise in your commit history.

For example, commits that address a little typo fix is nothing you wanna put in front of your colleagues. Giving everybody a concise and focused history of commits benefits everybody.

# Fixup

A fixup is simply an extension to another commit it will get squashed into.

Typos or white space issues are good candidates for commits we better squash instead of keeping them around on their own.  Keeping track of such commits is unecessary if we mark them as FIXUPS.

###################################################################################################
###################################################################################################

Show history of commits with typo fix

###################################################################################################
###################################################################################################


When we mark a commit as a fixup to another commit, we also tell Git that we don’t need the commit message. Git will discard it completely.

###################################################################################################
###################################################################################################

git add .
git commit --fixup hash

###################################################################################################
###################################################################################################

A fixup is convenient because we don’t need to remember which commit is just a fix and needs to be squashed. We can do it manually in an interactive rebasing session, but remembering and reordering commits by hand for it gets boring very quickly.

When we are ready to rebase we can make use of the autosquash option. This saves us the time to think which commit gets squashed into another.


###################################################################################################
###################################################################################################

git rebase --interactive --autosquash master

###################################################################################################
###################################################################################################

This will of course an interactive rebase session in your editor of choice. That means who have all the options available to fine tune your rebasing needs. The main difference to a normal interactive rebase is that fixup commits are already in the right place, prepped to be squshed into the correct commit that also has the commit message you need. No need to rearrange things on your own

###################################################################################################
###################################################################################################

pick aaa1111 A first commit
pick bbb2222 A second commit
fixup ddd4444 fixup! A second commit
pick ccc3333 A third commit

###################################################################################################
###################################################################################################

# Automating things

###################################################################################################
###################################################################################################

show normal rebase interactive
show rebase interactive autosquash 
compare

###################################################################################################
###################################################################################################

Why shouldn’t we autosquash all the time by default when we do an interactive rebase? Exactly.

###################################################################################################
###################################################################################################

git rebase --interactive --autosquash

###################################################################################################
###################################################################################################

git rebase interactive autosquash is only focused on commits that has a message that begins with “fixup!” or “squash!”.

###################################################################################################
###################################################################################################

Highlight fixup!, squash!

###################################################################################################
###################################################################################################

You can set this as the default via the rebase.autosquash setting

###################################################################################################
###################################################################################################

rebase.autosquash

$git config --global rebase.autosquash true

###################################################################################################
###################################################################################################

That makes your interactive rebasing session do an autosquash by default. Since it only picks up on squash! and fixup! it has no side effects that you need to be concerned about.

# No more SHAs
