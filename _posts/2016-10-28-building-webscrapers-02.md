---
layout: post
title: Building Your First Webscraper 02
date: 2016-10-28 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Mechanic, Nokogiri, Ruby]
---

# Topics

+ Single Page vs Pagination
+ Mechanize
+ Agent
+ Links


# Single Page vs Pagination

So far we have spent some time figuring out how we can scrape the screen of a single page using Nokogiri. This was a good basis to move one step forward and learn how to extract content from multiple pages. After all, the problem we try to solve involves getting the content from more than 140 episodes—which is more content that can reasonably fit a single web page. We have to work with pagination and need to figure out how to follow content down the rabbit hole. This is where Nokogiri stops and another useful gem called [Mechanize](https://github.com/sparklemotion/mechanize) comes into play

# Mechanize

Mechanize is another powerful tool that has lots of goodies to offer. It is essentially enabling you to automate interactions with websites you need to extract content from. In that sense it reminds me a bit of some functionality that you might know from testing with [Capybara](http://jnicklas.com/capybara/). Don’t get me wrong, playing with Nokogiri on a single page is awesome in itself, but for more spicy data extraction jobs, we need a bit more horsepower. We can essentially crawl through as many pages and we need and interact with its elements—imitating and automating human behavior. Pretty powerful stuff!

This gem enables you to follow links, submit forms, populating form fields and submitting that data—even dealing with cookies is on the table. That means you can also imitate users login into private sessions and get content from a site only you have access to. You fill out the login with your credentials and tell Mechanize how to follow along. Since you can click links and submit forms, there is very little that you cannot do with this tool. It has a close relationship to Nokogiri and also depends on it. [Aaron Patterson](https://twitter.com/tenderlove?lang=de) is again one of the authors of this lovely gem.
 
# Agent

Before we can start mechanizing things, we need to instantiate a mechanize agent.

#### **some_scraper.rb**

``` ruby

require 'mechanize'

agent = Mechanize.new

```

This `agent` will be used to fetch a page, similar to what we did with Nokogiri.

#### **some_scraper.rb**

``` ruby

require 'mechanize'

agent = Mechanize.new

podcast_url = "http://betweenscreens.fm/"

page = agent.get(podcast_url)

```

What happens here is that the mechanize agent got the podcast page and its cookies. We now have a page that is ready for extraction.

# Links

+ links
+ link_with
+ links_with

We can also already navigate the whole page to our liking. Probably the most important part of mechanize is its ability to let you play with links. Otherwise you could pretty much stick with Nokogiri on its own. Let’s take a look what we get returned if we ask a page for its links.

#### **some_scraper.rb**

``` ruby

require 'mechanize'

agent = Mechanize.new

podcast_url = "http://betweenscreens.fm/"

page = agent.get(podcast_url)

puts "#{page.links}"

```

#### **Output**

``` bash

[#<Mechanize::Page::Link "Logo cube" "/">
, #<Mechanize::Page::Link "fork!" "https://github.com/vis-kid/betweenscreens">
, #<Mechanize::Page::Link "about" "pages/about/">
, #<Mechanize::Page::Link "design" "design/">
, #<Mechanize::Page::Link "code" "code/">
, #<Mechanize::Page::Link "Randy J. Hunt" "episodes/144/">
, #<Mechanize::Page::Link "Jason Long" "episodes/143/">
, #<Mechanize::Page::Link "David Heinemeier Hansson" "episodes/142/">
, #<Mechanize::Page::Link "Zach Holman" "episodes/141/">
, #<Mechanize::Page::Link "Joel Glovier" "episodes/140/">
, #<Mechanize::Page::Link "João Ferreira" "episodes/139/">
, #<Mechanize::Page::Link "Corwin Harrell" "episodes/138/">
, #<Mechanize::Page::Link "Older Stuff »" "page/2/">
, #<Mechanize::Page::Link "Exercise" "/tags/exercise/">
, #<Mechanize::Page::Link "Company benefits" "/tags/company-benefits/">
, #<Mechanize::Page::Link "Tmux" "/tags/tmux/">
, #<Mechanize::Page::Link "FileTask" "/tags/filetask/">
, #<Mechanize::Page::Link "Decision making" "/tags/decision-making/">
, #<Mechanize::Page::Link "Favorite feature" "/tags/favorite-feature/">
, #<Mechanize::Page::Link "Working out" "/tags/working-out/">
, #<Mechanize::Page::Link "Scott Savarie" "/tags/scott-savarie/">
, #<Mechanize::Page::Link "Titles" "/tags/titles/">
, #<Mechanize::Page::Link "Erik Spiekermann" "/tags/erik-spiekermann/">
, #<Mechanize::Page::Link "Newbie mistakes" "/tags/newbie-mistakes/">
, #<Mechanize::Page::Link "Playbook" "/tags/playbook/">
, #<Mechanize::Page::Link "Delegation" "/tags/delegation/">
, #<Mechanize::Page::Link "Heat maps" "/tags/heat-maps/">
, #<Mechanize::Page::Link "Europe" "/tags/europe/">
, #<Mechanize::Page::Link "Sizing type" "/tags/sizing-type/">
, #<Mechanize::Page::Link "Focus" "/tags/focus/">
, #<Mechanize::Page::Link "Virtual assistants" "/tags/virtual-assistants/">
, #<Mechanize::Page::Link "Writing" "/tags/writing/">
, #<Mechanize::Page::Link "Hacking" "/tags/hacking/">
, #<Mechanize::Page::Link "Joel Glovier" "/tags/joel-glovier/">
, #<Mechanize::Page::Link "Corwin Harrell" "/tags/corwin-harrell/">
, #<Mechanize::Page::Link "Mario C. Delgado" "/tags/mario-c-delgado/">
, #<Mechanize::Page::Link "Tom Dale" "/tags/tom-dale/">
, #<Mechanize::Page::Link "Obie Fernandez" "/tags/obie-fernandez/">
, #<Mechanize::Page::Link "Chad Pytel" "/tags/chad-pytel/">
, #<Mechanize::Page::Link "Zach Holman" "/tags/zach-holman/">
, #<Mechanize::Page::Link "Max Luster" "/tags/max-luster/">
, #<Mechanize::Page::Link "Kyle Fiedler" "/tags/kyle-fiedler/">
, #<Mechanize::Page::Link "Roberto Machado" "/tags/roberto-machado/">
]

```

Holy moly, let’s break this down. Since we haven’t told mechanize to look elsewhere, we got an array of links from that very first page. Mechanize goes through that page in descending order and returns you this list of links from top to bottom. I have created a little image with green pointers to the various links that you can see in the output. Btw, this is already showing you the end result of the redesign for my podcast. I think this version is a bit better for demonstration purposes. You also get a glimpse how the final result looks like and why I needed to scrape my old Sinatra site.

#### **Screenshot**

![Alt text](/images/webscraper/mechanize-links1.png)

As always, we can also extract just the text from that.

#### **some_scraper.rb**

``` ruby

require 'mechanize'

agent = Mechanize.new

podcast_url = "http://betweenscreens.fm/"

page = agent.get(podcast_url)

page.links.each do |link|
  puts link.text
end

```

#### **Output**

``` bash

Logo cube
fork!
about
design
code
Randy J. Hunt
Jason Long
David Heinemeier Hansson
Zach Holman
Joel Glovier
João Ferreira
Corwin Harrell
Older Stuff »
Exercise
Company benefits
Tmux
FileTask
Decision making
Favorite feature
Working out
Scott Savarie
Titles
Erik Spiekermann
Newbie mistakes
Playbook
Delegation
Heat maps
Europe
Sizing type
Focus
Virtual assistants
Writing
Hacking
Joel Glovier
Corwin Harrell
Mario C. Delgado
Tom Dale
Obie Fernandez
Chad Pytel
Zach Holman
Max Luster
Kyle Fiedler
Roberto Machado

```

Getting all these links in bulk can be tedious and useless. Lucky for us, we have a few tools in place to fine tune what we need.

#### **some_scraper.rb**

``` ruby

require 'mechanize'

agent = Mechanize.new

podcast_url = "http://betweenscreens.fm/"

page = agent.get(podcast_url)

page = agent.page.links.find { |l| l.text == 'Focus' }

puts page

```

#### **Output**

``` bash

Focus

```

Boom! Now we are getting somwhere! What about following that fella and see what hides behind this link. Let’s `click` it!

#### **some_scraper.rb**

``` ruby

require 'mechanize'

agent = Mechanize.new

podcast_url = "http://betweenscreens.fm/"

page = agent.get(podcast_url)

page = agent.page.links.find { |l| l.text == 'Focus' }.click.links

puts page

```

This would get us another list of links like before. See how easy it was to combine `.click.links`.

If we would have had multiple `Focus` links, we could have zoomed in on a particular number on the page.

#### **some_scraper.rb**

``` ruby

require 'mechanize'

agent = Mechanize.new

podcast_url = "http://betweenscreens.fm/"

page = agent.get(podcast_url)

page = agent.page.links.find { |l| l.text == 'Focus'}

puts page

```

