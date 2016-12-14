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
+ Semi-Random Thoughts
+ Extract Class

# SOLID?

SOLID is an acronym for five design principles in object oriented programming.

+ SRP, or the “Single Responsibility Principle”.
+ Open/Closed Principle.
+ Liskov Substitution Principle.
+ Interface Segregation Principle.
+ Dependency Inversion Principle.

A lot of principles, I hear ya! Don’t let you discourage by their academic sex appeal. I assume you are new to this and you are probably still dealing with a lot of basic programming concepts. The last thing you wanna stuff your into your brain right now are coding principles. Fair point! I would say though that you are maybe missing out on an opportunity to step up your game from the very beginning.

If I would attempt to sum up the SOLID principles in one sentence, I would probably fail—miserably. What question do these principles address? This might be easier to answer. They were the result of thinking about how we can better manage dependencies. How can we write software with minimal entanglements?  How can we prevent ourselves from creating fast-rotting software? For that matter, these questions are of equal importance to beginners and pros alike.

It is not uncommon for newbies to think that they need to wait until they are ready to jump into more advanced waters like this. And that might be right in some regards. These design principles are different though. I feel like they are beginner-friendly and they will improve the quality of your code from day one. They will help you to design your code the way professionals do—at least you will start looking at your code as the big boys & girls do. What’s not to like?

Are these principles appropriate for a dynamic language like Ruby? After all, when SOLID was formed, it was in an era more dominated by statically typed Java and C++ programming. Good question! Yes, static languages might have stronger dependencies but there is no reason to throw these principles out the window when you write Ruby code. They will teach you basic techniques of how you can modularize and stabilize your code.

Stabilize? Yes, in the sense that you will be better equipped to write code that is less brittle and better prepared for the inevitable changes down the line. The ease with which you are able to change a code base is in direct correlation to its quality. I recommend that you start thinking about this early in your developer career. You can start by giving these OO design principles a little sneak peak.

# SRP: Single Responsibility Principle

> A class should have only one reason  to change

Your classes have ideally only one responsibility. If a class is focused on only one job, there is no other reason to change it. In short, I’d say this is the opposite of spaghetti code. You decrease your chances of creating unwanted entanglements and hanging your app with code that has too many intermingled responsibilities. If you would follow only a single one principle in your coding journey, SRP would be a good candidate. It brings a lot of benefits to the table—at literally little to no cost. Almost as a byproduct, it also encourages other positive behaviour in your code. Neat!

## Reasons to Change

Reasons to change roughly equals responsibilities. If you have a class with five different jobs, you have a good chance of ending up with five reasons to change. Say we have a class that sets a session, sends email which also gets validated and does the weather forecast for the next week? If any of these things change, the rest of the class is possibly affected by that. This is exactly what SRP wants us to avoid. This is the brittleness we talk about.

So we want to have one class that handles everything in regards to forecasting the weather, one that is in charge of sending email, one that creates a session and one that handles calculating the weather forecast. Maybe even two for that. One that handles the weather forecast for a week and another which is in charge of the next 24 hours.

When you look at this picture below, you can clearly see what you should avoid. This is a tool that tries to be too many things. It sure looks funny, I give you that, but at the same time it doesn’t look that useful either, does it?. Try to carry that around in your pocket, hell, even  holding it in your hands is an acrobatic challenge. That is the kind of class design we want to avoid. We want to keep things simple and organized. In terms of swiss army knives, we prefer one tool at a time.

