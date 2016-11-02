---
layout: post
title: Building Your First Webscraper 03
date: 2016-11-02 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Mechanic, Nokogiri, Ruby]
---

# Topics

+ Scraping my Podcast

# Scraping my Podcast

Let’s put what we learned into practice. For various  reasons, a re-design for my podcast [Between \| Screens](http://betweenscreens.fm/) was long overdue. There were design issues that made me scream when I woke up in the morning and I really disliked the whole publishing workflow. Out of a quick fix, I went with a WYSIWYG editor and I could kick myself for that decision after I’m done screaming. 

#### **WYSIWYG Editor Mornings**

![Alt text](/images/webscraper/al-bundy.gif)

Let’s be frank, I made the wrong choice technology-wise. I wanted to have a small project that is perfect for playing with Sinatra properly. A podcast seemed like the perfect choice for that. Reasonably small project with minor database needs. What could go wrong? Aside from pulling your hair out when you are a Vim addict when you prepare new episodes in the browser, the site never was a big pleasure to work with. When I ran into exotic errors, one can also not expect to consult Dr. Google like you can do with Rails for example. The amount of people running into the same errors as you is just a LOT smaller.

That means you have to spend a lot more time bug fixing than I was used to over the last couple of years. Overall this is fine and dandy, after all it is good practice. For a side project it was just a bit too time consuming when you also want to produce new episodes a few times per week. Sure, it was educational to build everything myself from the ground up but I left a lot of time on the table with needless fights. Don’t get me wrong, Sinatra is a nice tool, no it is a great tool, but for this job I should have chosen more wisely. I should have either gone with a Rails app or a plain static site that uses Jekyll or Middleman that runs on GitHub pages. In hindsight, that would have made my podcasting life a whole lot more joyful. Anyway.

For version two of my podcasting site I chose to build something with Middleman and host it with GitHub pages. I wanted a fast static site and the Git workflow for publishing new episodes. Simply deploying new `.markdown` files—just like a blog post—sounded like heaven to me.

 Below are two screenshots from my podcast [Between \| Screens](http://betweenscreens.fm/). 

#### **Screenshot Old Podcast**

![Alt text](/images/webscraper/between-screens-old-01.png)

#### **Screenshot New Podcast**

![Alt text](/images/webscraper/between-screens-new-01.png)


