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

We want to extract the following data from 21 pages:

+ the title
+ the interviewee
+ the subheader with the topic list
+ the SoundCloud track number for each episode
+ the date
+ the episode number
+ the text from the show notes
+ the links from the show notes

We iterate through the pagination and let Mechanize click every link for an episode. On the following detail page we will find all the information that we need.

Using that scraped data, we want to populate the [frontmatter]() of markdown files for each episode and add a few tricks that the old site didn’t play. Having a comprehensive tagging system in place was crucial for me. I wanted listeners who use the site to have a deep discovery tool.  Therefore, I wanted to create tags for every interviewee and out of the subheader. Since I produced around 140 episodes in the first season alone, I wanted to prep the site for a time when the content becomes harder to comb through. A deep tagging system with intelligently placed recommendations was the way to go for me.

Below can you see a preview of how we will compose the new markdown files with the content we extacted. I think this will give you a good idea about the scope ahead of us. This represents the final step in our little script. Don’t worry, we will go over it in more detail. Along the way, I highly encourage you to start thinking about how you can improve the code in front of you. This will be your final task at the end of this article.

Let’s have a look at the complete code that I needed to scrape the content of my site. Look around and try to figure out the big picture what’s going on. Since I expect you to be on the beginner side of things, I stayed away from abstracting too much and err on the side of clarity of elegance. I did a couple of refactorings that were targeted at aiding code clarity but I also left a little bit of meat on the bone for you to play with when you are finished with this article.

A little hint from me, breaking up large methods into smaller ones is a good place to start. If you understood how the code works, you should have a fun time honing in on that refactoring. I already started with extracting a bunch of methods into small, focused helpers. You should be able to easily apply what you have learned from my previous articles about code smells and their refactorings. If this still goes over your head right now, don’t worry, just keep at it and at some point things will start to click.

# Full Code

``` ruby

#require 'Nokogiri'
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

# (Utilities)

def clean_date(extracted_date)
  string_date = /[^|]*([,])(.....)/.match(extracted_date).to_s
  Date.parse(string_date)
end

def build_tags(text, interviewee)
  extracted_tags = strip_pipes(text)
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

#Pry.start(binding)

```
Now that you had a chance to familiarize yourself with the puzzle piece in place, I recommend we go over them one by one and clarify a few interesting points here and there. Let’s start with the central pieces here.

#### **podcast_scraper.rb**

``` ruby

...

def write_page(link)

  detail_page = link.click

  extracted_data = extract_data(detail_page)

  markdown_text = compose_markdown(extracted_data)
  date = extracted_data[:date]
  interviewee = extracted_data[:interviewee]
  episode_number = extracted_data[:episode_number]
  file_name = "#{date}-#{dasherize(interviewee)}-#{episode_number}.html.erb.md" 

  File.open(file_name, 'w') { |file| file.write(markdown_text) }
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

...

```

