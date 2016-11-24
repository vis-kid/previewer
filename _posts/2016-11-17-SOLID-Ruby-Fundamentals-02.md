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
+ Code Example

# OCP

The “Open/Closed Principle” simply states that you should be able to extend a class’s behavior without modifying it. Open for extension, closed for modification. At closer distance, this means that a class or module should be cool with extension but avoid modification. To some extend this can be useful for functions as well.

Why, what’s the beef with modification? Ideally, when you put a class into production, you don’t want to change it. We want to avoid taking chances of introducing bugs. The same goes for other clients who rely on our work. Changing existing code frequently endangers possibly such relationships. Changing behaviour of a class that clients rely on could impact all of their clients as well. The ripple effects can go far beyond your code. Not good!

What about bugs? Are we allowed to bug fix without adhering to OCP? Of course, that is always ok. In that case you have to change the class. I guess it’s fair to say that this principle at its core wants to manage change—at least the rate of it. Since this is a constant companion in our work, it is important to keep it in check where we can. Single responsibilities are one thing, not needing to modify them is a different story though. In practice, both SRP and OCP seem to have symbiotic existences. OCP often helps with attaining SRP

Changing the behavior is O.K., extending the behavior is O.K. We just want to be able to do that without changing existing code. That is the trick—as well as the tricky part. How can we have classes that stay the same? We achieve this with abstraction. We’re not living in a perfect world where we can always achieve OCP. It would be an idealistic scenario that you probably can’t achieve completely. Code that never changes is not on the table.

# Composition over inheritance

OCP was originally thought of in the context of inheritance. But we learned that composition over inheritance is the way to go. Both have their place but if you have the opportunity, prefer composing objects over using inheritance. Both approaches are valuable and can achieve similar goals. Inheritance is a great strategy for designing objects that are specializations of others. In general, you will be able to achieve better results in regards to coupling and cohesion. I hope I could already clarify why coupling is an important issue to address aggressively when you refactor code. Cohesion might be a little more vague for beginners.

Classes have better focus if they don’t mix concerns. If  an object is solely focused on extracting content, its cohesion is stronger than say a class that is also busy with composing markdown files. Strong cohesion reduces the chances of changing existing code because we separated concerns—basically, split up responsibilities. A single responsibility is the strongest cohesion you can get in a class.

When you compose objects, you have a lot more freedom to break up the functionality into discrete, encapsulated objects that are less dependent of each other. Inheritance can introduce a lot more “garbage” down the line. If objects inherit from various ancestors, the responsibilities can pile up quickly. The encapsulation between parent objects and their inheritors is not exactly strong.

The biggest downside to composition is a matter of context that you need to load in your brain. Composition is easily plagued by indirection. That simply means that the components in play can end up being all over the place and can seem harder to put together. That kind of specialized encapsulation will lead to more objects that we need to keep track of. Sometimes, the cost of indirection might be too big. Using inheritance instead can be a good compromise. I think a good guideline is to reach for composition first and only go for inheritance solutions if necessary. All that being said, increased indirection is always a side effect when you work towards OCP.

# Dependency Injection

Through DI, we are feeding a class new behavior without changing it. A dependency can be easily injected within the constructor. We pass an object into the targeted class without letting it know what this object is all about. This technique allows us to extend already existing classes without modifying them. Since this is a frequent strategy to follow OCP, I thought we should give it a quick look. It’s also useful in terms of keeping objects simple and adhering to SRP.  

Injecting dependencies sounds like a mouthful, I know. We inject behavior into classes without creating tight coupling between them. In fact, the less the injected class knows about other classes, the better off the all are. Ignorance is bliss in Object Oriented Design—maybe. Or “in part”, as politicians like to spin their arguments. Since this seems to be a lofty goal to achieve, I don’t want to propagate it as a rule of sorts.

It’s pretty clear that if a class uses the behavior of another class, the code is more brittle due to coupling. What we also reduce the reuseability with stronger dependencies. We want to make this connection as small and as flexible as possible. Smarter coupling makes mocking code dependencies in our tests a lot easier as well.

When we need to refer to another class through its name directly, we create a strong relationship, a stronger dependency between them. If you change the name of the class or its API, things break more easily. The amount of breakage is something we can try to limit with OCP in mind.


# Code Example

