---
layout: post
title: SOLID Ruby Fundamentals 02-Open/Closed-Principle
date: 2016-11-17 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Mechanic, Nokogiri, Ruby]
---

# Topics

+ OCP
+ Open/Closed?
+ Composition over inheritance
+ Dependency Injection
+ Refactor Code Example

??
+ Composite?
+ Decorator?

# OCP

The “Open/Closed Principle” simply states that you should be able to extend a class’s behavior without modifying it. Open for extension, closed for modification. At closer distance, this means that a class or module should be cool with extension but avoid modification. To some extend this can be useful for functions as well.

Why, what’s the beef with modification? Ideally, when you put a class into production, you don’t want to change it. We want to avoid taking chances of introducing bugs. The same goes for other clients who rely on our work. Changing existing code frequently endangers possibly such relationships. Changing behaviour of a class that clients rely on could impact all of their clients as well. The ripple effects can go far beyond your code. Not good!

What about bugs? Are we allowed to bug fix without adhering to OCP? Of course, that is always ok. In that case you have to change the class. I guess it’s fair to say that this principle at its core wants to manage change—at least the rate of it. Since this is a constant companion in our work, it is important to keep it in check where we can. Single responsibilities are one thing, not needing to modify them is a different story though. In practice, both SRP and OCP seem to have symbiotic existences. OCP often helps with attaining SRP

# Composition over inheritance

OCP was originally thought of in the context of inheritance. But we learned that composition over inheritance is the way to go.

Extension can redefine parts of the behaviour of a class.

subclassing. dont affect anybody else that way. you dont need to overwrite functionality

prefer subclassing over reopening classes


Extend them via child classes and inheritance






It reduces churn??

similar to immutable datasstructures??

Code that never changes

change the behavior is ok, extend the behavior is ok. We just want to be able to do that without changing existing code.

The downside to ocp is indirection

Idealistic scenario that you probably can’t achieve completely.

The classes stay the same. we can achieve this with abstraction.




# Dependency Injection

The use of dependency injection is pretty typical strategy to achieve OCP. This technique allows us to extend already existing classes without modifying them. Since this is a frequent strategy to follow OCP, I thought we should give it a quick look. It’s also useful in terms of keeping objects simple and adhering to SRP.  

Injecting dependencies sounds like a mouthful, I know. Maybe its not a bad analogy to a vaccination. We inject behavior into classes without creating tight coupling between them. In fact, the less the injected class knows about other classes, the better off the all are. Ignorance is bliss in Object Oriented Design—maybe. Or “in part”, as politicians like to spin their arguments. Since this seems to be a lofty goal to achieve, I don’t want to propagate it as a rule of sorts.

What is pretty clear though is that if a class uses the behavior of another, the code is more brittle due to tight coupling. If you change the name of the class or its API, things break. The amount of breakage is something we can try to limit though.

Below is an example from the code in the previous article. As I addressed at the end of that article, it was not done in terms of refactoring. I hope that was clear that the refactorings targeted the Single Responsibility Principle first and foremost. I left the code in that condition because it was still not addressing the Open/Closed Principle in a few places.

Please take a look and view it through the lens of what you have learned so far about being open for extension and closed for modification. There are definitely some issues here that make the code brittle. Think about how we can use dependency injection to decouple the affected classes.

## Old Podcast Scraper Code

``` ruby

require 'Mechanize'
require 'Pry'
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

## Injected Dependencies

Let’s dig in! For example, `MarkdownComposer` knows too much of another class, `PageExtractor`. Unnecessarily so. It knows the class’ name, a method `extract_data` and its argument `detail_page` for instantiation. `MarkdownCompoer` can and should be more ignorant of these. Sure, it needs to know a little bit if it uses the behavior of another objects, but we can limit the extend. Knowing the details about the `detail_page_link` is maybe also not the business of dealing with markdown.


#### class MarkdownComposer

``` ruby

class MarkdownComposer

  attr_reader :extracted_data

  def initialize(detail_page_link)
    @extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

```

It’s easy to see that the refactored `MarkdownComposer` is now kept in the dark about the object in charge of extracting data. Now we could easily change the `PageExtractor` object, or even swap it out completely with something different.


Naming can play a big part in that as well. If you name methods that are focused they can fall less victim to change. For example, `write_page` or `write_markdown_page` can make a significant difference if you decide to add the behaviour of writing HTML pages as well. Chances are good that we would need to touch `write_page` now—at the very least rename it to easily differentiate it from the `write_html_page`.


#### class MarkdownComposer Refactored

``` ruby

