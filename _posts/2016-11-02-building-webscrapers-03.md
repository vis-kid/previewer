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

#### **podcast_scraper central piece**

``` ruby

...


def write_page(link)

  detail_page = link.click

  interviewee = extract_interviewee(detail_page)
  title = extract_title(detail_page)
  sc_id = extract_soundcloud_id(detail_page)
  text = extract_shownotes_text(detail_page)
  episode_subtitle = extract_subtitle(detail_page)
  episode_number = extract_episode_number(episode_subtitle)
  date = clean_date(episode_subtitle)
  tags = build_tags(title, interviewee)

  markdown_text = compose_markdown interviewee: interviewee,
                                   title: title,
                                   tags: tags,
                                   sc_id: sc_id,
                                   text: text,
                                   date: date,
                                   episode_number: episode_number

  #Pry.start(binding)
  File.open("#{date}-#{dasherize(interviewee)}-#{episode_number}.html.erb.md", 'w') { |file| file.write(markdown_text) }
end

def scrape
    link_range = 1

  #until link_range == 21
  until link_range == 2
    agent ||= Mechanize.new
    #page = agent.get("http://betweenscreens.fm/?page=#{link_range}")
    page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")

    link_range += 1
    links = page.links

    links[2..8].map do |link|
      write_page(link)
    end
  end
end

...

```

What happens here is that I already extracted the necessary data using a bunch of smaller methods and feed it to this `compose_markdown` method through an options hash.



Why didn’t we `require "Nokogiri"`? Mechanize provides us with all our scraping needs. As we discussed in the previous article, Mechanize builds on top of Nokogiri and allows us to extract content as well.


## Helper Methods

#### **podcast_scraper**

``` ruby

...

# Helper Methods

def strip_pipes(text)
  tags = text.tr('|', ',')
  tags = tags.gsub(/[@?#&]/, '')
  tags.gsub(/[w\/]{2}/, 'with')
end

def build_tags(text, interviewee)
  extracted_tags = strip_pipes(text)
  "#{interviewee}"+ ", #{extracted_tags}"
end

def extract_soundcloud_id(detail_page)
  sc = detail_page.iframes_with(href: /soundcloud.com/).to_s
  sc.scan(/\d{3,}/).first
end

def clean_date(extracted_date)
  string_date = /[^|]*([,])(.....)/.match(extracted_date).to_s
  Date.parse(string_date)
end

def clean_episode_number(episode_number)
  episode_number.to_s.tr('#', '')
end

def extract_episode_number(episode_subtitle)
  number = /[#]\d*/.match(episode_subtitle)
  clean_episode_number(number)
end

def dasherize(text)
  text.lstrip.rstrip.tr(' ', '-')
end

def extract_title(detail_page)
  title_selector = ".episode_title"
  detail_page.search(title_selector).text.gsub(/[?#]/, '')
end

def extract_interviewee(detail_page)
  interviewee_selector = '.episode_sub_title span'
  detail_page.search(interviewee_selector).text
end

def extract_shownotes_text(detail_page)
  shownote_selector = "#shownote_container > p"
  detail_page.search(shownote_selector)
end

def extract_subtitle(detail_page)
  subheader_selector = ".episode_sub_title"
  detail_page.search(subheader_selector).text
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

...

```

## Full Code

``` ruby

require 'Pry'
#require 'Nokogiri'
require 'Mechanize'
require 'date'

# Helper Methods

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

def write_page(link)

  detail_page = link.click

  interviewee = extract_interviewee(detail_page)
  title = extract_title(detail_page)
  sc_id = extract_soundcloud_id(detail_page)
  text = extract_shownotes_text(detail_page)
  episode_subtitle = extract_subtitle(detail_page)
  episode_number = extract_episode_number(episode_subtitle)
  date = clean_date(episode_subtitle)
  tags = build_tags(title, interviewee)

  markdown_text = compose_markdown interviewee: interviewee,
                                   title: title,
                                   tags: tags,
                                   sc_id: sc_id,
                                   text: text,
                                   date: date,
                                   episode_number: episode_number

  #Pry.start(binding)
  File.open("#{date}-#{dasherize(interviewee)}-#{episode_number}.html.erb.md", 'w') { |file| file.write(markdown_text) }
end

def scrape
    link_range = 1
    agent ||= Mechanize.new

  #until link_range == 21
  until link_range == 2
    #page = agent.get("http://betweenscreens.fm/?page=#{link_range}")
    page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")
    link_range += 1
    #links = page.links

    page.links[2..8].map do |link|
      write_page(link)
    end
  end

end

scrape

#Pry.start(binding)


```



















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
