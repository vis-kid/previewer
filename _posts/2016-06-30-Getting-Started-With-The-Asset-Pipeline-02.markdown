---
layout: post
title: Getting Started With The Asset Pipeline 02
date: 2016-06-30 04:29:10 +0100
comments: true
sharing: true
published: true 
categories: [Rails, RSpec, Ruby, Ruby on Rails, Asset Pipeline, Sass, CSS, JS, JavaScript]
---

## Topics

+ Sprockets?
+ Precompiling Assets
+ MD5 Fingerprinting
+ Asset Links
+ Pimped Styles

## Sprockets?

The Asset Pipeline in Rails is handled by the `sprockets-rails` gem. By default, it is enabled when you create a new Rails application. Sprockets is a Ruby library and helps you manage your JavaScript and CSS assets. On top of that, it takes care compiling as well as preprocessing higher-level languages like Sass, SCSS and CoffeeScript. Preprocessing simply means that your styles, HTML or JavaScript get generated after you wrote code in another language. For example your Sass stylesheets get precompiled into valid CSS. Since browsers can’t comprehend stuff like CoffeeScript we have to preprocess it first.

There are three gems that play a vital role in all of this:

+ `coffee-rails`
+ `uglifier`
+ `sass-rails`

`sass-rails` let’s you write your Sass-flavor of choice for creating your CSS, `coffee-rails` gives you a nicer syntax for writing you JavaScript and `uglifier` in turn compresses these assets. Btw, `sass-rails` is in charge for compressing CSS files itself. All three gems will be added to your Gemfile automatically when you create a new Rails app. If you have a reason to not use Sprockets with Rails you can skip it from the command line when you initiate your app:

#### Terminal

``` ruby

rails new appname --skip-sprockets

```

This command prevents adding the above mentioned gems to your Gemfile and give you a different version of the `config/application.rb` file. Now you need to set up your own solution for handling your assets.

### Processing

When you preprocess something like CoffeeScript into valid JS, or Sass into CSS, you also have the option to process these files some more. Minfication and compression are the two big ones. Minification gets rid of stuff the browser does not care about—white space and comments are good candidates for example. Sass can also deal directly with minification as well. Or you simply use the Rails default of the YUI compressor when dealing with stylesheets. Just as an aside, Rails lets you even write and configure your own custom compressor—just sayin’.

I should mention that Sass has a compressed version of outputting styles. This is different actually confusing since it is minifying your stylesheets. Compression is a different process and reduces your files size significantly more. Compression, or gzipping, targets repetive bits of the code and replaces them with pointers that take up less space. So the more repetetive your assets files, the more it be compressed and reduced in size. Minification is quite nice but you will see the biggest filesize reductions when you use gzipping. It’s not rare to see files shrink to 14% or their original size if you both minify and compress them. When you think about downloading tons of assets over slower networks, this can be of tremendous benefit.

## Precompiling Assets

For production, your assets need to be compiled first. Files that you place in `app/assets` usually need to be preprocessed before they can be served. What are we talking about here? Let’s say you have worked one some new app that works on your local server. Your Sass files and your CoffeeScript flavored JavaScript works magically out of the box. Cool, but what happens if you wanna push this to a production server?

Such a server, responsible for delivering this content to a possibly much larger audience, has a few different demands than your local server.
As a default, Rails will look for files that are named `application` and try to precompile them for you. In this step, assets are compiled from high-level languages like Sass into CSS, they are concatenated together—from multiple files into fewer bundles of files.
Having as few files as possible is beneficial for performance and speed. Compressing their size to their bare minimum is also of tremendous importance—especially for bigger applications. On top of all that, files are also cached. That means you only load new assets when they have been changed on your end.

### Compilation

You have two options where you  want to compile your assets. You compile on your production server or locally. Local compilation means that you execute this process on your own machine first and then push it to production. This has the advantages that you don’t need write access to the file system on a production server and if you deploy to multiple servers you can do this process only once. Not needing to precompile your assets on the server if deployed changes do not include asset changes is another benefit of precompiling locally.

