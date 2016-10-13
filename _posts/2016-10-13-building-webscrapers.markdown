---
layout: post
title: Building Webscrapers
date: 2016-10-13 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Mechanic, Nokogiri, Ruby]
---

## Topics

+ The Problem

# The Problem

I needed to build a new podcast. The design was not where I wanted it to be and I hated to way to publish new posts. A little bit of context. About two years ago, I built the first version of my podcast. The idea was to play with Sinatra and build something super simple and lightweight. I ran into a couple of unexpected issues since I tailor made pretty much everything. Coming from Rails it definitely was an educational journey that I appreciate, but I quickly regretted not having used a static site that I ran on GitHub via GitHub pages. Deploying new episodes and maintaining lacked the simplicity that I was looking for. I decided to have bigger fish to fry and focus on producing new podcast material instead for now.

This past summer I started to get serious and worked on a Middleman site that is hosted via GitHub pages. For season  two of the show I wanted something fresh. A new, simplified design, markdown for publishing new episodes and no fist fights with Heroku—heaven! The thing was that I had 139 episodes lying around that needed to be converted in order to work with Middleman. For posts, Middleman uses .markdown files that have so called frontmatter—which replaces my database basically. Doing this transfer by hand is not an option at 139 episodes. That’s what computation is for. I needed to figure out a way to parse the HTML of my old website, scrape the relevant content and transfer it to blog posts on Middleman that I use for publishing new podcast episodes. Therefore, over the next three articles, I’m gonna introduce you to the tools commonly used in Ruby land for such tasks. In the end, we’ll go over my solution to show you something practical as well.