class MarkdownComposer

  attr_reader :extracted_data

  def initialize(page_extractor)
    @page_extractor = page_extractor
  end

  ...

```


 We have one dependency left in here that we should talk about. With the `page_extractor` object ready to extract inside the `MarkdownComposer class, it needs to do its business. That means it needs to use the API of the `PageExtractor` class at some point. That dependency stays. Therefore, if we change the method calls we might break the class that were injected with it. I hope it is clear why this is on the one hand necessary but how it already improved the level of fragility in play here. Also, I hope it illustrated how change can ripple through an application and why we would rather opt for extensions than changes to existing code. It’s hard to predict how a little change can ripple through your codebase. Better err on the side of sanity and safety. 

Let’s have a look at the next class to illustrate this through an actual example. `FilenameComposer` has the same issues as the previous class. Also, both classes’ initializers are kinda overly motivated. They instantiate an instance of `PageExtractor`, feed it a link acquired from the `Scraper` class and on top of that, the also `extract_data`. Ok, not super bad for a first attempt, but this code is nowhere near production ready.

#### class FilenameComposer

``` ruby

class FilenameComposer
  include ExtractionUtilities

  attr_reader :extracted_data

  def initialize(detail_page_link)
    @extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  ...

```

We can use the same refactoring as in `MarkdownCompower`. That change alone made the whole code example not only simpler but more flexible along the way. The initializers became ignorant and straightforward. Nothing to trip over really.

#### class FilenameComposer Refactored

``` ruby

class FilenameComposer
  include ExtractionUtilities

  attr_reader :page_extractor

  def initialize(page_extractor)
    @page_extractor = page_extractor
  end

  ...

```

The dependency issue that is left, is the API issue I mentioned above. Let’s look at an example. `FilenameComposer` needs the `date`, `interviewee` and `episode_number` of the scraped episode at some point. Nothing easier than that, it simply uses the injected object and asks for the data.

#### class FilenameComposer Refactored

``` ruby

...

  def date
    page_extractor.date
  end

  def interviewee
    dasherize(page_extractor.interviewee)
  end

  def episode_number
    page_extractor.episode_number
  end

...

```

The coupling of these three methods on `page_extractor` is something we need to accept. On the flip side, these composer classes are now a lot more ignorant about `PageExtractor`. They don’t know its class name and what argument it needs for instantiation. Two degrees of coupling are gone with only one left. Good trade off. 


















When you inject dependencies you avoid having that object creating these dependencies themselves. It’s like outsourcing that responsibility??? As a result you have more flexibility and less coupling. But for our purposes here, also fewer reasons to change due to less coupling.


The `initialize` method of `PageWriter` is smelly as well. We instantiate it with two objects and feed it another link the class itself has no business of knowing about. I have more to say about passing this link around for scraping, but first things first.

#### class PageWriter

``` ruby

class PageWriter

  attr_reader :text, :file_name

  def initialize(detail_page_link)
   @text          = MarkdownComposer.new(detail_page_link).prepare_markdown
   @file_name     = FilenameComposer.new(detail_page_link).prepare_filename
  end

  ...

```

We could scrub the knowledge of the particular link to scrape and only pass interchangeable objects into its initializer. `PageWriter` itself is a simple class with a simple task. It does not really need to have a hard ties. We just keep it around to take care of writing out the final end product after we scraped and composed all the data.

#### class PageWriter Refactored

``` ruby

class PageWriter

  attr_reader :markdown_composer, :filename_composer

  def initialize(markdown_composer, filename_composer)
    @markdown_composer = markdown_composer
    @filename_composer = filename_composer
  end

  ...

```

It is completely ignorant about its dependencies. For example, if we want to extend that class by making it write `.txt` or `.html` files, we don’t need to change it. No opportunity to break it for that matter. We can let the injected objects be in charge of dealing with different filenames and what not—encapsulated in new methods of course.

O.K., back to my rant about the `detail_page_link` that was passed around too much. Our little refactoring made it easier to only provide `PageExtractor` with that information. In fact nothing changed in that class in that regard but we could eliminate its references in three places:

+ `MarkdownComposer`
+ `FilenameComposer`
+ `PageWriter`

