nice touch from Haml:

``` haml

 %meta{:content => "text/html; charset=UTF-8", "http-equiv" => "Content-Type"}/

```

Does this get created automatically by Haml?

## Getting started

#### Sinatra how to section?

gem install slim and then require it?? Think documentation can be more specific to where it needs to be required.

In the documentation, after having installed the gem, maybe add a note about generating .slim files automatically when you generate controllers and scaffolds in Rails.

Since most users are using Slim probably with Rails, it might be a better idea to go though this installation process in Getting Started.

slim-rails github page:
??? From the version 0.2.0, there is no need to include gem "slim" in your Gemfile.
-> Isn’t that a good idea for the Gemfile.lock file

#### With Rails


## slimrb

slimrb section is broken in the documentation on rubydoc.info. On Github is seems to be in order

## Dynamic Content

?? Maybe later in the documentation??
?? A bit more explanation, that this is the way to inject Ruby into Slim templates??
?? Is dynamic content necessary when we have an “Output =” section under “Line Indicators”??

## Output section
#Merged !! Typo
Equals sign not equal sign

## Line indicators

## Text Interpolation
?? Maybe bring this section up to the Output section
Is related I think and therefore a good idea to keep close??

?? Better wording about adding a backslash for escaping text interpolation
To escape the interpolation (i.e. render as is)
->
To escape the interpolation (i.e. render as is) simply prepend a backslash before `#{code}`

## Embedded engines (Markdown, ...)
## Merged!! PR
?? Thanks to Tilt, Slim has impressive support for embedding other template engines.
Lets sound less self-congratulatory here:
?? Thanks to Tilt, Slim has !!!extensive!!! support for embedding other template engines.

## Framework Support
??Maybe put the section up next to get started??

## Testing Benchmarks
!! link for newest benchmarking results in -> This is not an active repository Site

## Contributing
!!f you'd like to help improve Slim, clone the project with Git by running:

## Configuring Slim 
!! This section is hard to understand
When Minad comes to Fuerte, we can maybe go over it together after he explained the various options to me
?? Shouldn’t it say clone the project

#### Verbatim text
The pipe tells Slim to just copy the line. 
?? Maybe being more clear about this??
?? Why can’t it be on the same line as the parapgraph tag??
?? why does the following lines need to be indented further??

## HTML tags
## PR 
## Merged!
Error! <!DOCTYPE html> is not an HTML tag!!

?? Maybe start the documentation about the syntax with HTML tags??

Continue with ID shortcut # and class shortcut .
Easier for beginners to get into that. Also, sufficient to play with the language and see its benefits right away.

## PR
Typo!!!
Splat attributes *
The splat shortcut allows you !!! turn !!! a hash in to attribute/value pairs

## Output consistency
## PR
“renders as”
“This renders as”
-> “This renders as:” or “Output:”


Rearrange this code example to use 'data-id'=>place.id before 'data-url...:
Is more consitent with the rendered example where data-id comes second
.card*{'data-url'=>place_path(place), 'data-id'=>place.id} = place.name

## Configure Slim
at available options
??is this wrong for :code_attr_delims and :attr_list_delims??
=> ')', '[' => ']', '{' => ''}

Version on GitHub pages is fine, rubydoc version is displaying it incorrectly


!! Too early in the documentation
Trailing and leading whitespace (<, >)
More clarity in this section, maybe an example of processed HTML code

!!
Inline tags
Explain inline tags better. What are they doing exactly and show the output
Output maybe via a javascript slide-down section??

!!! Doc hasn’t covered wrapping attributes yet!!!
“For readability, don't forget you can wrap the attributes.”

Quoted attributes??
?? You can use text interpolation in the quoted attributes:??
YOu mean in double quoted text??

Ruby attributes
?? Mistake??
That code example 
a href=(path_to_user user) = user.name
should look likt this?? No whitespace between = and user.name??
 a href=(path_to_user user) =user.name

text interpolation should be called string interpolation??

#### Generator

``` bash

rails g controller Whatever index

```

Can I get an index.slim file without the .html part? -> no index.html.slim


Question about text interpolation:
Is that bad practice??

h2 2nd Agents
ul
  - @agents.each do |agent|
    li.agent
      div Name:            #{agent.name}
      div Number:          #{agent.number}
      div Licence to kill: #{ agent.licence_to_kill}

or is this better?? Performance??

h2
  | Agents
ul
  - @agents.each do |agent|
    li.agent
      div
        | Name: 
        =            agent.name
      div
        | Number: 
        =          agent.number
      div
        | Licence to kill: 
        = agent.licence_to_kill
      br


## Inline HTML
??Bug with inline html, Markup gets rendered differently than using pure Slim. Indentation bugs??
Works fine in the browser, at least in Chrome. 
Slim2HTML converter might have the problem but not chrome so no bug??

## Verbatim Text
Maybe through in the phrase “word-for-word” to not make developers with intermediate English skills look up verbatim. Word for word is easier to understand immediately.

?? Error in Documentation??
No need for indentation or pipe character to go multiline. the rendered Markup differs but it looks the same on the page without indentation and pipe symbol.??

The indentation part is unclear

## Shortcuts
Pull Request Merged: Where do I set the shortcut. Documentation is missing that info

?? Does Haml offer something similar to these shortcut customizations in Slim?? If not, mention it in the documentation. 

Put the custom shortcuts section after the ```.``` and ```#``` shortcuts section
?? Does it belong in config/initializers/slim.rb??
Where do I put it in Sinatra??