Precompiling assets is one of these things we need to do before we send them our “pure”, compiled CSS, HTML and JS. Servers should probably not need to know about how to deal with high-level languages like Sass, Slim and whatnot. They have enough responsibilities already. For that to work, you are responsible of having the compression and minifying gems ready on your machine. You can put these compiled assets in your Git repo—or whatever version control tool you prefer—and deploy only these final assets to production.

Rails offers you a Rake task that takes care of precompiling assets though. Such a task is simply a series of predefined steps that are executed in order to achieve a specific goal. Using the Rake build tool for such things is very common in Ruby land. You can easily write your own tasks in Rake yourself. Rails makes this very easy. Out of the box, Rails comes with the `lib/tasks` directory where you can conveniently park your Rake tasks. No further setup necessary. So when you run:

#### Terminal

``` bash

rake assets:precompile

or

bundle exec rake assets:precompile

```

Sprockets will take the assets it can find in its search path and preprocesses / compiles them into `public/assets`. They output will look something like this:

#### Terminal

``` bash

your_rails_app/public/assets/application-454840c9c54adb8be789d8ca014fa0a02022a5dcd00523f2dd0469bc990ed0e6.js

your_rails_app/public/assets/application-454840c9c54adb8be789d8ca014fa0a02022a5dcd00523f2dd0469bc990ed0e6.js.gz

your_rails_app/public/assets/application-e80e8f2318043e8af94dddc2adad5a4f09739a8ebb323b3ab31cd71d45fd9113.css

your_rails_app/public/assets/application-e80e8f2318043e8af94dddc2adad5a4f09739a8ebb323b3ab31cd71d45fd9113.css.gz

```

When you want to compile locally, it is recommended to change the location where Rails outputs your assets. If you don’t do that, you would need to recompile assets for development because you wouldn’t see local changes without it. The changed URL will be used by Sprockets to serve your assets in development mode.

#### config/environments/development.rb

```

config.assets.prefix = "/dev-assets"

```

For production, the compiled files will still be placed into an `/assets` directory by default.

You will end up with single assets, aka manifest files like `application.js` and `application.css`. That way you don’t need to manually link to all your assets by hand in your markup and avoid having to load multiple asset files instead of one for each category. The long number you see that is attached is called the fingerprint. It is used to compare files and if their contents might have changed before they need to be cached again. What you can also see is that you get two versions of the same file. The one with the `.gz` file extension is the gzipped version of the assets. `gzip` is used for file compression and decompression and cut away a bit of the fat that we don’t want to have sent over the wire. Another improvement to increase speed basically.

In case you feel the need to precompile your assets on a production server, the following command below will create the assets directly on your server(s). However I only add this for completion sake. Not sure that you will have much need for this as a beginner right now.

#### Terminal

``` bash

RAILS_ENV=production bin/rails assets:precompile

```

### Manifest Files & Directives

These manifest files like `application.css` include the logic to import all the files its search path. Rails imports these partials first and then compiles into a single authorative file that the browser will use. That’s just a default though and you can change that of course.

Every manifest file has directives which are instructions that determine which files need to be required to build up these manifest files. The order in which they are imported is determined in there as well. The final result contains all the contents of all the files the directives have access to. Sprockets loads these files, does the necessary preprocessing and rounds the whole thing off by compressing the result. Pretty darn useful!

For your CSS manifest file, `app/assets/stylesheets/application.css` this looks like the following:

``` css

/* ...
*= require_self
*= require_tree .
*/

```

The JavaScript equivalent `app/assets/javascripts/application.js` looks similar:

``` js

// ...
//= require jquery
//= require jquery_ujs
//= require_tree .

```

As you can see from this example, requiring jQuery first is a must if you rely on it within your JavaScript code.

## MD5 Fingerprinting?


By default, Rails creates a fingerprint for each filename during the precompilation process. More specifically, sprockets creates an MD5 hash from your files’ contents. The resulting 32 character long hexadecimal string, aka a digest. It is then attached to the filenames of your assets. That means that if the contents of your files change, your filenames, the MD5 hash part of it, will change as well. Why is it called fingerprinting? Such hashes have a very high probability of being unique and can therefore be used to identify uniqueness of file—just like fingerprints.

#### Filename Example

``` bash

navbar-908e25f4bf641868d8683022a5b62f54.css

```

