---
layout: post
title: SOLID Ruby Fundamentals 01-SRP
date: 2016-11-10 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Mechanic, Nokogiri, Ruby]
---

# Topics

+ SOLID?
+ SRP: Single Responsibility Principle

# SOLID?

SOLID is an acronym for five design principles in object oriented design in programming.

+ SRP, or the “Single Responsibility Principle”.
+ Open/Closed Principle.
+ Liskov Substitution Principle.
+ Interface Segregation Principle.
+ Dependency Inversion Principle.

A lot of principles, I hear ya! Don’t let you discourage by their academic sex appeal. You are new to this and are maybe still dealing with a lot of basic programming concepts. The last thing you wanna stuff your brain with are coding principles. Fair point! I would say though that you are maybe missing out on an opportunity to step up your game from the very beginning.

minimal entanglements

It is not uncommon to think that you need to wait until you are ready, until you are more advanced to jump into more advanced waters. And you may be right in some regards. These design principles are different though. On the one hand, I feel like they are beginner-friendly to grasp and on the other hand, they will improve the quality of your code from day one. They will help you to design your code the way professionals do—at least you will start looking at your code as the big girls do. What’s not to like?

If I would attempt to sum up the SOLID principles in one sentence, I would probably fail—miserably. What question do these principles address? This might be easier to answer. They were a result of thinking about how we can better manage dependencies. How can we prevent to create fast-rotting software? These are questions that are of equal importance for beginners and pros alike.

Are these principles appropriate for a dynamic language like Ruby? After all, when SOLID was formed, it was in an era of statically typed Java and C++ programming. Good question! Yes, static languages have stronger dependencies but there is no reason to throw these principles out the window when you write Ruby code. You will learn basic techniques how you can modularize and stabilize your code.

Stabilize? Yes, in the sense that you will be netter equipped to write code that is less brittle and better prepared for the inevitable change down the line. The ease with which you are able to change a code base is in direct correlation to its quality. I recommend that you start early in your developer career to think about this by starting to give these OO design principles an early look.

# SRP: Single Responsibility Principle

> A class should have only one reason  to change