Refactoring with OCP in mind should make the code simpler and less coupled. The logic should be more focused in each class. We will instantiate the objects in play at the highest layer—instead of all over the place. Classes will know a lot less about each other and will end up being more flexible to face changes. 

Below is an example from the code of the previous article. As I mentioned at the end of that article, it wasn’t done in terms of refactoring. I hope it was clear that the previous refactorings targeted the “Single Responsibility Principle” first and foremost. I left the code in that condition because it was still not addressing the Open/Closed Principle in a few places.

Please take another look and view it through the lens of what you have learned so far about being open for extension and closed for modification. There are definitely some issues here that make the code unnecessarily brittle. Think about how we can use dependency injection to decouple the affected classes.

## Old Podcast Scraper Code

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

## Injected Dependencies

Let’s dig in! For example, `MarkdownComposer` knows too much of another class, `PageExtractor`. Unnecessarily so. It knows the class’ name, the method `extract_data` and its argument `detail_page` for instantiation. `MarkdownCompoer` can and should be more ignorant of these. Sure, it needs to know a little bit for using the behavior of another objects. We can limit the extend though. Knowing the details about the `detail_page_link` is  also not exactly the business of a class that is dealing with markdown.

When we inject dependencies we avoid having that affected object creating these dependencies themselves. It’s like outsourcing that responsibility. As a result you have more flexibility and less coupling. But for our purposes here, also fewer reasons to change due to less coupling. Let’s go through the major changes step by step and talk about the improvements we can make.

## MarkdownComposer -> TextComposer

#### class MarkdownComposer

``` ruby

class MarkdownComposer

  attr_reader :extracted_data

  def initialize(detail_page_link)
    @extracted_data ||= PageExtractor.new(detail_page_link).extract_data
  end

  ...

```

#### class MarkdownComposer Refactored

``` ruby

class TextComposer

  attr_reader :page_extractor

  def initialize(page_extractor)
    @page_extractor = page_extractor
  end

  ...

```

The most obvious change is that the class name is now different. I figured it would be more flexible if I leave the door open for extension. Other classes might benefit as well. If I need to add the ability to compose `.html` or `.txt` files, I already have a home for them.

It’s easy to see that the refactored `TextComposer` is now kept in the dark about the object in charge of extracting data. Now we could easily change the name of  `PageExtractor`, or even swap it out completely with something different.

Of course we have one dependency left in here that we should talk about. With the `page_extractor` object ready to extract inside the `TextComposer` class, it needs to do its business. That means it needs to use the API of the `PageExtractor` class at some point. That dependency stays. Therefore, if we change the method calls we might break the class that was injected with it.

I hope it’s clear why this dependency is necessary and how it already improved the level of fragility in here. Also, it hopefully illustrated how change can ripple through an application and why we would rather opt for extensions than changes to existing code. It’s hard to predict how a little change can ripple through your codebase. Better err on the side of sanity and safety and minimize the risk of tight coupling as much as we can. 

`TextComposer` is now also significantly simpler. We don’t need to fumble around with the options hash of extracted data. The API is also less smelly. Oh, yes and I decided to just request `markdown` without the compose part when I got rid of `compose_markdown`. It only requested a private method and was not as much needed as I thought in the beginning. Now we have a `markdown` method in charge for that kind of text and it would be easy to add `txt` or `html` methods without changing existing code. OCP!

#### before

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

#### after

``` ruby

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

``` 

A nice improvement I think. Reads better, feels cleaner and doesn’t need an argument. If I would need to edit the file I wanna compose, say I extract additional data, I would need to open `markdown`. In such a case, I don’t see a way around modifying existing code without introducing irrational complexity. Other than that, the overall class is small, ignorant and focused.

#### class TextComposer

``` ruby

class TextComposer

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

```

## FilenameComposer

Let’s have a look at the next class. `FilenameComposer` has the same issues as the previous class. Also, both classes’ initializers are kinda overly motivated. They instantiate an instance of `PageExtractor` and feed it a link acquired from the `Scraper` class. On top of that, they also `extract_data` right away. Ok, not super bad for a first attempt, but this code is nowhere near production ready. This is a good example how refactoring is a never-ending sandblasting process that often involves multiple steps and attempts.

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

