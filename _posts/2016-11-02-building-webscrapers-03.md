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
+ Pry
+ Scraper
+ Helper Methods
+ Writing Posts

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

Let’s break down what we want to accomplish. We want to extract the following data from 21 pages:

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

Why didn’t we `require "Nokogiri"`? Mechanize provides us with all our scraping needs. As we discussed in the previous article, Mechanize builds on top of Nokogiri and allows us to extract content as well.

# Pry

First things first. Before we jump into our code here, I thought it was necessary to show you how you can efficiently check if your code works as expected every step of the way. As you have certainly noticed, I have added another tool to the mix. I added `Pry` which is really handy for debugging. If you place `Pry.start(binding)` anywhere in your code, you can inspect your application at exactly that point.

It is really helpful to check every step along the way if you have the data you expected. For example, let’s place it right after our `write_page` function and check if `link` is what we expect.

#### Pry

``` ruby

...

def scrape
  link_range = 1
  agent ||= Mechanize.new

  until link_range == 21
    page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")
    link_range += 1

    page.links[2..8].map do |link|
      write_page(link)
      Pry.start(binding)
    end
  end
end

...

```

If you run the script, we will get something like this.

#### Output

``` bash

»$ ruby noko_scraper.rb

    321: def scrape
    322:     link_range = 1
    323:     agent ||= Mechanize.new
    324: 
    326:   until link_range == 21
    327:     page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")
    328:     link_range += 1
    329: 
    330:     page.links[2..8].map do |link|
    331:       write_page(link)
 => 332:     Pry.start(binding)
    333:     end
    334:   end
    335: end

[1] pry(main)>

```

When we then ask for the `link` object, we can check if we are on the right track before we move on to other implementation details.

``` bash

[2] pry(main)> link
=> #<Mechanize::Page::Link
 "Masters @ Work | Subvisual | Deadlines | Design personality | Design problems | Team | Pushing envelopes | Delightful experiences | Perfecting details | Company values"
 "/episodes/139">

```

Looks like what we need. Great, we can move on. Doing this step by step through the whole application is an important practice not to get lost and really understand how it works. I won’t cover Pry here in any more detail since it would take me at least another fatty article to do so. I can only recommend using it as an alternative to the standard IRB shell. Back to our main task.

# Scraper

Now that you had a chance to familiarize yourself with the puzzle pieces in place, I recommend we go over them one by one and clarify a few interesting points here and there. Let’s start with the central pieces.

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

#### def scrape

``` ruby

page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")

```

That does all the pagination I needed. Simple and effective. Then, whenever I got a new page with another 8 episodes to scrape, I use `page.link` to identify the links we want to click and follow to the detail page for that podcast epside. I decided to go with a range of links (`links[2..8]`) since this was the easiest way to target the links I need on the page. No need to fumble around with CSS selectors here.

In the loop itself, we feed that link for the detail page to the `write_page` method. This is where most of the work is done. We take that link, follow it to the detail page and start to extract its data. On that page we find all the info that I need to compose my new markdown episodes for the new site. 

#### def write_page

``` ruby

extracted_data = extract_data(detail_page)

```

#### def extract_data

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

As you can see above, we take that `detail_page` and apply a bunch of extraction methods on it. We extract the `interviewee`, `title`, `sc_id`, `text`, `episode_title and `episode_number`. I extracted a bunch of focused helper methods who are in charge of these extraction responsibilities. Let’s have a quick look at them:

# Helper Methods 

## Extraction Methods

I extracted these methods because it enabled me to have smaller methods overall. Encapsulating their behaviour was also important. The code reads better as well. Most of them take the `detail_page` as an argument and extract some specific data we need for our Middleman posts.

``` ruby

def extract_interviewee(detail_page)
  interviewee_selector = '.episode_sub_title span'
  detail_page.search(interviewee_selector).text.strip
end

```

We search the page for a specific selector and get the text without unnecessary white space.

``` ruby

