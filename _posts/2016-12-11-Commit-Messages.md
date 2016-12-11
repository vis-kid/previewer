---
layout: post
title: Commit Messages
date: 2016-12-11 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Git]
---

# Topics

+ Why?
+ Good commit messages
+ Tips
+ Style
+ PRs

# Why?

## Screen 01

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
title

laptop terminal with bullet points
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Why should we care about good commit messages? When we started out, probably few of us paid attention to the quality of our commit messages.
  
In that context it might not have been that important. Working on one of the many beginner tutorials out there doesn’t create the need for a concise and precise commit history.

We are busy with figuring tons of more important things out and chances are small that anybody else needs to read these commit messages anyway.

But once we leave that egg hatching state behind, it is a good idea to start putting some thought into your commit history.

It’s not only the responsible thing to do for your team, it is a strategy to be considerate of your future self who might need to go through these commit and needs to make sense of them.

If I can promise you one thing then it is that the context in which you have finished some feature will be gone after a few weeks and you will have a hard time remembering what exactly was going on at that time in a projects history.

# Good Commit Messages

It’s probably not bad advice to treat commit messages as a mini diary for your projects. That doesn’t mean you should record too much context, your feelings or be overly verbose.

Simply provide enough information to keep things readable and understandable. Another developer or designer needs to be able to understand your reasoning and implementation.

Just imagine that you start to contribute to some cool open source project and you wanna suggest that superb new addition that you created. Trust me, you don’t wanna look like you don’t know what you are doing when you compose that pull request.

Not that people are ripping you a new one—except Linus maybe—but being able to express your thoughts in a concise and precise manner certainly elevates your contribution even more.

But you might need to rewrite your commit if a maintainer pushes for good commit messages. Again, no biggie, but often a scenario you can easily avoid. Saves everybody’s time as well.


# Tips

First things first. Keep the first line of a commit message concise and digestable. Avoid going over 50 characters We are simply looking for a summary. In many situations this ends up looking a lot better. Short logs for example.

## Screen 02

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
terminal laptop

50 characters first line

72 characters body text

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
  
Nobody wants to see you ramble. You have enough space below that first line. Oh and, capitalizing this text is a good idea as well.

If you need to, you should go to greater detail in the paragraphs below. Below the first line in the body, you can wrap paragraphs at 72 characters. Long, unreadable lines won’t benefit anybody.

It’s not unlike an email with a short subject line and a longer explanatory body.

Between the first line and the paragraphs below is a blank line. Always! It’s not a hard rule but it seems like a standard most people have adopted by now. The reason behind this is simple. Rebase doesn’t get confused that way.

If you are using vim, you can enhance your .vimrc to help you not going over the 72 characters width. We can set the textwidth to 72 characters.

## Screen 03

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
.vimrc

autocmd Filetype gitcommit setlocal spell textwidth=72

or simply

set textwidth=72

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Bot options work well, totally up to you.

## Screen 04

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
git commit -m "Fix login bug"

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

It is probably a great idea to almost never use the -m flag for messages when we commit. This feels like a bad habit that breeds lazy commit messages.

Writing your commit messages using the -m flag can easily trick us being influenced by the size of the current shell itself. For example, we might have tight space in some crammed terminal next to our editor and end up with typing only a few words that don’t explain anything.

## Screen 05

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Show small sized terminal with little space left. has stupid commit message

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
What we wanna achieve is write a page in our project’s history, not only a little cryptic sticky note that doesn’t convey much meaning without context. 

## Screen 06

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
showing a good example

Redirect user to the requested page after login

https://trello.com/path/to/relevant/card

Users were being redirected to the home page after login, which is less
useful than redirecting to the page they had originally requested before
being redirected to the login form.

* Store requested path in a session variable
* Redirect to the stored location after successfully

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

What should we pay attention to? The why and how? plus potential side effects.

## Screen 07

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Terminal

Why?

How?

Side effects

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

Why is this commit necessary is a good question to start with. Not only for writing the commit message, but for approaching the commit at all.

How did you achieve that goal, what did you do in that change set? Keep that description as high-level as possible and let the code speak for itself if somebody wants to dive deeper.

## Screen 08

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Introduce a red/black tree to increase search speed

More examples ...

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Pointing your team mates and your future self to possible side effects that this commit introduces is important as well.

Sure, not every commit will need this kind of detail, but thinking about each commit in such terms is a great approach of keeping your history focused and readable.

Marking commits that are little mistakes or fixes as squashes or fixups for later interactive rebase sessions should not give us an excuse for lazy messaging as well. 

## Screen 09

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
git commit --fixup

git commit --squash

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
## Screen 10

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Output

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Style

Use the imperative mood for the subject line. Instead of “Fixed issue #324” use “Fix issue #324”.

## Screen 11

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Fixed issue

Fix issue

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
The use of bullet points is totally fine. Use a hypen or asterisk for bullet points.

## Screen 12

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
commit messages with bullets

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# PRs

## Screen 13

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
laptop terminal: Pull Requests

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

All of the above basically apply to Pull Requests as well of course—maybe even more so since it is targeted at other developers who need to make sense of your code suggestion. You want to make their lives as easy as possible with your contribution.

If your team has some sort of process for code reviews before it gets merged into master or whatever, it is even more important to craft commit messages carefully I think. This can influence the team on multiple levels. Being able to approve a piece of code faster is just one aspect.

I guess a good overall guideline is to respect other people’s time and intelligence first and foremost. The rest is comes then easy and is icing on the cake. You will make mistakes and people will point them out to you eventually. But this is good, it is an opportunity to sharpen your skills and improve the quality of your commit process.

In reality, you will learn all these things very quickly if you pay attention to what other developers do or don’t. It’s no rocket science after all.

In the commit message, you should also describe why you suggest this code. Be specific what it is for and what are things that you wanna look out for. Links to issues, pull request or cards from Trello can be useful as well.

## Screen 14

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
commit message ends up in pull request

do a complete pull request

show result in browser

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


When you stick to the ideas that compose good commit messages, then you will rarely need to do extra work for pull rquests. They will end up being one and the same. Using a tool like github, it will write your pull request message for you. If your commit message is hitting all the points, then you will have no reason to change it for the PR.


## Screen 15

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
laptop terminal: Final Thoughts 

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
Overall, we wanna be able to present a story of a project through our commit history. The perceived quality of a project will most likely be influenced by they quality of your commit messages. Even the quality of individual developers or designers might be seen under a different light if you pay attention to your commit messages.

We wanna make Git pleasant to work with. Good, thoughtful commit messages play a bigger part in this as I originally thought when I started out. I recommend that you start paying attention to this little detail as soon as possible. The payoff is great. You can thank me later! But most of all, Tim Pope of course!