We can use the same refactoring as in `TextComposer`. This little change alone made the code not only simpler but more flexible along the way. The initializers became ignorant and straightforward. Nothing to trip over really.

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

So far so good. The dependency issue that is left, is an API issue. `FilenameComposer` needs the

+ `date`
+ `interviewee`
+ and `episode_number`

of the scraped episode from `PageExtractor` at some point. Nothing easier than that. Instead of extracting this data from a hash that is temporarily parked in an instance variable, it simply uses the injected object and asks for the data needed. Reads nicer too.

#### before

``` ruby

  ...

  private

  def date
    extracted_data[:date]
  end

  def interviewee
    dasherize(extracted_data[:interviewee])
  end

  def episode_number
    extracted_data[:episode_number]
  end

  ...

```

#### after

``` ruby

  ...

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

  ...

```

The coupling of these three methods on `page_extractor` is something we need to accept. On the flip side, both composer classes are now a lot more ignorant about `PageExtractor`. They don’t know its class name and what argument it needs for instantiation. Two degrees of coupling are gone with only one left. Good trade off! 

We can also simplify the way we compose the final file name.

#### before

``` ruby

 ...

 def prepare_filename
    compose_filename
  end

  private

  def compose_filename
    "#{date}-#{interviewee}-#{episode_number}.html.erb.md" 
  end

  ...

```

#### after

``` ruby

 ...

  def md_filename
    "#{date}-#{interviewee}-#{episode_number}.html.erb.md" 
  end

  ...

```

```md_filename``` is not private anymore and is much more specific. `compose_filename` can mean just about anything. If we need to add another method for composing HTML file names, it would be as easy as adding something like `html_filename`. Also, I removed `prepare_filename` since it was an intermediary method that lost its purpose.

## PageWriter

The `initialize` method of `PageWriter` is smelly as well. We instantiate it with two objects and feed it another link the class itself has no business of knowing about. I have more to say about passing this link around for scraping, but first things first. What I like the least about the scenario here is that if we need additional data from either of these injected objects, we would need to modify the `initialize` method.

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

We can encapsulate the knowledge of the particular link to scrape within `PageExtractor` alone. Then we only need to pass interchangeable objects into the initializer here. No need to play with fire here. Even before the refactoring, `PageWriter` itself is a simple class with a simple task. It does not really need to have all these hard ties. We keep this class only around to focus on writing out the final files after we scraped and composed all the data somewhere else.

#### class PageWriter Refactored

``` ruby

class PageWriter

  attr_reader :text_composer, :filename_composer

  def initialize(text_composer, filename_composer)
    @text_composer = text_composer
    @filename_composer = filename_composer
  end

  ...

```

It is now a lot more ignorant about its dependencies. For example, if we want to extend that class by making it write `.txt` or `.html` files, we don’t need to change it. The injected `TextComposer` and `FileNameComposer` should then provide all the knowledge to write out whatever new file name or text. We leave no opportunity to break it for that matter. We can let the injected objects be in charge of dealing with different filenames and what not—encapsulated in new methods of course.

O.K., back to my rant about the `detail_page_link` that was passed around too much. Our little refactoring made it easier to only provide `PageExtractor` with that information. In fact nothing changed in that class in that regard but we could eliminate its references in three places:

+ `TextComposer`
+ `FilenameComposer`
+ `PageWriter`

That might not look like a big win but overall, this little change reduced a pretty sneaky dependency that previously trickled through the whole scraper. This was possibly the smelliest part we needed to improve. It was passed around too much. Adhering to the “Single Responsibility Principle” was not enough.

After injecting `PageExtractor` in a couple of classes, we could not only strengthen the responsibility aspect, we could encapsulate this parameter in one place where it is actually put to work. By injecting responsibilities, we could encapsulate this parameter only where it is instantiated. That means less duplication as well. You can see that applying sane coding principles also create nice side effects along the way. The feed off of each other—the same goes for the opposite as well of course.

Naming methods adequately can play a significant part towards improved OCP as well. If you name and build methods to have a sharp focus, they will less likely fall victim to change as well. For example, `write_page` or `write_markdown_page` can make a critical difference if you decide to add the behaviour of writing HTML pages later on. Chances are good that we would need to touch `write_page` in that scenario—at the very least we would need to rename it in order to easily differentiate it from the `write_html_page`. Even the names for `file_name` and `text` will turn out to be problematic. What file name and what text are we exactly talking about?