(https://www.flickr.com/photos/redjar/113974357)

![Alt text](/images/SOLID/swiss-army-knife.jpg)

In short, it is the opposite of spaghetti code I’d say. You decrease your chances of creating unwanted entanglements and hanging your app with code that has too many intermingled responsibilities.

If you would follow only once principle in your coding journey, SRP would be a good candidate. It brings a lot of benefits to the table, at literally little to no cost while encouraging other development in your code that is good. As a byproduct almost.

That means your classes have only one responsibility. If this class is focused on only one job, there can be another reason to change it.

## Clarity

Optimized for readablity

Big classes are very often a smell that they are not following SRP.

You want to avoid classes that have too much going on. If they start to feel blobby and you need to take some time to figure out what’s actually going on, it’s probably a good time to refactor its responsibilities. 

## Reuseability

Ideally, you want your classes to handle one responsibility well so that you can reuse that functionality in other places as well.

A good guideline is to build small components because they increase the chance of being useful to be recycled some place else in your code.

Keeping things isolated, in the own little scope

## Easier to Test

When you don’t have classes that are too involved and frankly all over the place, you can write much more focused and readable tests as well. Your tests are probably a good indicator of your classes quality of state. They run faster as well because of reduced dependencies.

But not only are such tests slower if classes don’t follow SRP, they will also be more brittle along they way. Your classes and your tests will change, this is the one certainty you have. The less responsibilities you encapsulate in classes, the less brittle your code will end up being.

Look out for tests that are hard to set up and involve a lot of dependencies. That’s a good indicator that you should spend some time on simplifying the class design.

## Easy to Change

Classes with tons of responsibilities are a pain to change. Since this is expected future behaviour of your application, you should plan for it. SRP classes are more flexible to change. They are easy to refactor or to completely rewrite if your application goes through growing pains for example.

Flexibility and resiliency are two important qualities you want to have from every piece of your code. The Single Responsibility Principle hands you these benefits on a silver platter without much effort. Rewriting you work becomes less and less of a liability—or even of a necessity when reuse and modularity are built in from the start.

## Code Quality

For senior software writers, the ease of being able to change code when the requirements necessitate that, is one of their highest metric to assess code quality.

## Downloading Context

If the classes are smaller, you will give other team members and your future self an easier time to get familiarized with its behaviour as well. Nobody wan’t to open a class and sit there for 15 minutes before they have a general sense of what’s going on. That will make people—you guessed it—put funny stuff in your coffee at some point.

Also, what developers need to load into their heads in this context, is often what you will end up needing to load as dependencies in your tests. Not exactly ideal in many ways..
## Reasons to Change

Reasons to change roughly equal responsibilities. If you have a class with five different jobs, you have a good chance of ending up with five reasons to change. You class sets a session, sends email that also get validated and does the weather forecast for the next week?

If any of these things change, the rest of the class is possibly affected by that. This is exactly what SRP wants us to avoid. This is the brittleness we talk about.

## Cohesive Classes

Classes should be highly cohesive. You are setting up the dynamite to blow up the house you are living in yourself. O.K., let’s not overdramatize these things, I was just trying to find a strong metaphor that sticks with you. Nobody is gonna loose his life or anything, but some people might loose their minds if these things pile up for years and then something happens. Entangling that large piece of spaghetti code could literally break a product if people loose the ability to rebuild the application with the components at hand.

Every piece of a class should be highly related to other pieces of a class. In our imaginary example from above, the weather forecast is not highly related to email validation, sending email or creating a session. So we end up with weak cohesion which basically let’s your app rot from the inside. You are setting up the dynamite to blow up the house you are living in yourself.

O.K., let’s not overdramatize these things, I was just trying to find a strong metaphor that sticks with you. Nobody is gonna loose his life or anything, but some people might loose their minds if these things pile up for years and then something happens. Entangling that large piece of spaghetti code could literally break a product if people loose the ability to rebuild the application with the components at hand.

So we want to have a class that has everything to do with forecasting the weather, one that is in charge of sending email, one that creates a session and one that handles the responsibility of calculating the weather forecast. Maybe even two for that. One that handles the weather forecast for a week and another who is in charge of the next 24 hours.

In Rails, stuff that people often put in ApplicatioController or ApplicationHelper are often not very cohesive by nature. It’s a top-level object that often collects a bunch of inconvenient stuff. You will often see applications where it acts as a dumpster of stuff that the lazy fix didn’t compose a better home for. This can quickly get out of hand and pile on stuff from all over the place. Just because it’s easy to use these compartments for “global” stuff, doesn’t meant that we should stop there. Also, even as beginners, we already know that global stuff is rarely in our best self-interest.

## Boundries

It’s best to evolve your code to become SRP friendly. Often you don’t know from the beginning how the responsibilities are spread and you should not over-optimize from the start without knowing where the journey is headed.

Some classes will end up being magnets for responsibilities while others are like a lot more lazy in piling them on.

## Gem churn

It will show you how often particular classes have been changed. These statistics can help you to identify which classes might be in need of an SRP treatment. Your god classes and the ones that cover the central business proposition will often end up being high on that list.

When these classes change often it tells you that they have many reasons to change.

This gives you an easy peek into your core models and updates you that the rate of change might be alarming or not.

## Bugs

Intermingling and churn is an open door for bugs. It’s almost like inviting cock roaches into your home by laying out food at your doorstep.

## God Classes and User

Always think hard before you add something to the User model. It is such an attractive magnet for all sorts of things in most applications.

## One ... bullet

You will often have to weigh different principles against each other. SRP is no golden bullet that fits every occasion. It is often a very good starting point though. Figuring out a balanced middle ground will always be part of your job when you design applications. That also expands to applying code quality principles.

## How do we recognize good design?



Dependencies tell you how change is gonna propagate throughout your code.

Many dependencies create a sort of pressure to not change things. Worse if yo don’t know what the extend is that a little change here or there  can cause.

## And / Or

If you need “and” or “or” in the description, the purpose of the functionality in a particular class, you can start thinking about applying SRP. Both scenarios imply that it does more than one focused thing. 

As mentioned above, nothing is set in stone, there is no one truth fits all scenario, but at least it should raise the question if it might be a good idea to refactor your code to follow only one responsibility.

## But it is Hard

I give you that. In the beginning it will be relatively easy to break models into focused classes that do one thing. That will be your foundation to move forward into more complex waters. Take the time to build this base with smarts. Once complexity kicks in, it becomes a lot harder to break objects apart. I mean, there is no way around it. It will happen anyway, but with a good foundation, not only buildings get to be built taller and more stable. It will get harder over time, that is probably for sure. But at the same time, the problems also get more interesting, more challenging. It is a fun process that you might actually enjoy.

## Benefits

Breaking classes into focused components have a few benefits that can easily go unnoticed among the upside mentioned above. A few benefits that I can see are the following: 

+ Method names can often stay simpler due to fewer dependencies referenced.
+ Better organization because you grouped functionality more coherently.
+ Namespace clashes are easier to avoid.
+ Classes are easier to read and digest.
+ Same might apply for variable names.
+ Simpler, more readable APIs.
+ Smaller objects.
+ Faster objects.

## Changes

The question about inevitable change is, how would you like the ride to be? Crazy as hell or fun and interesting. Do you wanna feel like the world is doomed or just another day at the office with an interesting problem. The design of your application will strongly influence the experience of change. If you have an unflexible, rigid system in place, you can count on creating a cascade of related changes that pop up during that process. Fragile systems are not your friend.

We don’t want application where everything is connected to everything else. That is a nightmare for anybody to work with. It’s basically a fancier way of saying it’s “Spaghetti code”. That does not sound like a four star 20 course menu, does it? You don’t want to build incredible long domino lines that all fall by tripping over a single piece.

https://commons.wikimedia.org/wiki/File:Toppledominos.jpg

![Alt text](/images/SOLID/Toppledominos.jpg)



https://www.flickr.com/photos/baccharus/5817342671

![Alt text](/images/SOLID/cable-mess.jpg)

image. hammer arcade game

## Payoff

Good design will pay off. As with having a test suite around. It might cost you a little bit of extra effort in the beginning, but you will be glad going forward to have built on solid ground. Once you start to feel the need to fight your app all the time, you will wish back the good times where the application was young and perfect where you had an easy opportunity to apply solid design.

Good design takes time in the beginning, as does TDD. But you can count on being better prepared for changing conditions down the line. Actually, it will most likely cost you extra money to not pay attention to design in your application. Cleaning up spaghetti code takes time and costs big bucks.

## SRP Symtoms

When your code is

+ loosely coupled
+ highly cohesive
+ easily composeable
+ context independent


It keeps your classes and methods small. That directly leads to better maintainability

Coupling is dependency

Managing dependencies is a huge chunk of your actual work as a software writer.

## What should I do?

As a consequence of applying SRP, you will very often extract classes. If you have a bigger class with many responsibilities, you will end up needing more classes that have smaller, more focused jobs.

# Extract Class




You don’t go fast by writing bad code. That’s for sure. Chances are pretty good that the opposite is adequate.

Rigidity in your code means that you need to modify a bunch of seemingly unrelated stuff when you touch one thing. You can only create a new state of consistency once the other dpendencies are taken care of. There are various degrees of course, but the treashold, the line to cross is not that big.

You cannot make an isolated change without changing everything around it. Bad dependencies. 

Fragility. Breaking things in many places that seem unrelated to the one place that actually changed. like butterfly effect.

Don’t put functions that change for different reasons into the same class. That means you can organize you classes around functions that change for the same reason.

# Scraper Example

In my last article about scraping websites, I built a small scraper to get data off my podcast [Between \| Screens](http://betweenscreens.fm/). I left the readers with a small exercise to play with. Since it was another article that was targeted at coding newbies, I wanted the readers to take the code and play with it. The exercise was to find ways to refactor that piece of code once they wrapped their heads around it. 

In the article I focused solely on the functionality, the API and how readers can use [Nokogiri](http://www.nokogiri.org/) / [Mechanize](https://github.com/sparklemotion/mechanize) to extract data from websites. I thought I don’t want to muddy the waters with code smells and refactorings in that context. After all, I expect most of my readers to be freshly hatched coders. Confusing newbies with various different concepts at the same time was something I disliked starting out.

I gave a little hint that extracting classes is a good way to start getting this code in better shape. Let’s use this code and see how we can encapsulate it into objects that focus on simple responsibilities. If the example feels a little dense, I recommend that you go back to at least the third article in the series. You will find detailed explanations for every method this webscraper is executing. 

## Raw Podcast Scraper

``` ruby

require 'Mechanize'
require 'Pry'
require 'date'

# Helper Methods

# (Extraction Methods)

def extract_interviewee(detail_page)
  interviewee_selector = '.episode_sub_title span'
  detail_page.search(interviewee_selector).text.strip
end

def extract_title(detail_page)
  title_selector = ".episode_title"
  detail_page.search(title_selector).text.gsub(/[?#]/, '')
end

def extract_soundcloud_id(detail_page)
  sc = detail_page.iframes_with(href: /soundcloud.com/).to_s
  sc.scan(/\d{3,}/).first
end

def extract_shownotes_text(detail_page)
  shownote_selector = "#shownote_container > p"
  detail_page.search(shownote_selector)
end

def extract_subtitle(detail_page)
  subheader_selector = ".episode_sub_title"
  detail_page.search(subheader_selector).text
end

def extract_episode_number(episode_subtitle)
  number = /[#]\d*/.match(episode_subtitle)
  clean_episode_number(number)
end

# (Utility Methods)

def clean_date(episode_subtitle)
  string_date = /[^|]*([,])(.....)/.match(episode_subtitle).to_s
  Date.parse(string_date)
end

def build_tags(title, interviewee)
  extracted_tags = strip_pipes(title)
  "#{interviewee}"+ ", #{extracted_tags}"
end

def strip_pipes(text)
  tags = text.tr('|', ',')
  tags = tags.gsub(/[@?#&]/, '')
  tags.gsub(/[w\/]{2}/, 'with')
end

def clean_episode_number(number)
  number.to_s.tr('#', '')
end

def dasherize(text)
  text.lstrip.rstrip.tr(' ', '-')
end

def extract_data(detail_page)
  interviewee = extract_interviewee(detail_page)
  title = extract_title(detail_page)
  sc_id = extract_soundcloud_id(detail_page)
  text = extract_shownotes_text(detail_page)
  episode_subtitle = extract_subtitle(detail_page)
  episode_number = extract_episode_number(episode_subtitle)
  date = clean_date(episode_subtitle)
  tags = build_tags(title, interviewee)

  options = {
    interviewee:    interviewee,
    title:          title,
    sc_id:          sc_id,
    text:           text,
    tags:           tags,
    date:           date,
    episode_number: episode_number
  }
end

def compose_markdown(options={})
<<-HEREDOC
--- 
title: #{options[:interviewee]}
interviewee: #{options[:interviewee]}
topic_list: #{options[:title]}
tags: #{options[:tags]}
soundcloud_id: #{options[:sc_id]}
date: #{options[:date]}
episode_number: #{options[:episode_number]}
---

#{options[:text]}
HEREDOC
end

def write_page(link)
  detail_page = link.click
  extracted_data = extract_data(detail_page)

  markdown_text = compose_markdown(extracted_data)
  date = extracted_data[:date]
  interviewee = extracted_data[:interviewee]
  episode_number = extracted_data[:episode_number]

  File.open("#{date}-#{dasherize(interviewee)}-#{episode_number}.html.erb.md", 'w') { |file| file.write(markdown_text) }
end

def scrape
  link_range = 1
  agent ||= Mechanize.new

  until link_range == 21
    page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")
    link_range += 1

    page.links[2..8].map do |link|
      write_page(link)
    end
  end
end

scrape

```

Now that is one blob of code. I hear you. I wouldn’t say it’s spaghetti, but it certainly is a good opportunity to play with SRP. I hope the readers of the last article gave the exercise some thought and came up with a few refactorings that simplified things or improved readability. The code above works as is, but only focuses on functionality. It does not comply with the things we discussed above. Cohesion and encapsulation is not strongly represented. In short, this does not scale well. For an article about Nokogiri and Mechanize for beginners or a first spike, this is alright, but we can and should do better. Let’s have a look how extracting a few classes can make a difference. 

## Podcast Scraper with Extracted Classes v.1

``` ruby

require 'Mechanize'
require 'Pry'
require 'date'

module ExtractionUtilities

  def build_tags(title, interviewee)
    extracted_tags = strip_pipes(title)
    "#{interviewee}"+ ", #{extracted_tags}"
  end

  def strip_pipes(title)
    tags = title.tr('|', ',')
    tags = tags.gsub(/[@?#&]/, '')
    tags.gsub(/[w\/]{2}/, 'with')
  end

  def clean_episode_number(number)
    number.to_s.tr('#', '')
  end

  def clean_date(episode_subtitle)
    string_date = /[^|]*([,])(.....)/.match(episode_subtitle).to_s
    Date.parse(string_date)
  end

  def dasherize(text)
    text.lstrip.rstrip.tr(' ', '-')
  end
end

class PageExtractor
  attr_reader :detail_page
  include ExtractionUtilities

  def initialize(detail_page_link)
    @detail_page = detail_page_link.click
  end

  def extract_data
    options = {
      interviewee:    interviewee,
      title:          title,
      sc_id:          soundcloud_id,
      text:           shownotes_text,
      tags:           tags,
      date:           date,
      episode_number: episode_number
    }
  end

  private

  def interviewee
    interviewee_selector = '.episode_sub_title span'
    detail_page.search(interviewee_selector).text.strip
  end

  def title
    title_selector = ".episode_title"
    detail_page.search(title_selector).text.gsub(/[?#]/, '')
  end

  def soundcloud_id
    sc = detail_page.iframes_with(href: /soundcloud.com/).to_s
    sc.scan(/\d{3,}/).first
  end

  def shownotes_text
    shownote_selector = "#shownote_container > p"
    detail_page.search(shownote_selector)
  end

  def tags
    build_tags(title, interviewee)
  end

  def date
    clean_date(subtitle)
  end

  def episode_number
    number = /[#]\d*/.match(subtitle)
    clean_episode_number(number)
  end

  def subtitle
    subheader_selector = ".episode_sub_title"
    detail_page.search(subheader_selector).text
  end
end

class MarkdownComposer

  def compose_markdown(options={})
  <<-HEREDOC
  --- 
  title: #{options[:interviewee]}
  interviewee: #{options[:interviewee]}
  topic_list: #{options[:title]}
  tags: #{options[:tags]}
  soundcloud_id: #{options[:sc_id]}
  date: #{options[:date]}
  episode_number: #{options[:episode_number]}
  ---

  #{options[:text]}
  HEREDOC
  end
end

class PageWriter

  attr_reader :detail_page_link
  include ExtractionUtilities

  def initialize(detail_page_link)
    @detail_page_link = detail_page_link
  end

  def write_page
    markdown_text = prepare_markdown
    file_name     = prepare_filename

    File.open(file_name, 'w') { |file| file.write(markdown_text) }
  end

  private

  def prepare_markdown
    markdown_text = compose_markdown(extract_data)
  end

  def prepare_filename
    date =           extract_data[:date]
    interviewee =    extract_data[:interviewee]
    episode_number = extract_data[:episode_number]
    file_name =      compose_filename(date, interviewee, episode_number)
  end

  def extract_data
    extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  def compose_markdown(extracted_data)
    MarkdownComposer.new.compose_markdown(extracted_data)
  end

  def compose_filename(date, interviewee, episode_number)
    "#{date}-#{dasherize(interviewee)}-#{episode_number}.html.erb.md" 
  end

end

class Scraper

  def scrape
    link_range = 1
    agent ||= Mechanize.new

    until link_range == 21
      page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")
      link_range += 1

      page.links[2..8].map do |link|
        PageWriter.new(link).write_page
      end
    end
  end
end

Scraper.new.scrape

```

We now have four classes and one module that handles a few utilities.

+ `PageExtractor`
+ `MarkdownComposer`
+ `PageWriter`
+ `Scraper`
+ `ExtractionUtilities`

Let’s go over each of them and check their responsibilities. If you would like to see a more detailed explanation about their functionality, please check out my previous article where I go more into the nitty gritty details.

## module ExtractionUtilities

We could have extracted some more but for now I felt it would be overkill to split the module up. If I would amass more methods like dasherize, I would start thinking to find a better home for them. Since I need these utilities in more than one place I chose a module for now and include these methods where needed. I felt the `class PageExtractor` shouldn’t be concerned about stuff like striping pipe characters or building tags from the extracted data. That way we also increase the likelihood that we can reuse them.

#### module ExtractionUtilities

``` ruby

module ExtractionUtilities

  def build_tags(title, interviewee)
    extracted_tags = strip_pipes(title)
    "#{interviewee}"+ ", #{extracted_tags}"
  end

  def strip_pipes(title)
    tags = title.tr('|', ',')
    tags = tags.gsub(/[@?#&]/, '')
    tags.gsub(/[w\/]{2}/, 'with')
  end

  def clean_episode_number(number)
    number.to_s.tr('#', '')
  end

  def clean_date(episode_subtitle)
    string_date = /[^|]*([,])(.....)/.match(episode_subtitle).to_s
    Date.parse(string_date)
  end

  def dasherize(text)
    text.lstrip.rstrip.tr(' ', '-')
  end
end

```

## class PageExtractor

This class now has one responsibility, extracting the data from the link it was fed in the loop that visits every podcast episode. It returns an options hash that stores the data we needed from the shownotes site. It has a few private methods that are cohesive enough to not extract into another object on their own.

+ `interviewee`
+ `title`
+ `soundcloud_id`
+ `shownotes_text`
+ `tags`
+ `date`
+ `episode_number`
+ `subtitle`

They are made private to not mess with the API of this class while providing the convenience of looking up the implementation of scraping various selectors—without needing to go to another class or module. I felt they belong better into this class since they are in charge of actually scraping the data that we export in the options hash.

The class includes a module that takes care of smaller utility functions like `clean_episode_number`, `clean_date` and ` build_tags`. As always, trade offs are part of the game. I had to weigh readability and cohesiveness when I decided to put these methods into a module. It might now be immediately clear where they come from, but on the other hand, their implementation might not be needed to be investigated most of the time. We saved some space in the private section of this class and put them into a module that fits their purpose better.

#### class PageExtractor

``` ruby

class PageExtractor
  attr_reader :detail_page
  include ExtractionUtilities

  def initialize(detail_page_link)
    @detail_page = detail_page_link.click
  end

  def extract_data
    options = {
      interviewee:    interviewee,
      title:          title,
      sc_id:          soundcloud_id,
      text:           shownotes_text,
      tags:           tags,
      date:           date,
      episode_number: episode_number
    }
  end

  private

  def interviewee
    interviewee_selector = '.episode_sub_title span'
    detail_page.search(interviewee_selector).text.strip
  end

  def title
    title_selector = ".episode_title"
    detail_page.search(title_selector).text.gsub(/[?#]/, '')
  end

  def soundcloud_id
    sc = detail_page.iframes_with(href: /soundcloud.com/).to_s
    sc.scan(/\d{3,}/).first
  end

  def shownotes_text
    shownote_selector = "#shownote_container > p"
    detail_page.search(shownote_selector)
  end

  def tags
    build_tags(title, interviewee)
  end

  def date
    clean_date(subtitle)
  end

  def episode_number
    number = /[#]\d*/.match(subtitle)
    clean_episode_number(number)
  end

  def subtitle
    subheader_selector = ".episode_sub_title"
    detail_page.search(subheader_selector).text
  end
end

```

## class MarkdownComposer

The next class is even smaller. It takes the extracted info from the options hash and fills them into a blueprint for a `.markdown` file with [front matter](https://jekyllrb.com/docs/frontmatter/). It has one simple job and therefore only one reason to change.

#### class MarkdownComposer

``` ruby

class MarkdownComposer

  def compose_markdown(options={})
  <<-HEREDOC
  --- 
  title: #{options[:interviewee]}
  interviewee: #{options[:interviewee]}
  topic_list: #{options[:title]}
  tags: #{options[:tags]}
  soundcloud_id: #{options[:sc_id]}
  date: #{options[:date]}
  episode_number: #{options[:episode_number]}
  ---

  #{options[:text]}
  HEREDOC
  end
end

```

## class PageWriter

Again, only one job, writing out the actual page for a new podcast episode with the scraped data composed in the `MarkdownComposer`. Wait, is that really the case? At first glace it looks like it, but it has more than one reason to change. Yes, on the surface this class only writes out the page but it is also involved in logic that belongs to extracting data. It uses a few private methods to extract behaviour which kept things readable and succinct. See, it is not as easy as looking at the number of methods the API provides. The private methods can also mess with your responsibilities. We have more work to do. 

#### class PageWriter

``` ruby

class PageWriter

  attr_reader :detail_page_link
  include ExtractionUtilities

  def initialize(detail_page_link)
    @detail_page_link = detail_page_link
  end

  def write_page
    markdown_text = prepare_markdown
    file_name     = prepare_filename

    File.open(file_name, 'w') { |file| file.write(markdown_text) }
  end

  private

  def prepare_markdown
    markdown_text = compose_markdown(extract_data)
  end

  def prepare_filename
    date =           extract_data[:date]
    interviewee =    extract_data[:interviewee]
    episode_number = extract_data[:episode_number]
    file_name =      compose_filename(date, interviewee, episode_number)
  end

  def extract_data
    extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  def compose_markdown(extracted_data)
    MarkdownComposer.new.compose_markdown(extracted_data)
  end

  def compose_filename(date, interviewee, episode_number)
    "#{date}-#{dasherize(interviewee)}-#{episode_number}.html.erb.md" 
  end
end

```

Back to the drawing board. The private method above needs another look.

## PageWriter Refactored

``` ruby

class MarkdownComposer
  include ExtractionUtilities

  attr_reader :detail_page_link

  def initialize(detail_page_link)
    @detail_page_link = detail_page_link
  end

  def prepare_markdown
    compose_markdown(extract_data)
  end

  def prepare_filename
    compose_filename
  end

  private

  def compose_markdown(options={})
<<-HEREDOC
--- 
title: #{options[:interviewee]}
interviewee: #{options[:interviewee]}
topic_list: #{options[:title]}
tags: #{options[:tags]}
soundcloud_id: #{options[:sc_id]}
date: #{options[:date]}
episode_number: #{options[:episode_number]}
---

#{options[:text]}
HEREDOC
  end

  def compose_filename
    "#{date}-#{interviewee}-#{episode_number}.html.erb.md" 
  end

  def extract_data
    extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  def date
    extract_data[:date]
  end

  def interviewee
    dasherize(extract_data[:interviewee])
  end

  def episode_number
    extract_data[:episode_number]
  end
end

class PageWriter

  attr_reader :detail_page_link

  def initialize(detail_page_link)
    @detail_page_link = detail_page_link
  end

  def write_page
    text          = MarkdownComposer.new(detail_page_link).prepare_markdown
    file_name     = MarkdownComposer.new(detail_page_link).prepare_filename

    File.open(file_name, 'w') { |file| file.write(text) }
  end
end

```

So we shifted things around a bit. Big deal! Sure, that is one way to see it, but now we have a `PageWriter` class that is not dabbling in the responsibilities of composing markdown. Since the bulk of the work that we wanna achieve culminated in the creation of `.markdown` files with the data for each episode, it is more appropriate to let `MarkdownComposer do the heavy lifting. Clear cut responsibilities. And these decisions can frequently be necessary to keep the size of classes in check. Good class design will lead to touch tangential classes less and less when you introduce new code, or delete old one.

## class Scraper

As you can  see below, the `Scraper` class was unaffected by the changes we made in the previous little refactoring. That means the coupling between these classes is acceptably low in this case. The API for `Pagewriter#write_page` changed internally, but the `Scrape` class does not care a bit. It’s none of its concerns.

Here we only get Mechanize ready and loop over the pagination to extract links for each episode. Every link gets fed to the `PageWriter` class that is in charge of orchestrating the whole data extraction and writing processes.

``` ruby

class Scraper

  def scrape
    link_range = 1
    agent ||= Mechanize.new

    until link_range == 21
      page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")
      link_range += 1

      page.links[2..8].map do |link|
        PageWriter.new(link).write_page
      end
    end
  end
end

```

Below is again the full code with extracted classes that now better manage their responsibilities. Not perfect but something beginners can do pretty early in their trajectory. Extracting classes is one refactoring method to deal with a bunch of code smells and should be in your bag of tricks from the start.

## Refactored Podcast Scraper

``` ruby

require 'Mechanize'
require 'Pry'
require 'date'

module ExtractionUtilities

  def build_tags(title, interviewee)
    extracted_tags = strip_pipes(title)
    "#{interviewee}"+ ", #{extracted_tags}"
  end

  def strip_pipes(title)
    tags = title.tr('|', ',')
    tags = tags.gsub(/[@?#&]/, '')
    tags.gsub(/[w\/]{2}/, 'with')
  end

  def clean_episode_number(number)
    number.to_s.tr('#', '')
  end

  def clean_date(episode_subtitle)
    string_date = /[^|]*([,])(.....)/.match(episode_subtitle).to_s
    Date.parse(string_date)
  end

  def dasherize(text)
    text.lstrip.rstrip.tr(' ', '-')
  end
end

class PageExtractor
  attr_reader :detail_page
  include ExtractionUtilities

  def initialize(detail_page_link)
    @detail_page = detail_page_link.click
  end

  def extract_data

    options = {
      interviewee:    interviewee,
      title:          title,
      sc_id:          soundcloud_id,
      text:           shownotes_text,
      tags:           tags,
      date:           date,
      episode_number: episode_number
    }
  end

  private

  def interviewee
    interviewee_selector = '.episode_sub_title span'
    detail_page.search(interviewee_selector).text.strip
  end

  def title
    title_selector = ".episode_title"
    detail_page.search(title_selector).text.gsub(/[?#]/, '')
  end

  def soundcloud_id
    sc = detail_page.iframes_with(href: /soundcloud.com/).to_s
    sc.scan(/\d{3,}/).first
  end

  def shownotes_text
    shownote_selector = "#shownote_container > p"
    detail_page.search(shownote_selector)
  end

  def tags
    build_tags(title, interviewee)
  end

  def date
    clean_date(subtitle)
  end

  def episode_number
    number = /[#]\d*/.match(subtitle)
    clean_episode_number(number)
  end

  def subtitle
    subheader_selector = ".episode_sub_title"
    detail_page.search(subheader_selector).text
  end
end

class MarkdownComposer
  include ExtractionUtilities

  attr_reader :detail_page_link

  def initialize(detail_page_link)
    @detail_page_link = detail_page_link
  end

  def prepare_markdown
    compose_markdown(extract_data)
  end

  def prepare_filename
    compose_filename
  end

  private

  def compose_markdown(options={})
<<-HEREDOC
--- 
title: #{options[:interviewee]}
interviewee: #{options[:interviewee]}
topic_list: #{options[:title]}
tags: #{options[:tags]}
soundcloud_id: #{options[:sc_id]}
date: #{options[:date]}
episode_number: #{options[:episode_number]}
---

#{options[:text]}
HEREDOC
  end

  def compose_filename
    "#{date}-#{interviewee}-#{episode_number}.html.erb.md" 
  end

  def extract_data
    extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  def date
    extract_data[:date]
  end

  def interviewee
    dasherize(extract_data[:interviewee])
  end

  def episode_number
    extract_data[:episode_number]
  end
end

class PageWriter

  attr_reader :detail_page_link

  def initialize(detail_page_link)
    @detail_page_link = detail_page_link
  end

  def write_page
    text          = MarkdownComposer.new(detail_page_link).prepare_markdown
    file_name     = MarkdownComposer.new(detail_page_link).prepare_filename

    File.open(file_name, 'w') { |file| file.write(text) }
  end
end

class Scraper

  def scrape
    link_range = 1
    agent ||= Mechanize.new

    until link_range == 21
      page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")
      link_range += 1

      page.links[2..8].map do |link|
        PageWriter.new(link).write_page
      end
    end
  end
end


Scraper.new.scrape

```

Let’s stop here and call it a day. I recommend that you play with this code a bit on your own and see if you can come up with other strategies than class extraction to simplify things even more. There are other things to optimize for. Let’s see if we can take this a step further in the next SOLID article about the “Open/Closed Principle”. See you there.