We are not talking about a randomized hexadecimal string. The contents of files are pushed though a mathematical function that converts it into a unique 32 character long sequence. That means that you get the same hashed result when you apply the function to the same content twice—or how often you like.

Through that clever mechanism it is possible to check for changes and only update files that would result in a different MD5 hash. For caching purposes, this is golden. If nothing has changed it is cached by the web browser for future requests. In that context, cache busting simply means that remote clients will request a new copy of a file because the fingerprint has changed. Of course, a new fingerprint will created and added to the filename of your asset.

``` bash

MD5("The quick brown fox jumps over the lazy dog")

```

``` bash

9e107d9d372bb6826bd81d3542a419d6

```

You can disable fingerprinting both for production and development:

#### config/environments/production.rb 

``` ruby

config.assets.digest = false

```

#### config/environments/development.rb

``` ruby

config.assets.digest = false

```

## Asset Links

Let’s not forget why it’s nice to have the Asset Pipeline. It aims at making it easy for you to deal with assets. Writing the styles and behaviors for apps has become increasingly more nuanced and complex. Some of the tools also have become more joyful to work with. Preparing assets for production and serving them should be at least a bit more trivial and save you some time.

Having a bit of automation and conventions to organize assets is truly nice because it makes your actual job easy along the way. The Asset Pipeline even sweetens the deal and rounds things off with a few sugary assets links. This makes it a blast to deal with assets in your code. Let’s look at a few of the usual suspects that hopefully increase your happiness level even more.: 

+ `javascript_include_tag`
+ `stylesheet_link_tag` 
+ ```image_tag```

Since its extraction, Sprockets did not change the way you can link your assets, it still works the same as before. The examples above are convenience methods that take the name of your assets as arguments. They then figure out the extension names for correlated files themselves. These helper methods not only create the necessary tags for the proper HTML but also take care of linking to the asset files. They are not mandatory of course but still nice to have and very readable too. There is a bit less clutter in your markup if you make use of them.

#### Some View

``` erb

<%= stylesheet_link_tag "some-stylesheet", media: "all" %>

<%= javascript_include_tag "some-js-file" %>

<%= image_tag "some-image.png" %>

```

In your global layout file, Rails gives you three of them out of the box.

#### app/views/layouts/application.html.erb

``` erb

<%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
<%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
<%= csrf_meta_tags %>

```

This results in the following output in the rendered HTML:

``` html

<link rel="stylesheet" media="all" href="/assets/application.self-e80e8f2318043e8af94dddc2adad5a4f09739a8ebb323b3ab31cd71d45fd9113.css?body=1" data-turbolinks-track="true" />

<script src="/assets/application.self-3b8dabdc891efe46b9a144b400ad69e37d7e5876bdc39dee783419a69d7ca819.js?body=1" data-turbolinks-track="true"></script>

<meta name="csrf-param" content="authenticity_token" />

<meta name="csrf-token" content="hwaD9ubZUD8tKOrHAA9wgccbxmnOW5mSl4VzU7S2UmPvTXv9fVJND25/YZOz84ntdpP0rhun6ShSEkkZrmcsbQ==" />

```

Let’s look closer at how to handle image assets.

### image_tag

Images placed in `public/assets/images` directory can be accessed via this convenience method—no need to fiddle around with path names yourself. A good example of “convention over configuration” at work.

#### some.html.erb file

``` ruby

<%= image_tag "some-image.png" %>

```

That would result in the following:

``` html

<img src="http://example.com/assets/some-image.png" alt="Some image"  />

```

If activated, Sprockets will serve such files if found. When a file like `some-image.png` is fingerprinted like `some-image-9e107d9d372bb6826bd81d3542a419d6.png` it will be treated the same way.

If you need other directories within `public/assets/images` or within `app/assets/images` to organize your images, maybe something extra for icons or svg files, Rails will have no problem finding them. You simply need to add the directory’s name first:

``` ruby


<%= image_tag "icons/some-icon.png" %>

<%= image_tag "svgs/some.svg" %>

```

See, no rocket science and the other asset helpers are handled the same way.

## Pimped Styles


### CSS & ERB