#### before

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

```

#### after

``` ruby

class PageWriter

  attr_reader :text_composer, :filename_composer

  def initialize(text_composer, filename_composer)
    @text_composer = text_composer
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
    text_composer.markdown
  end
end


```

Maybe you asked yourself why `PageWriter` now uses two new private methods. Simple, I decided it would scale better if I rename `write_page` into `write_markdown_page`.

+ markdown_filename
+ markdown_text

Proper method naming can also play into the whole closed for modification theme. If I wanna use `PageWriter` to not only write out markdown files but say, `txt` or `html` files, I just need to extend the code without touching existing methods. The arguments for `write_markdown_page` are now more specific as well.

`PageWriter` is set up to easily write out new pages and hide the functionality behind specialized private methods for possibly new file names and other forms of text. The same goes for `md_filename` and `markdown` methods. If I need a different kind of filename or a different blueprint for text, I simply extend the injected classes. Long story short, proper naming is hard but worth the extra effort.





## PageExtractor

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

Along the way, I could delete and simplify a few bits that were harder to do before the refactoring. For exmaple, `PageExtractor` is not returning a hash with the extracted data. Instead, we let the injected `page_extractor` handle the extraction business more fine grained. For example, if I would have decided to extract more data than the hash provided, I would have had one more reason to change the class. Now, this class only handles the extraction logic for every piece of data separately and is easy to extend.

It saves the state of each page which can then be passed into the injected classes to do their bidding. The method `extract_data` is completely gone because it was not as future proof if I wanted to scrape additional data. The API used to get that data in other classes was also a bit iffy. Each method is focused on one extraction job only. We left little reason to change but opened every opportunity to extend the code easily.

#### class PageExtractor refactored

``` ruby

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

```





## Scraper

Where does this lead to? Where are all these injected objects ending up? Good question. I decided to put them high up in the food chain so to speak. These injected objects kinda bubble up and often enable you to make changes in one or a few places—instead of all over the place.

#### Before

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

`Scraper` is now in charge of instantiating these objects and injects them into their respective targets. Here we build up all the objects that are needed to write the markdown page. We sort of feed them into each other, bottom-up. If we need to make changes, we can do that in one central place—in contrast to doing it in multiple places at once.


#### After

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

Instead of passing each page link to `PageWriter`, we directly pass it to the class who scrapes the data from the page. Nobody else needs to know about it. `PageExtractor` takes the link, clicks it, saves its state, follows it to the shownotes and provides `MarkdownComposer` and `FileComposer` all the abilities to prepare the markdown text and the filename from that particular page. In turn, `PageWriter` get injected with the former two which are needed to write out the new page containing the extracted data.


## Complete Scraper After Dependency Injection

I did a couple of other tweaks while shuffling the code around but I don’t think they are worth mentioning here. Take a look at the complete code after dependency injection. I think we made some good progress in terms of the ”Open/Closed Principle”. The code is better equipped to deal with future changes. OCP helped to tighten the responsibilities we tried to isolate using the “Single Responsibility Principle”. The code below hopefully shows that both principles have a symbiotic thing going on.

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

class TextComposer

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

  attr_reader :text_composer, :filename_composer

  def initialize(text_composer, filename_composer)
    @text_composer = text_composer
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
    text_composer.markdown
  end
end

class Scraper

  def scrape
    link_range = 1
    agent ||= Mechanize.new

    #until link_range == 21
    until link_range == 2
      page = agent.get("https://between-screens.herokuapp.com/?page=#{link_range}")
      link_range += 1

      page.links[2..8].map do |link|
        page_extractor    = PageExtractor.new(link)
        text_composer = TextComposer.new(page_extractor)
        filename_composer = FilenameComposer.new(page_extractor)

        PageWriter.new(text_composer, filename_composer).write_markdown_page
      end
    end
  end
end

Scraper.new.scrape

```







The class is not as ignorant as it could be. it knows about pageextractor.

We have a strong link here.

What is obvious above, is that if we change the name of the  PageExtractor class or change the extract_data method, we need to come here as well. 

in the new methods, MarkdownComposer only knows that the injected object responds to the `extract_data` methods???

It allows us to only use the interface of an injected dependency but not coupled to the creation of ...

