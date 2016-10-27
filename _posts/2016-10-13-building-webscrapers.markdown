---
layout: post
title: Building Your First Webscraper 01
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
+ Extraction?
+ Pages
+ API
+ Node Navigation

# Web Scraping?

Screen scraping

There are fancier terms around, like web harvesting and web data extraction that pretty much tell you right away what’s going on. You can automate the extraction of data from web pages—and it’s not that complicated as well. In a way, these tools allow you to imitate and automate human web browsing. You write a program and give it specific instructions what sort of data is of interest to you. Targeting specific data is almost as easy as using CSS selectors.

I remember a few years ago when I subscribed to some online video course that had like a million short videos to download but no option to do that in bulk. I had to go through every link on my own and do the dreaded ‘save as’ myself. It was sort of human web scraping—something that we often need to do when we lack the knowledge to automate that kinda stuff. The course itself was alright but I didn’t use their services anymore after that. Today, I wouldn’t care too much about such mind-melting UX. A scraper that would do the downloading for me would take me only a couple of minutes to throw together. No biggie!

Let me break it down real quick before we start. The whole thing can be condensed into a couple of steps. First we fetch a web page that has the desired data we need. Then we search through that page and identify the information we want to extract. The final step is to target these bits and decide where you want to store them. Well-written HTML is often key to making this process easy and enjoyable. For more involved extractions, it can be a pain in the butt if you have to deal with poorly structured markup.

What about APIs? Very good question? If you have access to a service with an API, there is little need to write your own scraper. This approach is mostly for websites that don’t offer that sort of convenience. Without an API, this is often the only way to automate the extraction of information from websites. You might ask, how does this scraping thing actually work? Without jumping into the deep end, the short answer is, by traversing tree data structures. Nokogiri builds these data structures from the documents you feed it and lets you target bits of interest in it. For example, CSS is a language written for tree traversal, for searching tree data strcutures, and we can make use of it for data extraction.

There are many approaches and solutions out there to play with. Rubyland has two gems that occupy the spotlight for a number of years now. Many people still rely on Nokogiri and Mechanize for HTML scraping needs. Both have been tested and proven themselves to be easy to use while being highly capable. We will look at both of them. But before that, I’d like to take a moment to address the problem that we are gonna solve in this short introduction.

# Permission

Before you start scraping away, make sure you have the permission of the sites you are trying to access for data extraction. If the site has an API or RSS feed for example, it might not only be easier to get that desired content, it might also be the legal option of choice. Not everybody will appreciate if you do massive scraping on their sites—understandably so. Get yourself educated on that particular site you are interested in and don’t get yourself in trouble. Chances are low that you will inflict serious damage but risking trouble unknowingly is not the way to go.

# The Problem

I needed to build a new podcast. The design was not where I wanted it to be and I hated to way to publish new posts. A little bit of context. About two years ago, I built the first version of my podcast. The idea was to play with Sinatra and build something super simple and lightweight. I ran into a couple of unexpected issues since I tailor made pretty much everything. Coming from Rails it definitely was an educational journey that I appreciate, but I quickly regretted not having used a static site that I ran on GitHub via GitHub pages. Deploying new episodes and maintaining lacked the simplicity that I was looking for. I decided to have bigger fish to fry and focus on producing new podcast material instead for now.

This past summer I started to get serious and worked on a Middleman site that is hosted via GitHub pages. For season  two of the show I wanted something fresh. A new, simplified design, markdown for publishing new episodes and no fist fights with Heroku—heaven! The thing was that I had 139 episodes lying around that needed to be converted in order to work with Middleman. For posts, Middleman uses .markdown files that have so called frontmatter—which replaces my database basically. Doing this transfer by hand is not an option at 139 episodes. That’s what computation is for. I needed to figure out a way to parse the HTML of my old website, scrape the relevant content and transfer it to blog posts on Middleman that I use for publishing new podcast episodes. Therefore, over the next three articles, I’m gonna introduce you to the tools commonly used in Ruby land for such tasks. In the end, we’ll go over my solution to show you something practical as well.

# Nokogiri