(https://www.flickr.com/photos/redjar/113974357)

![Alt text](/images/SOLID/swiss-army-knife.jpg)

## Cohesive Classes

Classes should be highly cohesive. Every piece of a class should be highly related to other pieces of a class. In our imaginary example from above, the weather forecast is not highly related to email validation, sending email or creating a session. So we end up with weak cohesion which basically let’s your app rot from the inside. You are setting up the dynamite to blow up the house you are living in.

O.K., let’s not overdramatize these things, I was just trying to find a strong metaphor that sticks with you. Nobody is gonna loose his life or anything, but some people might loose their minds if these things pile up for years and suddenly the need for change hits you hard. Entangling large pieces of spaghetti code could literally break a product if people loose the ability to rebuild the application with the components at hand.

Let’s talk Rails for a second. In Rails, people put tons of stuff in ApplicatioController or ApplicationHelper that are often not very cohesive by nature. It’s a top-level object that can easily collect a bunch of inconvenient stuff. You will often see applications where they act as a dumpsters for stuff that the lazy fix didn’t compose a better home for. This can quickly get out of hand and pile on stuff from all over the place. Just because it’s easy to use these compartments for “global” stuff, doesn’t mean that we should stop there. We need to take the time to compose our objects with their dedicated responsibilities in mind. Also, even as beginners, we already know that global stuff is rarely in our best self-interest.

## Changes

The question about inevitable change is, how would you like the ride to be? Crazy as hell or fun and interesting. Do you wanna feel like the world is doomed or just another day at the office with an interesting problem. The design of your application will strongly influence the experience of change. If you have an unflexible, rigid system in place, you can count on creating a cascade of related changes that pop up during that process. Fragile systems are not your friend.

https://commons.wikimedia.org/wiki/File:Toppledominos.jpg

![Alt text](/images/SOLID/Toppledominos.jpg)

We don’t want to create applications where everything is connected to everything else. That is a nightmare to work with. It’s basically a fancier way of saying it’s “Spaghetti code”. That does not sound like a four star 20 course menu, does it? You don’t want to build incredible long domino lines that all fall by tripping over a single piece.

https://www.flickr.com/photos/baccharus/5817342671

![Alt text](/images/SOLID/cable-mess.jpg)

## Benefits

Breaking classes into focused components have a few benefits that can easily go unnoticed. A few benefits that I can see are the following: 

+ Method names can often stay simpler due to fewer dependencies referenced.
+ Better organization because you grouped functionality more coherently.
+ Namespace clashes are easier to avoid.
+ Classes are easier to read and digest.
+ Same might apply for variable names.
+ Simpler, more readable APIs.
+ Smaller objects.
+ Faster objects.


# Semi-Random Thoughts

In this section I just wanna quickly discuss a few topics that fit into this whole context. The “Single Responsibility Principle” has everything to do with the following considerations:

+ Clarity
+ Reusability
+ Easier to Test
+ Easy to Change
+ Code Quality
+ Downloading Context
+ Boundaries
+ God Classes
+ Gem Churn
+ Bugs
+ Silver Bullet
+ Good Design?
+ And / Or
+ Hard
+ Payoff
+ SRP Symptoms

## Clarity

You want to avoid classes that have too much going on. If they start to feel blobby and you need to take some time to figure out what’s actually going on, it’s probably a good time to refactor its responsibilities. SRP aids the need for readable classes as well. Long, convoluted classes are not exactly as easy to read. In that sense, big classes can often be a smell that they took on too many responsibilities. Applying SRP will have an immediate effect on the clarity of these objects. They will end up being more focused and succinct. Always optimize for readability. Your fellow coders are still human. 

## Reusability

Ideally, you want your classes to handle one responsibility well so that you can reuse that functionality in other places as well. Keeping things isolated, in their own little scope, is a good guideline to end up with small components that increase the chance of being useful some place else. Ideally they can be recycled in your code.

## Easy to Test

When you don’t have classes that are too involved, and frankly all over the place, you can write much more focused and readable tests as well. Your tests are probably a good indicator of the quality of your your classes’ design. With single responsibilities, they also run faster because of reduced dependencies. This really is all about exposure.

Look out for tests that are hard to set up and involve a lot of dependencies. That’s a good indicator that you should spend some time on simplifying the class design. Besides tests being slower, if classes don’t follow SRP, they will also be more brittle along they way. Your classes and your tests will change, this is the one certainty you have. The less responsibilities you encapsulate in classes, the less brittle your code will end up being.

## Easy to Change

Classes with tons of responsibilities are a pain to change. Since this is expected future behaviour of your applications, you should plan for it. SRP classes are more flexible to deal with that scenario. They are easy to refactor if your application goes through growing pains for example.

Flexibility and resiliency are two important qualities you want to have from every piece of your code. The Single Responsibility Principle hands you these benefits on a silver platter without much effort. Rewriting you work becomes less and less of a liability—or even a necessity when reuse and modularity are baked in from the start.

## Code Quality

For senior software writers, the ease of being able to change code when the requirements necessitate it, is one of their highest metrics to assess code quality. They have seen all kinds of trends and cool new stuff but what they often look out for is simplicity and elegance. Code that has adaptability built in is a good indicator of a software writer that aims to produce quality software—not only quick fixes that cater to short-term needs.

## Downloading Context

If the classes are smaller, you will give other team members and your future self an easier time to get familiarized with its behaviour as well. Nobody wants to open a class and sit there for 15 minutes before they have a general sense of what’s going on. That will make people—you guessed it—put funny stuff in your coffee at some point.

Also, what developers need to load into their heads in this context, is often what you will end up needing to load as dependencies in your tests. Not exactly ideal in many ways..

## Boundaries

Often you don’t know from the beginning how the responsibilities are spread. Therefore you should not over-optimize from the start. It’s really hard to know where the journey is headed. It’s best to evolve your code to become SRP friendly step by step. Some classes will end up being magnets for responsibilities while others are likely to be a lot more lazy in piling stuff up.

## God Classes

White we’re at it. Always think hard before you add something to models like User. It is such an attractive magnet for all sorts of things in most applications. This and the class that tackles the heart of the business logic, like bookmarking in a bookmarking app for example. You should be especially hawk-eyed with these. If you don’t pay attention they can quickly become black holes in your code, devouring tons of responsibilities at once. We call them god classes because they know too much about other classes’ inner businesses.

## Gem churn

When classes change often it tells you that they have many reasons to change. [Churn](https://github.com/danmayer/churn) is a gem that, among other things, will show you how often particular classes have been changed. This gives you an easy peek into your core models and updates you that the rate of change might be alarming or not. These statistics can help you to identify which classes might be in need of an SRP treatment. Your god classes and the ones that cover the central business proposition will often end up being high on that list.

## Bugs

Intermingling and churn is an open door for bugs. It’s almost like inviting cockroaches into your home by laying out food at your doorstep. SRP is one of the most effective strategies to minimize the risk for serious insect hazard. All that spaghetti code is a feeding frenzy for bugs.

## Silver Bullet

You will often have to weigh different principles against each other. SRP is no silver bullet that fits every occasion. It is often a very good starting point though. Finding some balanced middle ground will always be part of your job when you design applications. That also expands to applying code quality principles. The more you know, the more you might need to weigh. Having said that, I don’t want this to sound bleak at all. It is actually quite fun to sandblast your code like that. As with professional writers, the first couple of drafts are often just the beginning of some heavy editing. That is not only normal but to be expected and fun.

## Good design?

Dependencies tell you how change is gonna propagate throughout your code. Many dependencies create a sort of pressure to not change things. Worse if you don’t know to what extend a little change will ripple through your code.

The SOLID design principles are a great way to start thinking about good design. People with decades of experience extracted some real hard lessons for you to reap. Keeping dependencies in check is on top of their list when it comes to code quality. We should not try to reinvent the wheel too much and build upon their shoulders.

## And / Or

If you need “and” or “or” in the description of the purpose, the functionality in a particular class, you can bet that applying SRP is a good idea. Both scenarios imply that it does more than one focused thing. As mentioned above, nothing is set in stone, there is no approach that fits all scenarios, but at least it should raise the question if it might be a good idea to refactor your code until “and” and “or” is out of that class’s elevator pitch.

## Hard

“Nothing good comes easy” as they say. Maybe, I give you that. In the beginning it will be relatively easy to break models into focused classes that do one thing. That will be your foundation to move forward into more complex waters. Take your time to build this base with care. Rushing is not your friend. You don’t go fast by writing bad code. That’s for sure. Chances are pretty good that the opposite is adequate.

Once complexity kicks in, it becomes a lot harder to break objects apart. I mean, there is no way around it. It will happen anyway, but with a good foundation you can build higher and more stable—as with buildings. It will get harder over time, that is probably for sure. But at the same time, the problems also get more interesting, more challenging. It is a fun process that you might actually enjoy. Embrace it!

## Payoff

Good design will pay off, similar to having a test suite around. It might cost you a little bit of extra effort in the beginning, but going forward, you will be glad that you have built on solid ground. Once you feel the need to fight your app all the time, you will wish you could go back to the good times where the application was young and perfect. A time where you had an easy opportunity to build a solid design basis.

Good design takes time in the beginning, as does TDD. No doubt about it! But you can count on being better prepared for changing conditions down the line. Actually, it will most likely cost you extra money if you don’t pay attention to design in your application. Cleaning up spaghetti code takes time and costs big bucks. Ouch!

## SRP Symtoms

To close this section of semi-random thoughts, let’s summarize the symptoms of code that adheres to SRP.

When your code is

+ loosely coupled
+ highly cohesive
+ easily composeable
+ context independent

chances are good that your responsibilities are well spread and managed. This keeps your classes and methods small. These factors directly lead to better maintainability. Coupling for example is just another fancy term for dependencies. Managing dependencies is a huge chunk of your actual work as a software writer and this is what SRP and SOLID in general is all about.

# Extract Class

Spaghetti in your code? What should you do? SRP, you will very often require you to extract classes. This is a very common refactoring method. If you have a bigger class with many responsibilities, you will end up needing more classes that have smaller, more focused jobs. Don’t put functions that change for different reasons into the same class. That means you can organize your classes around functions that change for the same reason.

## Scraper Example

In my last article about scraping websites, I built a small scraper to get data off my podcast [Between \| Screens](http://betweenscreens.fm/). I left the readers with a small exercise to play with. Since it was another article that was targeted at coding newbies, I wanted the readers to take the code and play with it. The exercise was to find ways to refactor that piece of code once they wrapped their heads around it. 

In the article I focused solely on the functionality, the API and how readers can use [Nokogiri](http://www.nokogiri.org/) / [Mechanize](https://github.com/sparklemotion/mechanize) to extract data from websites. I thought I don’t want to muddy the waters with code smells and refactorings in that context. After all, I expect most of my readers to be freshly hatched coders. Confusing newbies with various different concepts at the same time was something I disliked starting out.

I gave a little hint that extracting classes is a good way to start getting this code in better shape. Let’s use this code and see how we can encapsulate it into objects that focus on simple responsibilities. If the example feels a little dense, I recommend that you go back to at least the third article in the series. You will find detailed explanations for every method this web scraper is executing. 

## Raw Podcast Scraper

``` ruby

require 'Mechanize'
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
  extracted_tags = clean_title(title)
  "#{interviewee}"+ ", #{extracted_tags}"
end

def clean_title(text)
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
require 'date'

module ExtractionUtilities

  def build_tags(title, interviewee)
    extracted_tags = clean_title(title)
    "#{interviewee}"+ ", #{extracted_tags}"
  end

  def clean_title(title)
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
    @detail_page ||= detail_page_link.click
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

We could have extracted some more but for now I felt it would be overkill to split the module up. If I would amass more methods like `dasherize`, I would start thinking to find a better home for them. Since I need these utilities in more than one place I chose a module for now and include these methods where needed. I felt the `class PageExtractor` shouldn’t be concerned about stuff like striping pipe characters or building tags from the extracted data. That way we also increase the likelihood that we can reuse functionality.

#### module ExtractionUtilities

``` ruby

module ExtractionUtilities

  def build_tags(title, interviewee)
    extracted_tags = clean_title(title)
    "#{interviewee}"+ ", #{extracted_tags}"
  end

  def clean_title(title)
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

This class now has one responsibility, extracting the data from the link it was fed in the loop that visits every podcast episode. It returns an options hash that stores the data we needed from the show notes site. It has a few private methods that are cohesive enough to not extract into another object on their own.

+ `interviewee`
+ `title`
+ `soundcloud_id`
+ `shownotes_text`
+ `tags`
+ `date`
+ `episode_number`
+ `subtitle`

They are made private to not mess with the API of this class while providing the convenience of looking up the implementation of scraping various selectors—without needing to go to another class or module. I felt they belong better into this class since they are in charge of actually scraping the data that we export in the options hash.

The class includes a module that takes care of smaller utility functions like `clean_episode_number`, `clean_date` and `build_tags`. As always, trade offs are part of the game. I had to weigh readability and cohesiveness when I decided to put these methods into a module. It might not be immediately clear where they come from, but on the other hand, their implementation might not be needed to be investigated most of the time. We saved some space in the private section of this class and put them into a module that fits their purpose better.

#### class PageExtractor

``` ruby

class PageExtractor
  attr_reader :detail_page
  include ExtractionUtilities

  def initialize(detail_page_link)
    @detail_page ||= detail_page_link.click
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

## PageWriter v.2

``` ruby

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

class MarkdownComposer
  include ExtractionUtilities

  attr_reader :extracted_data

  def initialize(detail_page_link)
    @extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  def prepare_markdown
    compose_markdown(extracted_data)
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

  def date
    extracted_data[:date]
  end

  def interviewee
    dasherize(extracted_data[:interviewee])
  end

  def episode_number
    extracted_data[:episode_number]
  end
end

```

So we shifted things around a bit, big deal! Sure, that is one way to see it, but now we have a `PageWriter` class that is not dabbling too much into the responsibilities of composing markdown (we will improve the design even more when we get to injecting dependencies in the next article).

Since the bulk of the work that we wanna achieve culminated in the creation of `.markdown` files with the data for each episode, it is more appropriate to let `MarkdownComposer` do the heavy lifting. Clear cut responsibilities. Moving things around like this can frequently be necessary to keep the size of classes in check. Good class design will lead to touch tangential classes less and less when you introduce new code, or delete old one.

But! Doesn’t the `PageWriter` class still feel a bit smelly? After all, it does handle the prepping of filenames and markdown text. Sure, it has a single method, `write_page`, but its responsibilities can be split up even more. As a last step before we move forward, I suggest we create another class that is in charge of `FilenameComposition` and adjust `PageWriter` to make use of it. 

## PageWriter v.3

``` ruby

class PageWriter

  attr_reader :text, :file_name

  def initialize(detail_page_link)
   @text          = MarkdownComposer.new(detail_page_link).prepare_markdown
   @file_name     = FilenameComposer.new(detail_page_link).prepare_filename
  end

  def write_page
    File.open(file_name, 'w') { |file| file.write(text) }
  end
end

class MarkdownComposer

  attr_reader :extracted_data

  def initialize(detail_page_link)
    @extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  def prepare_markdown
    compose_markdown(extracted_data)
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
end

class FilenameComposer
  include ExtractionUtilities

  attr_reader :extracted_data

  def initialize(detail_page_link)
    @extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  def prepare_filename
    compose_filename
  end

  private

  def compose_filename
    "#{date}-#{interviewee}-#{episode_number}.html.erb.md" 
  end

  def date
    extracted_data[:date]
  end

  def interviewee
    dasherize(extracted_data[:interviewee])
  end

  def episode_number
    extracted_data[:episode_number]
  end
end

```

To summarize, we could remove the `ExtractionUtilities` mixin from `PageWriter` and extracted the methods necessary for the filename creation from `MarkdownComposer` into the new class `FilenameComposer`. This new class now has one single job and does not mix filename business with markdown stuff. If you extend this class in the future, it will be able to easily handle the creation of any filename you like—not only markdown files.

I hope this little example was able to illustrate that you will often take this design process step by step. It is more of a discovery process where you are guided by coding principles. Like above, you will often shuffle stuff around until you can’t extract anymore. I suspect that very few people design these things perfectly in their head without sandblasting the code along the way. So far so good. There is still work to do with this class but we can save that for another SOLID article.

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
require 'date'

module ExtractionUtilities

  def build_tags(title, interviewee)
    extracted_tags = clean_title(title)
    "#{interviewee}"+ ", #{extracted_tags}"
  end

  def clean_title(title)
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
    @detail_page ||= detail_page_link.click
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

  attr_reader :extracted_data

  def initialize(detail_page_link)
    @extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  def prepare_markdown
    compose_markdown(extracted_data)
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
end

class FilenameComposer
  include ExtractionUtilities

  attr_reader :extracted_data

  def initialize(detail_page_link)
    @extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  def prepare_filename
    compose_filename
  end

  private

  def compose_filename
    "#{date}-#{interviewee}-#{episode_number}.html.erb.md" 
  end

  def date
    extracted_data[:date]
  end

  def interviewee
    dasherize(extracted_data[:interviewee])
  end

  def episode_number
    extracted_data[:episode_number]
  end
end

class PageWriter

  attr_reader :text, :file_name

  def initialize(detail_page_link)
   @text          = MarkdownComposer.new(detail_page_link).prepare_markdown
   @file_name     = FilenameComposer.new(detail_page_link).prepare_filename
  end

  def write_page
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

# Final Thoughts

Let’s stop here and call it a day. I recommend that you play with this code a bit on your own and see if you can come up with other strategies than class extraction to simplify things even more. There are other things to optimize for. Let’s see if we can take this a step further in the next SOLID article about the “Open/Closed Principle”. See you there.