The Asset Pipeline is set up to evaluate ERB code from the get go. All you need to do is add the `.erb` extension to a file and you are good to go—Sprockets will take care of the rest. Btw, when you generate controllers, it will also create views that already have the `.erb` extension. The same goes for scaffolds.

But this works for stylesheets as well. You can enhance them by using ERB in them. You simply create something like `example.css.erb` files. I’m not sure if it is a widely used technique though. It can be very handy but I would probably be cautious to overuse this since you can take this very far. These dynamic CSS files should probably not contain too much logic. Feels like a code smell but the damage seems to be contained if you rely on ERB helpers mostly.

```

.some-class { 
  background-image: url(<%= asset_path 'some-image.png' %>) 
}

```

This image will be found if you have it in one of the load paths of the Asset Pipeline—usually somewhere under `app/assets/images`. The cool thing is, if this file has already been processed and fingerprinted, then Sprockets will use the path `public/assets` and find it there. The same goes for other types of assets as well of course. Don’t forget to use `<%= %>`, `<% %>` to interpolate Ruby code in there. It won’t work without them. During precompilation, Sprockets will interpolate the code used in the CSS or Sass files and output plain `.css` files again—with or without a fingerprint according to your settings of course. 

Below are a few more options that might come in handy for linking to various asset categories:

+ asset_path
+ asset_url
+ image_path
+ image_url
+ audio_path
+ audio_url
+ font_path
+ font_url
+ video_path
+ video_url


The difference between these siblings is that the ```_url``` version gives you the full path, like `http://example.com/assets/application.css` while the ```_path``` version translates to the relative path to an asset,  like `/assets/application.css`.

### Sass Asset Helpers

When you use the `sass-rails` gem you can also make use of `path` and `url` helpers for your assets. They are a no-brainer really. It’s as simple as this:

#### some-stylesheet.css.erb

``` css

.some-class {
  background-image: asset-path("some-image.png");
}

.some-class {
  background-image: asset-url("some-image.png");
}

.some-class {
  background-image: image-path("some-image.png");
}

.some-class {
  background-image: image-url("some-image.png");
}

```

#### Notice that these helpers use a hyphen ( - ) and not an underscore ( _ ).

`image-path("some-image.png")` translates to `"/assets/some-image.png"`. `image-url("some-image.png")` will expand into `url(/assets/some-image.png)`—which in turn translates to the full url like `"http://www.example.com/assets/some-image.png"`. Same goes for `asset-path` of course.

Interestingly, this gem also offers their own flavor of other asset helpers from Rails. That means you don’t have to use `.erb` files and do `<%= %>` interpolations in your stylesheets. You simply use these asset helpers from `sass-rails`, which feels a bit more elegant in my opinion. Also, less code-smelly.

+ asset-path
+ asset-url
+ image-path
+ image-url
+ audio-path
+ audio-url
+ font-path
+ font-url
+ video-path
+ video-url

These helper methods know exactly where to find your assets—if you put it in the conventional directory, the search path basically. The difference between `path` and `url` is that you have relative paths and one one absolute path. A quick reminder. A relative path is the path to a certain file destination from some other file location. Absolute paths give you the location in reference to the root directory.

## Final Thoughts

The Asset Pipeline was extracted since Rails 4 and is not a core functionality anymore. It is enabled by default but Sprockets is now in charge of it. You are free to skip it when you initiate a project.

Using the pipeline makes asset management and compilation a breeze. You don’t need to set up anything and can focus just on working with these assets. The cherry on top is that you get a lot of handy convenience methods as well.

Your files for your CSS, JS, CoffeeScript, Sass, Haml, Slim, etc are neatly organized in one central. They are conveniently placed, under `app/assets`. Sprockets is responsible for serving files from this directory. Files in there usually need some preprocessing, like turning Sass files into their equivalent CSS files.

By now you know most of the Asset Pipeline features that beginners usually have a tricky time to wrap their heads around. More important than knowing its functionality alone, you are now familiar with the why as well. You should have a good introductory grasp on compilation, fingerprinting, caching, minification and compression. I hope you will appreciate how much this pipeline does for you in order to make your developer lives a little bit more hassle-free.