def extract_title(detail_page)
  title_selector = ".episode_title"
  detail_page.search(title_selector).text.gsub(/[?#]/, '')
end

```

We return the title and clean it from `?` and `#` since this doesn’t play nice with the frontmatter in the posts for our episodes. More about frontmatter further below.

``` ruby

def extract_soundcloud_id(detail_page)
  sc = detail_page.iframes_with(href: /soundcloud.com/).to_s
  sc.scan(/\d{3,}/).first
end

```

Here we needed to work a little harder to extract the SoundCloud id for our hosted trackes. First we need the Mechanize iframe with the `href` of soundcloud and make it a string for scanning..

``` bash

"[#<Mechanize::Page::Frame\n nil\n \"https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/221003494&amp;auto_play=false&amp;hide_related=false&amp;show_comments=false&amp;show_user=true&amp;show_reposts=false&amp;visual=true\">\n]"

```

Then match a regular expression for its digits for the track id, our `soundcloud_id` `"221003494"`.

``` ruby

def extract_shownotes_text(detail_page)
  shownote_selector = "#shownote_container > p"
  detail_page.search(shownote_selector)
end

```

Extracting show notes is again quite straightforward. All we need is look for show notes paragraphs in the detail page. No surprises here. 

``` ruby

def extract_subtitle(detail_page)
  subheader_selector = ".episode_sub_title"
  detail_page.search(subheader_selector).text
end

```

Same goes for the subtitle, except that it is just a preparation to extract the episode number from it.

``` ruby

def extract_episode_number(episode_subtitle)
  number = /[#]\d*/.match(episode_subtitle)
  clean_episode_number(number)
end

```

Here we need another round of regular expression. Let’s have a look before and after we applied the regex.

#### Subheader

``` bash

" João Ferreira  | 12  Minutes |  Aug 26, 2015 | Episode #139  "

```

#### Extracted episode

``` bash

"#139"

```

One more step until we have a clean number.

``` ruby

def clean_episode_number(number)
  number.to_s.tr('#', '')
end

```

We take that number with a hash `#` and remove it. Voilà, we have our first episode number `139`` extracted as well. I suggest we look at the smaller utility methods as well before we bring it all together.

## Utility Methods

After all extraction business, we have some cleanup to do. We can already start to prepare the data for composing the markdown. For example, I slice the `episode_subtitle` some more to get a clean date and build the `tags` with the `build_tags` method.  

``` ruby

def clean_date(episode_subtitle)
  string_date = /[^|]*([,])(.....)/.match(episode_subtitle).to_s
  Date.parse(string_date)
end

```

We run another regex that looks for dates like this `"  Aug 26, 2015"`. As you can see, this is not very helpful yet. From that `string_date` we get from the subtitle, we need to create a real `Date` object. Otherwise it would be useless for properly creating Middleman posts.

#### sting_date

``` bash

"  Aug 26, 2015"

```

Therefore we take that string and do a `Date.parse`. The result looks a lot more promising.

#### Date

``` bash

2015-08-26

```


``` ruby

def build_tags(title, interviewee)
  extracted_tags = strip_pipes(title)
  "#{interviewee}"+ ", #{extracted_tags}"
end

```

This takes the `title` and `interviewee` we have built up inside the `extract_data` method and removes all pipe characters and junk. We replace pipe characters with a comma, replace `@`, `?`, `#`, `&` with an empty string and finally take care of abbreviations for `with`.

#### def strip_pipes

``` ruby

def strip_pipes(text)
  tags = text.tr('|', ',')
  tags = tags.gsub(/[@?#&]/, '')
  tags.gsub(/[w\/]{2}/, 'with')
end

```

In the end we include the interviewee name in the tag list as well and separate each tag by a comma.

#### Before

``` bash

"Masters @ Work | Subvisual | Deadlines | Design personality | Design problems | Team | Pushing envelopes | Delightful experiences | Perfecting details | Company values"

```

#### After

``` bash

"João Ferreira, Masters  Work , Subvisual , Deadlines , Design personality , Design problems , Team , Pushing envelopes , Delightful experiences , Perfecting details , Company values"

```

Each of these tags will end up being a link to a collection of posts for that topic. All of this happened inside the `extract_data` method. Let’s have another look where we are:

#### def extract_data

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



All that is left to do here is return an options hash with the data we exracted. We can feed this returned options hash into the `compose_markdown` method which then composes my markdown. It gets it ready for writing out the file I need for my new site.

# Writing Posts

#### def compose_markdown

``` ruby

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

```

For creating episodes on my middleman site, I opted to repurpose its blogging system. Instead of creating “pure” blog posts, I create show notes for my episodes that display the SoundCloud hosted episdes via iframes. On the index site, I only display that iframe plus title and stuff.  

The format I need for this to work is comprised of something called frontmatter. This is basically a key / value store for my static sites. It is replacing my database needs from my old Sinatra site. Data like interviewee name, date, SoundCloud track id, episode number and so on goes in between three dashes (`---`) on top of our episodes files. Below comes the content for each episode. Stuff like questions, links, sponsor stuff…

#### frontmatter

```

---
key: value
key: value
key: value
key: value
---

Episode content goes here.

```

In the `compose_markdown` method, I use a `HEREDOC` to compose that file with its frontmatter for each episode we loop through. From the options hash we feed this method, we extract all the data that we collected in the `extract_data` helper method.

#### def compose_markdown

```

...

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

...

```

This is the blueprint for a new podcast episode right there. This is what we came for. In case you are wondering about this syntax `#{options[:interviewee]}`. I interpolate as usual with strings but since I’m already inside a `<<-HEREDOC` I can leave the double quotes off.

Just to orientate ourselves, we still in the loop, inside the `write_page` function for each clicked link to a detail page with the show notes of a singular episode. What happens next is prepping to write this blueprint to the file system. In other words, we create the actual episode by providing a file name and the composed `markdown_text`. 

For that final step, we just need to prepare the following ingredients. The date, the interviewee name and the episode number. Plust the `markdow_text` of course which we just got from `compose_markdown`.

#### def write_page

``` ruby

...

markdown_text = compose_markdown(extracted_data)
date = extracted_data[:date]
interviewee = extracted_data[:interviewee]
episode_number = extracted_data[:episode_number]

file_name = "#{date}-#{dasherize(interviewee)}-#{episode_number}.html.erb.md" 

...

```

Then we only need to take the `file_name` and the `markdown_text` and write the file.

#### def write_page

``` ruby

...

File.open(file_name, 'w') { |file| file.write(markdown_text) }

...

```

Let’s break this down as well. For each post I need a specific format. Something like `2016-10-25-Avdi-Grimm-120`. I want to write out files that start with the date, has the interviewee name and the episode number.

To match the format Middleman expects for new posts, I needed to take the interviewee name and put it through my helper method to `dasherize` the name for me. From `Avdi Grimm` to `Avdi-Grimm`. Nothing magic, but worth a look:

#### def dasherize

``` ruby

def dasherize(text)
  text.lstrip.rstrip.tr(' ', '-')
end

```

It removes whitespace from the text we scraped for the interviewee name and replaces the white space between Avdi and Grimm with a dash. The rest of the filename is dashed together in the string itself. `"date-interviewee-name-episodenumber"`

#### def write_page

``` ruby

...

"#{date}-#{dasherize(interviewee)}-#{episode_number}.html.erb.md"

...

```

Since the extracted content comes straight from an HTML site I can’t simply use `.md` or `.markdown` as filename extension. I decided to go with `.html.erb.md`. For future episodes that I compose without scraping, I can leave off the `.html.erb` part and only need `.md`.

After this step, the loop in the `scrape` function ends and we should have something that looks like this:

#### 2014-12-01-Avdi-Grimm-1.html.erb.md

``` HTML

--- 
title: Avdi Grimm
interviewee: Avdi Grimm
topic_list: What is Rake | Origins | Jim Weirich | Common use cases | Advantages of Rake
tags: Avdi Grimm, What is Rake , Origins , Jim Weirich , Common use cases , Advantages of Rake
soundcloud_id: 179619755
date: 2014-12-01
episode_number: 1
---

<p class="show_notes_display"><b>Questions:</b><br>- What is Rake?<br>- What can you tell us about the origins of Rake?<br>- What can you tell us about Jim Weihrich?<br>- What are the most common use cases for Rake?<br>- What are the most notable advantages of Rake?<br><br><b>Links:</b><br><a rel="nofollow" target="_blank" href="http://www.youtube.com/watch?v=2ZHJSrF52bc">In memory of the great Jim Weirich</a><br><a rel="nofollow" target="_blank" href="https://github.com/jimweirich/rake">Rake on GitHub</a><br><a rel="nofollow" target="_blank" href="https://github.com/jimweirich">Jim Weirich on GitHub</a><br><a rel="nofollow" target="_blank" href="http://www.youtube.com/watch?v=AFPWDzHWjEY">Basic Rake</a> talk by Jim Weirich<br><a rel="nofollow" target="_blank" href="http://www.youtube.com/watch?v=KaEqZtulOus">Power Rake</a> talk by Jim Weirich<br><a rel="nofollow" target="_blank" href="http://devblog.avdi.org/2014/04/30/learn-advanced-rake-in-7-episodes/">Learn advanced Rake in 7 episodes - from Avdi Grimm ( free )</a><br><a rel="nofollow" target="_blank" href="http://about.avdi.org/">Avdi Grimm</a><br>Avdi Grimm’s screencasts: <a rel="nofollow" target="_blank" href="http://www.rubytapas.com/">Ruby Tapas</a><br><a rel="nofollow" target="_blank" href="http://devchat.tv/ruby-rogues/">Ruby Rogues</a> podcast with Avdi Grimm<br>Great ebook: <a rel="nofollow" target="_blank" href="http://www.amazon.com/Rake-Management-Essentials-Andrey-Koleshko/dp/1783280778">Rake Task Management Essentials</a> from<a rel="nofollow" target="_blank" href="https://twitter.com/ka8725"> Andrey Koleshko</a><br><br><br><br><br><br><br><br><br></p>

```

This scraper would start at the last episode of course and loops until the first. For demonstration purposes, episode 01 is as good as any. You can see on top the frontmatter with the data we extracted. All of that was previously locked in the database of my Sinatra app. Episode number, date, interviewee name and so on. Now we have it prepared to be part of my new static Middlman site. Everything below the two triple dashes (`---`) is the text from the show notes. Questions and links mostly.


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

What happens here is that I already extracted the necessary data using a bunch of smaller methods and feed it to this `compose_markdown` method through an options hash.




<!--
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

def build_tags(title, interviewee)
  extracted_tags = strip_pipes(title)
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

def clean_date(episode_subtitle)
  string_date = /[^|]*([,])(.....)/.match(episode_subtitle).to_s
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
-->