What happens is that in the `scrape` method, I loop over every page in the old podcast. FYI, I’m using the old url from the heroku app since the new site is already online at [Between \| Screens](http://betweenscreens.fm/). I had 20 pages of episodes that I needed to loop over. I delimited the loop via the `link_range` variable which I updated with each loop. Going from page to page was as simple as using this variable in the url of each page.

``` ruby

page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")

```

That does all the pagination I needed. Simple and effective. Then, whenever I got a new page with another 8 episodes to scrape, I use `page.link` to identify the links we want to click and follow to the detail page for that podcast epside. I decided to go with a range of links (`links[2..8]`) since this was the easiest way to target the links I need on the page. No need to fumble around with CSS selectors here.

In the loop itself, we feed that link for the detail page to the `write_page` method. This is where most of the work is done. We take that link, follow it to the detail page and start to extract its data. On that page we find all the info that I need to compose my new markdown episodes for the new site. 

``` ruby

extracted_data = extract_data(detail_page)

```

``` ruby

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

```

As you can see above, we take that `detail_page` and apply a bunch of extraction methods on it. We extract the `interviewee`, `title`, `sc_id`, `text`, `episode_title and `episode_number`. I extracted a bunch of focused helper methods who are in charge of these extraction responsibilities. 

After that, I already start to prepare the data for composing the markdown. For example, I slice the `episode_subtitle` some more to get a clean date and build the `tags` with the `build_tags` method.  

At the end, I return an options hash that I can feed the `compose_markdown` method that composes my markdown and gets it ready for writing out the file I need for my new site.





What happens here is that I already extracted the necessary data using a bunch of smaller methods and feed it to this `compose_markdown` method through an options hash.



Why didn’t we `require "Nokogiri"`? Mechanize provides us with all our scraping needs. As we discussed in the previous article, Mechanize builds on top of Nokogiri and allows us to extract content as well.


## Helper Methods

#### **podcast_scraper**

``` ruby

...

# Extraction Methods

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

# Utilities

def clean_date(extracted_date)
  string_date = /[^|]*([,])(.....)/.match(extracted_date).to_s
  Date.parse(string_date)
end

def build_tags(text, interviewee)
  extracted_tags = strip_pipes(text)
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

...

```


















++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


!!!! Old version

``` ruby

require "HTTParty"
require 'Nokogiri'
require 'JSON'
require 'Pry'
require 'csv'
require 'mechanize'
require 'date'

def strip_pipes(text)
  tags = text.tr('|', ',')
  tags = tags.gsub(/[@?#&]/, '')
  tags = tags.gsub(/[w\/]{2}/, 'with')
end

def build_tags(text, interviewee)
  extracted_tags = strip_pipes(text)
  final_tag_list = "#{interviewee}"+ ", #{extracted_tags}"
end

def extract_soundcloud_id(detail_page)
  sc = detail_page.iframes_with(href: /soundcloud.com/).to_s
  sc = sc.scan(/\d{3,}/).first
end

def compose_markdown(options={})
<<-HEREDOC
--- 
title:          #{options[:interviewee]}
interviewee:    #{options[:interviewee]}
topic_list:     #{options[:title]}
tags:           #{options[:tags]}
soundcloud_id:  #{options[:sc_id]}
date:           #{options[:date]}
episode_number: #{options[:episode_number]}
---

#{options[:text]}
HEREDOC
end

def clean_date(extracted_date)
  string_date = /[^|]*([,])(.....)/.match(extracted_date).to_s
  date = Date.parse(string_date)
end

def extract_episode_number(text)
  episode_number = /[#]\d*/.match(text).to_s
end

def dasherize(text)
  text.lstrip.rstrip.tr(' ', '-')
end

def clean_episode_number(episode_number)
  episode_number = episode_number.to_s.tr('#', '')
end

def write_page(link)

  detail_page = link.click

  sc_id = extract_soundcloud_id(detail_page)

  title = detail_page.search('.episode_title').text
  title = title.gsub(/[?#]/, '')
  interviewee = detail_page.search('.episode_sub_title span').text
  text = detail_page.search('#shownote_container > p')

  episode_subtitle = detail_page.search('.episode_sub_title').text

  date = clean_date(episode_subtitle)

  tags = build_tags(title, interviewee)

  episode_number = extract_episode_number(episode_subtitle)
  episode_number = clean_episode_number(episode_number)

  markdown_text = compose_markdown interviewee: interviewee,
                                   title: title,
                                   tags: tags,
                                   sc_id: sc_id,
                                   text: text,
                                   date: date,
                                   episode_number: episode_number

  File.open("#{date}-#{dasherize(interviewee)}-#{episode_number}.html.erb.md", 'w') { |file| file.write(markdown_text) }
end

def scrape
    link_range = 1

  until link_range == 21
    agent ||= Mechanize.new
    page = agent.get("http://betweenscreens.fm/?page=#{link_range}")

    link_range += 1
    links = page.links

    links[2..8].map do |link|
      write_page(link)
    end
  end

end

scrape

#Pry.start(binding)

```

``` ruby

scrape

write_page

extract_soundcloud_id

clean_date

build_tags

extract_episode_number

clean_episode_number

compose_markdown

dasherize

strip_pipes

```