Even if you are completely new to Ruby / Rails land, chances are very good that you have already heard about this little gem. The name is dropped often and sticks with you easily. The creator of this gem is the love Tenderlove, [Aaron Patterson](https://twitter.com/tenderlove). Nokogiri converts XML and HTML documents into a data structure—a tree data structure to be more precise. It is fast, offers a nice interface as well. Overall, it’s a very potent tool that takes care of a multitude of HTML scraping needs. You can use Nokogiri not only to parse HTML, XML is fair game as well.

Nokogiri gives you the options of both XML path language and CSS interfaces to traverse the documents you load. XML path Language, or XPath for short, is a query language. It allows us to select nodes from XML documents. CSS selectors are most likely more familiar to beginners. As with styles you write, they make it fantastically easy to target specific sections of pages that are of interest for extraction. With that powerful gem, XML trees are as easy to parse as the DOM. You just need to let Nokogiri know what you are after when you target a particular destination.

# Pages

What we always need to start with is fetching the actual page we are interested in. We need to specify what kind of Nokogiri document we want to parse—XML or HTML for example:

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

`Nokogiri:XML` and `Nokogiri:HTML` can take IO objects or String objects. What happens above is straightforward. This opens and fetches the designated page using `open-uri` and then loads its structure, its XML or HTML into a new Nokogiri document.  XML is not something beginners have to deal with very often. Therefore I’d recommend that we focus on HTML parsing for now. Why `open-uri`? This module from the Ruby Standard Library lets us grab the site without much fuzz. Because IO objects are fair game, we can make easy use of `open-uri`.

# API

Let’s put this to practice with a mini example:

## at_css

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

Although this example has very limited applications, it possesses all the ingredients, all the steps that you need to understand. I think its kinda cool how simple this is. Because it might not be obvious from this example, I would like to point out how powerful this tool can be. Let’s see what else we can do with a Nokogiri script.

## css

The `css` method will give us not only a single element of choice but any element that matches the search criteria on the page. Pretty neat and straightforward!

#### **some_scraper.rb**

``` ruby

require 'nokogiri'
require "open-uri"

url = 'http://betweenscreens.fm/'

page = Nokogiri::HTML(open(url))

header = page.css("h2.post-title")

header.each do |header|
  puts "This is the raw title of the latest episode: #{header}"
end

header.each do |header|
  puts "This is the title of the latest episode: #{header.text}"
end

```

#### Output

``` bash

This is the raw title of the latest episode: <h2 class="post-title"><a href="episodes/142/">David Heinemeier Hansson</a></h2>
This is the raw title of the latest episode: <h2 class="post-title"><a href="episodes/141/">Zach Holman</a></h2>
This is the raw title of the latest episode: <h2 class="post-title"><a href="episodes/140/">Joel Glovier</a></h2>
This is the raw title of the latest episode: <h2 class="post-title"><a href="episodes/139/">João Ferreira</a></h2>
This is the raw title of the latest episode: <h2 class="post-title"><a href="episodes/138/">Corwin Harrell</a></h2>
This is the raw title of the latest episode: <h2 class="post-title"><a href="episodes/137/">Roberto Machado</a></h2>
This is the raw title of the latest episode: <h2 class="post-title"><a href="episodes/136/">James Edward Gray II</a></h2>

This is the title of the latest episode: David Heinemeier Hansson
This is the title of the latest episode: Zach Holman
This is the title of the latest episode: Joel Glovier
This is the title of the latest episode: João Ferreira
This is the title of the latest episode: Corwin Harrell
This is the title of the latest episode: Roberto Machado
This is the title of the latest episode: James Edward Gray II

```

The only little difference in this example is that I iterate on the raw header first before I extract its inner text with the `text` method. Nokogiri automatically stops at the end of the page and does not attempt to follow its pagination anywhere.

Let’s say we want to have a bit more information, say the date and the subtitle for each episode. We can simply expand on the example above. It is a good idea anyway to take this step by step. Get a little piece working and add in more complexity along the way.

#### **some_scraper.rb**

``` ruby

require 'nokogiri'
require "open-uri"

url = 'http://betweenscreens.fm/'

page = Nokogiri::HTML(open(url))

articles = page.css("article.index-article")

articles.each do |article|
  header     = article.at_css("h2.post-title")
  date       = article.at_css(".post-date")
  subtitle   = article.at_css(".topic-list")

  puts "This is the raw header:    #{header}"
  puts "This is the raw date:      #{date}"
  puts "This is the raw subtitle:  #{subtitle}\n\n"

  puts "This is the text header:   #{header.text}"
  puts "This is the text date:     #{date.text}"
  puts "This is the text subtitle: #{subtitle.text}\n\n"
end

```

#### Output

``` bash

This is the raw header: <h2 class="post-title"><a href="episodes/142/">David Heinemeier Hansson</a></h2>
This is the raw date: <span class="post-date">Oct 18 | 2016</span>
This is the raw subtitle: <h3 class="topic-list">Rails community | Tone | Technical disagreements | Community policing | Ungratefulness | No assholes allowed | Basecamp | Open source persona | Aspirations | Guarding motivations | Dealing with audiences | Pressure | Honesty | Diverse opinions | Small talk</h3>

This is the text header: David Heinemeier Hansson
This is the text date: Oct 18 | 2016
This is the text subtitle: Rails community | Tone | Technical disagreements | Community policing | Ungratefulness | No assholes allowed | Basecamp | Open source persona | Aspirations | Guarding motivations | Dealing with audiences | Pressure | Honesty | Diverse opinions | Small talk

This is the raw header: <h2 class="post-title"><a href="episodes/141/">Zach Holman</a></h2>
This is the raw date: <span class="post-date">Oct 12 | 2016</span>
This is the raw subtitle: <h3 class="topic-list">Getting Fired | Taboo | Transparency | Different Perspectives | Timing | Growth Stages | Employment &amp; Dating | Managers | At-will Employment | Tech Industry | Europe | Low hanging Fruits | Performance Improvement Plans | Meeting Goals | Surprise Firings | Firing Fast | Mistakes | Company Culture | Communication</h3>

This is the text header: Zach Holman
This is the text date: Oct 12 | 2016
This is the text subtitle: Getting Fired | Taboo | Transparency | Different Perspectives | Timing | Growth Stages | Employment & Dating | Managers | At-will Employment | Tech Industry | Europe | Low hanging Fruits | Performance Improvement Plans | Meeting Goals | Surprise Firings | Firing Fast | Mistakes | Company Culture | Communication

This is the raw header: <h2 class="post-title"><a href="episodes/140/">Joel Glovier</a></h2>
This is the raw date: <span class="post-date">Oct 10 | 2016</span>
This is the raw subtitle: <h3 class="topic-list">Digital Product Design | Product Design @ GitHub | Loving Design | Order &amp; Chaos | Drawing | Web Design | HospitalRun | Diversity | Startup Culture | Improving Lives | CURE International | Ember | Offline First | Hospital Information System | Designers &amp; Open Source</h3>

This is the text header: Joel Glovier
This is the text date: Oct 10 | 2016
This is the text subtitle: Digital Product Design | Product Design @ GitHub | Loving Design | Order & Chaos | Drawing | Web Design | HospitalRun | Diversity | Startup Culture | Improving Lives | CURE International | Ember | Offline First | Hospital Information System | Designers & Open Source

This is the raw header: <h2 class="post-title"><a href="episodes/139/">João Ferreira</a></h2>
This is the raw date: <span class="post-date">Aug 26 | 2015</span>
This is the raw subtitle: <h3 class="topic-list">Masters @ Work | Subvisual | Deadlines | Design personality | Design problems | Team | Pushing envelopes | Delightful experiences | Perfecting details | Company values</h3>

This is the text header: João Ferreira
This is the text date: Aug 26 | 2015
This is the text subtitle: Masters @ Work | Subvisual | Deadlines | Design personality | Design problems | Team | Pushing envelopes | Delightful experiences | Perfecting details | Company values

This is the raw header: <h2 class="post-title"><a href="episodes/138/">Corwin Harrell</a></h2>
This is the raw date: <span class="post-date">Aug 06 | 2015</span>
This is the raw subtitle: <h3 class="topic-list">Q&amp;A | 01 | University | Graphic design | Design setup | Sublime | Atom | thoughtbot | Working location | Collaboration &amp; pairing | Vim advocates | Daily routine | Standups | Clients | Coffee walks | Investment Fridays |</h3>

This is the text header: Corwin Harrell
This is the text date: Aug 06 | 2015
This is the text subtitle: Q&A | 01 | University | Graphic design | Design setup | Sublime | Atom | thoughtbot | Working location | Collaboration & pairing | Vim advocates | Daily routine | Standups | Clients | Coffee walks | Investment Fridays |

This is the raw header: <h2 class="post-title"><a href="episodes/137/">Roberto Machado</a></h2>
This is the raw date: <span class="post-date">Aug 03 | 2015</span>
This is the raw subtitle: <h3 class="topic-list">CEO @ Subvisual | RubyConf Portugal | Creators School | Consultancy | Company role models | Group Buddies | Portuguese startup | Rebranding | Technologies used | JS frameworks | TDD &amp; BDD | Startup mistakes | Culture of learning | Young entrepreneurs</h3>

This is the text header: Roberto Machado
This is the text date: Aug 03 | 2015
This is the text subtitle: CEO @ Subvisual | RubyConf Portugal | Creators School | Consultancy | Company role models | Group Buddies | Portuguese startup | Rebranding | Technologies used | JS frameworks | TDD & BDD | Startup mistakes | Culture of learning | Young entrepreneurs

This is the raw header: <h2 class="post-title"><a href="episodes/136/">James Edward Gray II</a></h2>
This is the raw date: <span class="post-date">Jul 30 | 2015</span>
This is the raw subtitle: <h3 class="topic-list">Screencasting | Less Code | Reading code | Getting unstuck | Rails’s codebase | CodeNewbie | Small examples | Future plans | PeepCode | Frequency &amp; pricing</h3>

This is the text header: James Edward Gray II
This is the text date: Jul 30 | 2015
This is the text subtitle: Screencasting | Less Code | Reading code | Getting unstuck | Rails’s codebase | CodeNewbie | Small examples | Future plans | PeepCode | Frequency & pricing

```

At this point we already have some data to play with. We can structure or butcher it any way we like. The above should simply show what we have in a readable fashion. Of course we can dig deeper into each of these by using regular expressions with the `text` method. We will look into this a lot more in detail when we get to solving the actual problem. It won’t be a class on regexp but you will see some more of it in action—but no worries, not so much to make your brains bleed.

## Attributes

What could be handy at this stage is extracting the `href` for the individual episodes. It couldn’t be any simpler.

#### **some_scraper.rb**

``` ruby

require 'nokogiri'
require "open-uri"

url = 'http://betweenscreens.fm/'

page = Nokogiri::HTML(open(url))

articles = page.css("article.index-article")

articles.each do |article|
  header      = article.at_css("h2.post-title")
  date        = article.at_css(".post-date")
  subtitle    = article.at_css(".topic-list")
  href        = article.at_css("h2.post-title a")
  podcast_url = "http://betweenscreens.fm/"

  puts "This is the raw header:    #{header}"
  puts "This is the raw date:      #{date}"
  puts "This is the raw subtitle:  #{subtitle}"
  puts "This is the raw link:      #{href}\n\n"

  puts "This is the text header:   #{header.text}"
  puts "This is the text date:     #{date.text}"
  puts "This is the text subtitle: #{subtitle.text}"
  puts "This is the raw link:      #{podcast_url}#{href[:href]}\n\n"
end

```

The most important bits to pay attention to here are `[:href]` and ```podcast_url```. If you tag on `[:]` you can simply extract an attribute from the targeted selector. I abstracted a little further, but you can see below more clearly how it works.

``` ruby

...

href = article.at_css("h2.post-title a")[:href]

...

```

To get a complete and useful url, I saved the root domain in a variable and constructed the full url for each episode.

``` ruby

...

podcast_url = "http://betweenscreens.fm/"

puts "This is the raw link: #{podcast_url}#{href[:href]}\n\n"

...

```

Let’s take a quick look at the output for comparison:

#### **Output**

``` bash

This is the raw header:   <h2 class="post-title"><a href="episodes/143/">Jason Long</a></h2>
This is the raw date:     <span class="post-date">Oct 25 | 2016</span>
This is the raw subtitle: <h3 class="topic-list">Open source | Empathy | Lower barriers | Learning tool | Design contributions | Git website | Branding | GitHub | Neovim | Tmux | Design love | Knowing audiences | Showing work | Dribbble | Progressions | Ideas</h3>
This is the raw link:     <a href="episodes/143/">Jason Long</a>

This is the text header: Jason Long
This is the text date:   Oct 25 | 2016
This is the text subtitle: Open source | Empathy | Lower barriers | Learning tool | Design contributions | Git website | Branding | GitHub | Neovim | Tmux | Design love | Knowing audiences | Showing work | Dribbble | Progressions | Ideas
This is the href:     http://betweenscreens.fm/episodes/143/

This is the raw header:   <h2 class="post-title"><a href="episodes/142/">David Heinemeier Hansson</a></h2>
This is the raw date:     <span class="post-date">Oct 18 | 2016</span>
This is the raw subtitle: <h3 class="topic-list">Rails community | Tone | Technical disagreements | Community policing | Ungratefulness | No assholes allowed | Basecamp | Open source persona | Aspirations | Guarding motivations | Dealing with audiences | Pressure | Honesty | Diverse opinions | Small talk</h3>
This is the raw link:     <a href="episodes/142/">David Heinemeier Hansson</a>

This is the text header: David Heinemeier Hansson
This is the text date:   Oct 18 | 2016
This is the text subtitle: Rails community | Tone | Technical disagreements | Community policing | Ungratefulness | No assholes allowed | Basecamp | Open source persona | Aspirations | Guarding motivations | Dealing with audiences | Pressure | Honesty | Diverse opinions | Small talk
This is the href:     http://betweenscreens.fm/episodes/142/

This is the raw header:   <h2 class="post-title"><a href="episodes/141/">Zach Holman</a></h2>
This is the raw date:     <span class="post-date">Oct 12 | 2016</span>
This is the raw subtitle: <h3 class="topic-list">Getting Fired | Taboo | Transparency | Different Perspectives | Timing | Growth Stages | Employment &amp; Dating | Managers | At-will Employment | Tech Industry | Europe | Low hanging Fruits | Performance Improvement Plans | Meeting Goals | Surprise Firings | Firing Fast | Mistakes | Company Culture | Communication</h3>
This is the raw link:     <a href="episodes/141/">Zach Holman</a>

This is the text header: Zach Holman
This is the text date:   Oct 12 | 2016
This is the text subtitle: Getting Fired | Taboo | Transparency | Different Perspectives | Timing | Growth Stages | Employment & Dating | Managers | At-will Employment | Tech Industry | Europe | Low hanging Fruits | Performance Improvement Plans | Meeting Goals | Surprise Firings | Firing Fast | Mistakes | Company Culture | Communication
This is the href:     http://betweenscreens.fm/episodes/141/

This is the raw header:   <h2 class="post-title"><a href="episodes/140/">Joel Glovier</a></h2>
This is the raw date:     <span class="post-date">Oct 10 | 2016</span>
This is the raw subtitle: <h3 class="topic-list">Digital Product Design | Product Design @ GitHub | Loving Design | Order &amp; Chaos | Drawing | Web Design | HospitalRun | Diversity | Startup Culture | Improving Lives | CURE International | Ember | Offline First | Hospital Information System | Designers &amp; Open Source</h3>
This is the raw link:     <a href="episodes/140/">Joel Glovier</a>

This is the text header: Joel Glovier
This is the text date:   Oct 10 | 2016
This is the text subtitle: Digital Product Design | Product Design @ GitHub | Loving Design | Order & Chaos | Drawing | Web Design | HospitalRun | Diversity | Startup Culture | Improving Lives | CURE International | Ember | Offline First | Hospital Information System | Designers & Open Source
This is the href:     http://betweenscreens.fm/episodes/140/

This is the raw header:   <h2 class="post-title"><a href="episodes/139/">João Ferreira</a></h2>
This is the raw date:     <span class="post-date">Aug 26 | 2015</span>
This is the raw subtitle: <h3 class="topic-list">Masters @ Work | Subvisual | Deadlines | Design personality | Design problems | Team | Pushing envelopes | Delightful experiences | Perfecting details | Company values</h3>
This is the raw link:     <a href="episodes/139/">João Ferreira</a>

This is the text header: João Ferreira
This is the text date:   Aug 26 | 2015
This is the text subtitle: Masters @ Work | Subvisual | Deadlines | Design personality | Design problems | Team | Pushing envelopes | Delightful experiences | Perfecting details | Company values
This is the href:     http://betweenscreens.fm/episodes/139/

This is the raw header:   <h2 class="post-title"><a href="episodes/138/">Corwin Harrell</a></h2>
This is the raw date:     <span class="post-date">Aug 06 | 2015</span>
This is the raw subtitle: <h3 class="topic-list">Q&amp;A | 01 | University | Graphic design | Design setup | Sublime | Atom | thoughtbot | Working location | Collaboration &amp; pairing | Vim advocates | Daily routine | Standups | Clients | Coffee walks | Investment Fridays |</h3>
This is the raw link:     <a href="episodes/138/">Corwin Harrell</a>

This is the text header: Corwin Harrell
This is the text date:   Aug 06 | 2015
This is the text subtitle: Q&A | 01 | University | Graphic design | Design setup | Sublime | Atom | thoughtbot | Working location | Collaboration & pairing | Vim advocates | Daily routine | Standups | Clients | Coffee walks | Investment Fridays |
This is the href:     http://betweenscreens.fm/episodes/138/

This is the raw header:   <h2 class="post-title"><a href="episodes/137/">Roberto Machado</a></h2>
This is the raw date:     <span class="post-date">Aug 03 | 2015</span>
This is the raw subtitle: <h3 class="topic-list">CEO @ Subvisual | RubyConf Portugal | Creators School | Consultancy | Company role models | Group Buddies | Portuguese startup | Rebranding | Technologies used | JS frameworks | TDD &amp; BDD | Startup mistakes | Culture of learning | Young entrepreneurs</h3>
This is the raw link:     <a href="episodes/137/">Roberto Machado</a>

This is the text header: Roberto Machado
This is the text date:   Aug 03 | 2015
This is the text subtitle: CEO @ Subvisual | RubyConf Portugal | Creators School | Consultancy | Company role models | Group Buddies | Portuguese startup | Rebranding | Technologies used | JS frameworks | TDD & BDD | Startup mistakes | Culture of learning | Young entrepreneurs
This is the href:     http://betweenscreens.fm/episodes/137/

```


Easy, right? You can do the same to extract the `[:class]` of a selector.

``` ruby

require 'nokogiri'
require "open-uri"

url = 'http://betweenscreens.fm/'

page = Nokogiri::HTML(open(url))

body_classes = page.at_css("body")[:class]

```

If that node has more than one class, you will get a list of all of them.

# Node Navigation

+ parent
+ children
+ previous_sibling
+ next_sibling

We are used to navigating these tree structures in CSS or even jQuery. It would be a pain if Nokogiri wouldn’t offer a handy API to move within such trees.


#### **some_scraper.rb**

``` ruby

require 'nokogiri'
require "open-uri"

url = 'http://betweenscreens.fm/'

page = Nokogiri::HTML(open(url))

header = page.at_css("h2.post-title")
header_children = page.at_css("h2.post-title").children
header_parent = page.at_css("h2.post-title").parent
header_prev_sibling = page.at_css("h2.post-title").previous_sibling

puts "#{header}\n\n"
puts "#{header_children}\n\n"
puts "#{header_parent}\n\n"
puts "#{header_prev_sibling}\n\n"

```

#### Output

``` bash

#header
<h2 class="post-title"><a href="episodes/143/">Jason Long</a></h2>

#header_children
<a href="episodes/143/">Jason Long</a>

#header_parent
<article class="index-article">
  <span class="post-date">Oct 25 | 2016</span><h2 class="post-title"><a href="episodes/143/">Jason Long</a></h2>

    <h3 class="topic-list">Open source | Empathy | Lower barriers | Learning tool | Design contributions | Git website | Branding | GitHub | Neovim | Tmux | Design love | Knowing audiences | Showing work | Dribbble | Progressions | Ideas</h3>

    <div class="soundcloud-player-small">  
        <iframe width="100%" height="166" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/290126141&amp;color=ff0000&amp;auto_play=false&amp;hide_related=true&amp;show_comments=false&amp;show_user=true&amp;show_reposts=false"></iframe>
    </div>
</article>

#header_previous_sibling
<span class="post-date">Oct 25 | 2016</span>

```

As you can see for yourself, this is some pretty powerful stuff—especially when you look what `.parent` was able to collect in one go. Instead of defining a bunch of nodes by hand, you could collect them wholesale style. You can even chain them for more involved traversals. You can take this as complicated as you like of course but I would caution you to keep things simple. It can quickly get a little unwieldy and hard to understand. Remember, "Keep it simple, stupid"!

``` ruby

...

header_parent_parent = page.at_css("h2.post-title").parent.parent
header_prev_sibling_parent_children = page.at_css("h2.post-title").previous_sibling.parent.children

...

```

#### **some_scraper.rb**

``` ruby

require 'nokogiri'
require "open-uri"

url = 'http://betweenscreens.fm/'

page = Nokogiri::HTML(open(url))

header = page.at_css("h2.post-title")
header_prev_sibling_children = page.at_css("h2.post-title").previous_sibling.children
header_parent_parent = page.at_css("h2.post-title").parent.parent
header_prev_sibling_parent = page.at_css("h2.post-title").previous_sibling.parent
header_prev_sibling_parent_children = page.at_css("h2.post-title").previous_sibling.parent.children

puts "#{header}\n\n"
puts "#{header_prev_sibling_children}\n\n"
puts "#{header_parent_parent}\n\n"
puts "#{header_prev_sibling_parent}\n\n"
puts "#{header_prev_sibling_parent_children}\n\n"

```

#### **Output**

``` bash
#header
<h2 class="post-title"><a href="episodes/143/">Jason Long</a></h2>

#header_previous_sibling_children
Oct 25 | 2016

#header_parent_parent
<li>
  <article class="index-article">
  <span class="post-date">Oct 25 | 2016</span><h2 class="post-title"><a href="episodes/143/">Jason Long</a></h2>

    <h3 class="topic-list">Open source | Empathy | Lower barriers | Learning tool | Design contributions | Git website | Branding | GitHub | Neovim | Tmux | Design love | Knowing audiences | Showing work | Dribbble | Progressions | Ideas</h3>

    <div class="soundcloud-player-small">  
        <iframe width="100%" height="166" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/290126141&amp;color=ff0000&amp;auto_play=false&amp;hide_related=true&amp;show_comments=false&amp;show_user=true&amp;show_reposts=false"></iframe>
    </div>
  </article>
</li>

#header_previous_sibling_children
<article class="index-article">
  <span class="post-date">Oct 25 | 2016</span><h2 class="post-title"><a href="episodes/143/">Jason Long</a></h2>

    <h3 class="topic-list">Open source | Empathy | Lower barriers | Learning tool | Design contributions | Git website | Branding | GitHub | Neovim | Tmux | Design love | Knowing audiences | Showing work | Dribbble | Progressions | Ideas</h3>

    <div class="soundcloud-player-small">  
        <iframe width="100%" height="166" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/290126141&amp;color=ff0000&amp;auto_play=false&amp;hide_related=true&amp;show_comments=false&amp;show_user=true&amp;show_reposts=false"></iframe>
    </div>
</article>

#header_previous_sibling_parent_children

  <span class="post-date">Oct 25 | 2016</span><h2 class="post-title"><a href="episodes/143/">Jason Long</a></h2>

    <h3 class="topic-list">Open source | Empathy | Lower barriers | Learning tool | Design contributions | Git website | Branding | GitHub | Neovim | Tmux | Design love | Knowing audiences | Showing work | Dribbble | Progressions | Ideas</h3>

    <div class="soundcloud-player-small">  
        <iframe width="100%" height="166" scrolling="no" frameborder="no" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/290126141&amp;color=ff0000&amp;auto_play=false&amp;hide_related=true&amp;show_comments=false&amp;show_user=true&amp;show_reposts=false"></iframe>
    </div>

```

# Command Line Scraping
