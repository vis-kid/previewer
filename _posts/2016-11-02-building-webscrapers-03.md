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

Let’s put what we learned into practice. For various  reasons, a re-design for my podcast [Between \| Screens](http://betweenscreens.fm/) was long overdue. There were issues that made me scream when I woke up in the morning. Also, I really disliked the whole publishing workflow and the site’s speed. The design also didn’t get the amount of love that I originally planned. The cherry on top came out of a quick fix. I went with a WYSIWYG editor and I could kick myself for that decision every day after I’m done screaming. Change was imperative! Even if I would need to stop podcasting for a while.

#### **WYSIWYG Editor Mornings**

![Alt text](/images/webscraper/al-bundy.gif)

Let’s be frank, I made the wrong choice technology-wise. I wanted to have a small project that is perfect for playing with Sinatra properly. A podcast seemed like the perfect choice for that. Reasonably small project with minor database needs. What could go wrong? Aside from pulling your hair out as a Vim addict when you prepare new episodes in the browser, the site never was a big pleasure to work with. When I ran into exotic errors, one can also not expect to consult Dr. Google like you can do with Rails for example. The amount of people running into the same errors is just a LOT smaller.

That means you have to spend a lot more time bug fixing than I was used to over the last couple of years. Overall this is fine and dandy, after all it is good practice. For a side project it was just a bit too time consuming when you also want to produce new episodes a few times per week. Sure, it was educational to build everything myself from the ground up but I left a lot of time on the table with needless fights.

Don’t get me wrong, Sinatra is a nice tool, no it is a great tool, but for this job I should have chosen more wisely. This was not setup to scale. I should have either gone with a Rails app or a plain static site that uses Jekyll or Middleman that runs on GitHub pages. In hindsight, that would have made my podcasting life a whole lot more joyful. Anyway.

For version two of my podcasting site I chose to build something with Middleman and host it with GitHub pages. I wanted a fast static site and the Git workflow for publishing new episodes. Simply deploying episodes through `.markdown` files that I can compose right here in Vim—just like a blog post—sounded like heaven to me. After all, show notes can result in quite a bit of text every day. This is not something that I wanted to spend any second more than necessary.

After I worked on the Middleman site and invested a good amount of time on the new design, I needed to import the content from my database backed Sinatra app into my new static site. Doing this by hand like in schmuck fashion was not on the table—not even a question since I could rely on my friends Nokogiri and Mechanize to do the job for me. What is ahead of you is a reasonably small scrape job that is not too complicated but it has a few interesting twists that I thought could be educational for the web scraping newbies out there. Below are two screenshots from my podcast [Between \| Screens](http://betweenscreens.fm/). 

#### **Screenshot Old Podcast**

![Alt text](/images/webscraper/between-screens-old-01.png)

#### **Screenshot New Podcast**

![Alt text](/images/webscraper/between-screens-new-01.png)

Let’s break down what we want to accomplish.

We want to extract:

+ the headers
+ the interviewee
+ the date
+ the SoundCloud track number
+ the text for the show notes
+ the subheader
+ the show notes
+ the links
+ the sponsor information

Using that scraped data, we want to populate the [frontmatter]() of markdown files for each episode and add a few tricks that the old site didn’t play. Having a comprehensive tagging system in place was crucial for me. I wanted listeners who use the site to have a deep discovery tool.  Therefore, I wanted to create tags for every interviewee and out of the subheader. Since I produced around 140 episodes in the first season alone, I wanted to prep the site for a time when the content becomes harder to comb through. A deep tagging system with intelligently placed recommendations was the way to go for me.
