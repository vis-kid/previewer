---
layout: post
title: Building Your First Webscraper
date: 2016-10-13 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Mechanic, Nokogiri, Ruby]
---

## Topics

+ Web Scraping?
+ Permission
+ The Problem
+ Nokogiri

# Web Scraping?

There are fancier terms around, like web harvesting and web data extraction that pretty much tell you right away what’s going on. You can automate the extraction of data from web pages—and it’s not that complicated as well. In a way, these tools allow you to imitate and automate human web browsing. You write a program and give it specific instructions what sort of data is of interest to you. Targeting specific data is almost as easy as using CSS selectors.

I remember a few years ago when I subscribed to some online video course that had like a million short videos to download but no option to do that in bulk. I had to go through every link on my own and do the dreaded ‘save as’ myself. It was sort of human web scraping—something that we often need to do when we lack the knowledge to automate that kinda stuff. The course itself was alright but I didn’t use their services anymore after that. Today, I wouldn’t care too much about such mind-melting UX. A scraper that would do the downloading for me would take me only a couple of minutes to throw together. No biggie!

Let me break it down real quick before we start. The whole thing can be condensed into a couple of steps. First we fetch a web page that has the desired data we need. Then we search through that page and identify the information we want to extract. The final step is to target these bits and decide where you want to store them. Well-written HTML is often key to making this process easy and enjoyable. For more involved extractions, it can be a pain in the butt if you have to deal with poorly structured markup.

What about APIs? Very good question? If you have access to a service with an API, there is little need to write your own scraper. This approach is mostly for websites that don’t offer that sort of convenience. Without an API, this is often the only way to automate the extraction of information from websites.

There are many approaches and solutions out there to play with. Rubyland has two gems that occupy the spotlight for a number of years now. Many people still rely on Nokogiri and Mechanize for HTML scraping needs. Both have been tested and proven themselves to be easy to use while being highly capable. We will look at both of them. But before that, I’d like to take a moment to address the problem that we are gonna solve in this short introduction.

# Permission

Before you start scraping away, make sure you have the permission of the sites you are trying to access for data extraction. If the site has an API or RSS feed for example, it might not only be easier to get that desired content, it might also be the legal option of choice. Not everybody will appreciate if you do massive scraping on their sites—understandably so. Get yourself educated on that particular site you are interested in and don’t get yourself in trouble. Chances are low that you will inflict serious damage but risking trouble unknowingly is not the way to go.

# The Problem

I needed to build a new podcast. The design was not where I wanted it to be and I hated to way to publish new posts. A little bit of context. About two years ago, I built the first version of my podcast. The idea was to play with Sinatra and build something super simple and lightweight. I ran into a couple of unexpected issues since I tailor made pretty much everything. Coming from Rails it definitely was an educational journey that I appreciate, but I quickly regretted not having used a static site that I ran on GitHub via GitHub pages. Deploying new episodes and maintaining lacked the simplicity that I was looking for. I decided to have bigger fish to fry and focus on producing new podcast material instead for now.

This past summer I started to get serious and worked on a Middleman site that is hosted via GitHub pages. For season  two of the show I wanted something fresh. A new, simplified design, markdown for publishing new episodes and no fist fights with Heroku—heaven! The thing was that I had 139 episodes lying around that needed to be converted in order to work with Middleman. For posts, Middleman uses .markdown files that have so called frontmatter—which replaces my database basically. Doing this transfer by hand is not an option at 139 episodes. That’s what computation is for. I needed to figure out a way to parse the HTML of my old website, scrape the relevant content and transfer it to blog posts on Middleman that I use for publishing new podcast episodes. Therefore, over the next three articles, I’m gonna introduce you to the tools commonly used in Ruby land for such tasks. In the end, we’ll go over my solution to show you something practical as well.

# Nokogiri

Even if you are completely new to Ruby / Rails land, chances are very good that you have already heard about this little gem. The name is dropped often and sticks with you easily. Nokogiri is fast and offers a nice interface. It is a very potent tool that takes care of a multitude of HTML scraping needs. You can use Nokogiri not only to parse HTML, XML is fair game as well.

Nokogiri gives you the options of both XML path language and CSS interfaces to traverse the documents you load. XML path Language, or XPath for short, is a query language. It allows us to select nodes from XML documents. CSS selectors are most likely more familiar to beginners. As with styles you write, they make it fantastically easy to target specific sections of pages that are of interest for extraction. With that powerful gem, XML trees are as easy to parse as the DOM. You just need to let Nokogiri know what you are after when you target a particular destination.

## Pages

What we always need to start with is fetching the actual page we are interested in. We need to specify what kind of Nokogiri document we want to have—XML or HTML for example:

``` ruby

Nokogiri::XML
Nokogiri::HTML

```

#### **some_scraper.rb**

``` ruby

require "nokogiri"
require "open-uri"

page = Nokogiri::XML(File.open("some.xml"))
page = Nokogiri::HTML(File.open("some.html"))

```

What happens above is straightforward. This opens and fetches the designated page using `open-uri` and then loads its structure, its XML or HTML into a new Nokogiri document. XML is not something beginners have to deal with very often. Therefore I’d recommend that we focus on HTML parsing for now. Why `open-uri`? This module from the Ruby Standard Library lets us grab the site without much fuzz.

Let’s put this to practice with a mini example:

#### **some_podcast_scraper.rb**

``` ruby

require 'nokogiri'
require "open-uri"

url = 'http://betweenscreens.fm/'

page = Nokogiri::HTML(open(url))

header = page.at_css("h2.post-title")

title = header.text

puts "This is the raw header of the latest episode: #{header}"

puts "This is the title of the latest episode: #{title}"

```

What we did here represents all the steps that are usually involved with web scraping—just at a micro level. We decide with url we need, which site we need to fetch and load into a new Nokogiri document. Then we open that page and target a specific section.

Here I only wanted to know the title of the latest episode. Using the ```at_css``` method and a CSS selector for `h2.post-title` was all I needed to target the extraction point. With this method we will only scrape this singular element though. This gives us the whole selector—which is most of the time not exactly what we need. Therefore we extract only the inner text portion of this node via the `text` method. For comparison, you can check the output for both the header and the text below.

### Attention!

If you are a beginner and not sure how to target the HTML needed for this, I recommend that you google how to inspect the contents of websites in your browser. Basically all major browsers make this process really easy these days. On Chrome you just need to right-click on an element in the website and choose the inspect option. This will open a small window at the bottom of your browser which shows you something like an x-ray of the site’s DOM. It has many more options and I would recommend spending some time on Google to educate yourself. This is time spent wise!

#### **Output**

``` bash

This is the raw title of the latest episode: <h2 class="post-title"><a href="episodes/142/">David Heinemeier Hansson</a></h2>

This is the title of the latest episode: David Heinemeier Hansson

```

Although this example has very limited applications, it possesses all the ingredients, all the steps that you need to understand. I think its kinda cool how simple this is. Because it might now be obvious from this example, I would like to point out how powerful this tool can be. 


# Command Line Scraping