That might not look like a big win but overall, this little change reduced a pretty sneaky dependency that previously trickled through the whole scraper. It was passed around too much. Adhering to the “Single Responsibility Principle” was not enough. But after injecting `PageExtractor` in a couple of classes, we could not only strengthen the responsibility aspect, we could encapsulate this parameter in one place where it is actually put to work. By injecting responsibilities, we could encapsulate this parameter where it is instantiated. That means less duplication as well. You can see that applying sane coding principles also create nice byproducts along the way. The feed off of each other—the same goes for the opposite as well of course.

Where does this lead to? Where are all these injected objects ending up? Good question. I decided to put them high up in the food chain so to speak. These injected objects kinda bubble up and often enable you to make changes in one or a few places—instead of all over the place. In our case, `Scraper` is now in charge of instantiating these objects injects them as well. If we need to make changes, we can do that in one central place—in contrast to doing it in multiple places at once.

#### class Scraper Refactored

``` ruby

class Scraper

  def scrape
    link_range = 1
    agent ||= Mechanize.new

    until link_range == 21
      page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")
      link_range += 1

      page.links[2..8].map do |link|
        page_extractor    = PageExtractor.new(link)
        markdown_composer = MarkdownComposer.new(page_extractor)
        filename_composer = FilenameComposer.new(page_extractor)

        PageWriter.new(markdown_composer, filename_composer).write_markdown_page
      end
    end
  end
end

```

Here we build up all the object that are needed to write the markdown page. We sort of feed them into each other, bottom-up.


#### class PageExtractor

``` ruby

class PageExtractor
  attr_reader :detail_page
  include ExtractionUtilities

  def initialize(detail_page_link)
    @detail_page ||= detail_page_link.click
  end

  ...

```



The class is not as ignorant as it could be. it knows about pageextractor.

We have a strong link here.

What is obvious above, is that if we change the name of the  PageExtractor class or change the extract_data method, we need to come here as well. 

in the new methods, MarkdownComposer only knows that the injected object responds to the `extract_data` methods???

It allows us to only use the interface of an injected dependency but not coupled to the creation of ...

A dependency can be easily injected within the constructor. We pass an object into the targeted class without letting it know what this object is all about.

Mocking code dependencies in our tests becomes easier with DI as well.

Through DI, we are feeding a class new behavior without changing it.

When we need to refer to another class through its name, we create a stronger relationship, a stronger dependency between them.

What we also reduce is reuseability with these stronger dependencies
























## After Dependency Injection

``` ruby

require 'Mechanize'
require 'Pry'
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
  include ExtractionUtilities

  attr_reader :detail_page

  def initialize(detail_page_link)
    @detail_page ||= detail_page_link.click
  end

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

  private

  def subtitle
    subheader_selector = ".episode_sub_title"
    detail_page.search(subheader_selector).text
  end
end

class MarkdownComposer

  attr_reader :page_extractor

  def initialize(page_extractor)
    @page_extractor = page_extractor
  end

  def markdown
<<-HEREDOC
--- 
title: #{page_extractor.interviewee}
interviewee: #{page_extractor.interviewee}
topic_list: #{page_extractor.title}
tags: #{page_extractor.tags}
soundcloud_id: #{page_extractor.soundcloud_id}
date: #{page_extractor.date}
episode_number: #{page_extractor.episode_number}
---

#{page_extractor.shownotes_text}
HEREDOC
  end
end

class FilenameComposer
  include ExtractionUtilities

  attr_reader :page_extractor

  def initialize(page_extractor)
    @page_extractor = page_extractor
  end

  def md_filename
    "#{date}-#{interviewee}-#{episode_number}.html.erb.md" 
  end

  private

  def date
    page_extractor.date
  end

  def interviewee
    dasherize(page_extractor.interviewee)
  end

  def episode_number
    page_extractor.episode_number
  end
end

class PageWriter

  attr_reader :markdown_composer, :filename_composer

  def initialize(markdown_composer, filename_composer)
    @markdown_composer = markdown_composer
    @filename_composer = filename_composer
  end

  def write_markdown_page
    File.open(markdown_filename, 'w') { |file| file.write(markdown_text) }
  end

  private

  def markdown_filename
    filename_composer.md_filename
  end

  def markdown_text
    markdown_composer.markdown
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
        page_extractor    = PageExtractor.new(link)
        markdown_composer = MarkdownComposer.new(page_extractor)
        filename_composer = FilenameComposer.new(page_extractor)

        PageWriter.new(markdown_composer, filename_composer).write_markdown_page
      end
    end
  end
end

Scraper.new.scrape

```

