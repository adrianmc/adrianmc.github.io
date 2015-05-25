---
layout: post
title: "Simple Scraping Tutorial in Rails"
comments: True
tags: tutorial, rails
---

For this tutorial, we are going to build a site with Ruby on Rails that can scrape a particular webpage for some specific information. More specifically, this tutorial will walk you through how to come up with a scraper for the titles and links on [Reddit's](http://www.reddit.com/) front page. You can go to the website and take a look to see what it is we are trying to obtain.

## Learning Objectives

By doing this tutorial you should have an understanding of the following:

1. How to approach scraping a website
2. How to use the Interactive Ruby Shell (IRB)
3. How to start a Ruby on Rails project
4. What a controller is
5. What a view is


## Step 1: Try it in the IRB

The IRB is kind of like a sandbox, it allows you to play with (and execute) Ruby code without having to start a huge project or do anything complex at all. That's why it is often a good idea to try things out in the IRB before we start building a whole site and getting into a lot of complexities, it's always nice to play with ideas and make sure that they are feasible and that you have some idea of how you're going to approach the problem you want to solve.

### Start the IRB

This tutorial is going to assume that you have both Ruby and Rails already installed. The IRB comes with Ruby, so all you have to do is go to the terminal and type `irb`. You should get a prompt like this:

```ruby
irb(main):001:0>
```

You can try to play around with it, but we won't spend too much time on explaining the IRB itself. You can do simple math like this:

```ruby
irb(main):001:0> x = 2
=> 2
irb(main):002:0> y = 3
=> 3
irb(main):003:0> x + y
=> 5
```

From this point on, I will omit writing the prompt `irb(main):001:0>`, it's assumed that you will be typing code at the prompt.

### Getting Ready to Scrape

We will need a tool to help us open webpages in our code, and also the ability to go through the HTML and find what we want. The former is accomplished by a wrapper called `open-uri` and the latter can be done with a parser called `nokogiri`.

So for our first step, we will `require` those two tools and then use them to load an entire page into our variable, `doc`:

```ruby
require 'open-uri'
require 'nokogiri'
doc = Nokogiri::HTML(open("http://www.reddit.com/"))
```

Notice that the `open` function opens the webpage, and then `Nokogiri` parses it into the `doc` variable as HTML. You will also see a lot of gibberish being displayed, but that's actually the information on the page as parsed by `Nokogiri`.

### Identifying What We Want

Now that we've grabbed the entire page in the `doc` variable, we can now mess with it however we want. So let's try to see how we can get the titles and links of the entries. To do that, we have to understand the structure of how the page is laid out. Let's go to the browser and find out.

So go to your favourite browser, navigate to [reddit.com](http://www.reddit.com/), right-click one of the entry titles and select `inspect element`. From inspecting the element of an entry title, you should find something that looks like this:

```html
...

<div class="entry unvoted">
    <p class="title">
        <a class="title may-blank " href="http://i.imgur.com/6aEop17.jpg" tabindex="1">The ingredients section on this toothpaste tube explains where each ingredient comes from and what it does</a>

...
```

What this tells us, is that every entry is wrapped inside a `<div>` with the class `entry`, and the title and link that we want is represented by an anchor tag `<a>` inside of a paragraph tag `<p>` with the class of `title`.

That's kind of confusing, so let's start simple.

### Getting at the Information

Let's start small and simply try to see if we can get a variable that represents all the entries on the page.

**Note:** Many scrapers use something called X-Path, but for simplicity's sake, we will use CSS selectors as Nokogiri provides that option for us.

Let's put all the `div` tags on the page with a class of `entry` inside a variable we will call `entries`:

```ruby
entries = doc.css('.entry')
```

Let's check how many we've got:

```ruby
entries.length
=> 35
```

Great! If you go to the front page of Reddit.com in your browser, you can count exactly 35 links on the front page. We're getting somewhere!

Now let's try to get the specific title of just one post. We'll try with just the first post for now, so we'll use `entries[0]` as our starting point and try to get more specific from there.

Try the following:

```ruby
entries[0].css('p.title>a')
```

Note that the selector string `p.title>a` simply means the `p` tag with a class of `title` and then get the `a` tag immediately under that.

If you take a careful look, you should see a representation of exactly the anchor tag we are looking for. In order to get the title and the link, we will use the following code:

```ruby
entries[0].css('p.title>a').text         # title
entries[0].css('p.title>a')[0]['href']   # link
```

The code for the title is quite self-explanatory, it is simply the text of the tag itself. For the link, we have to find the `href` attribute, and attributes are stored as the first child of the object, so that is why we type `[0]['href']` to get the link.

If you type those two lines above, you should be able to see the title and link as displayed by IRB.

Now let's see if we can list the titles for all 35 entries. Let's use the `.each` construct:

```ruby
entries.each do |entry|
	puts entry.css('p.title>a').text
	puts entry.css('p.title>a')[0]['href']
end
```

Notice how we use `puts` so that Ruby knows to display it onto our screen.

You should see the titles and their links being displayed in the terminal.

### Using a Class

Okay great, we know how to get the information, but this is kind of unwieldy, so let's attempt to create a class of `Entry` objects. Each `Entry` object will house the title and link for easy access. We will then try to create a whole array of these objects so it will be easy for us to manage and move around.

```ruby
class Entry
	def initialize(title, link)
		@title = title
		@link = link
	end
	attr_reader :title
	attr_reader :link
end
```

Note that every time we create an `Entry` object, we will need to initialize it with a title and a link. The initialized values will be assigned to the instance variables `@title` and `@link` respectively. The title and link of each Entry object is also made readable by the `attr_reader` lines.

Now let's try to make an array of Entry objects:

```ruby
# Create an empty array
entriesArray = []

# For each entry, 
# we're going to make an Entry object 
# and push it into the array
entries.each do |entry|
	title = entry.css('p.title>a').text
	link = entry.css('p.title>a')[0]['href']
	newEntry = Entry.new(title, link)
	entriesArray << newEntry
end
```

You can try runing something like `entriesArray[0].title` or `entriesArray[0].link` to ensure that this works.

### Refactoring

Refactoring means to change the code to make it better (in any number of ways) while keeping its functionality the same.

Let's refactor the code so that it's more readable. We can immediately place the newly created `Entry` object into the `entriesArray` instead of using the temporary variable `newEntry`:

```ruby
entriesArray = []
entries.each do |entry|
	title = entry.css('p.title>a').text
	link = entry.css('p.title>a')[0]['href']
	entriesArray << Entry.new(title, link)
end
```

Should we refactor again so that we remove the need for the temporary variables of `title` and `link`?

```ruby
entriesArray = []
entries.each do |entry|
	entriesArray << Entry.new(entry.css('p.title>a').text, entry.css('p.title>a')[0]['href'])
end
```

This is not so easy to read, so that's not good. We should refactor to be more concise when possible, but we should not reduce our code so much that it becomes hard to read.

Now we're ready to make this a Rails app.

## Step 2: Make this a Rails App

Let's open a brand new terminal and type in the following to start our brand new Rails app:

```
rails new scraper
```

After some setup stuff is run, you can `cd` into the directory and then type in `rails server` to run the app. Navigate to `http://localhost:3000` in your browser to see the default app page.


### Routing, Actions, and Controllers

The very basic explanation of what controllers do is this: When you try to access a website run by rails, a bunch of code in the rails app (called a router) will direct it to process a block of code (called an action) inside a particular file (called a controller).

Essentially, a controller is what processes a request sent to the server by a client. And each controller can house different actions.

For our example, let's add the `scrape_reddit` action inside our main controller in `app/controllers/application_controller.rb`:


```ruby
class ApplicationController < ActionController::Base
  
  ...
  
  def scrape_reddit
    render text: 'scrape reddit data here'
  end
end
```

We just made an action inside the main application controller. Right now, it does nothing other than try to render the text "scrape reddit data here", we will add more functionality to this later. We just want to see that it works first.

Now let's point to it with our router so that it loads right away when we load our webpage.

Add the following to `app/config/routes.rb`

```ruby
Rails.application.routes.draw do
  
  ...
  
  root 'application#scrape_reddit'
  
  ...

end
```

What we're doing here is to say that the `root` route should be directed to the `application` controller and more specifically, the `scrape_reddit` action within that controller.

Now let's test it. Go back to your browser and hit refresh. You should simply see the text "scrape reddit data here".

Great, it works as expected! But we don't just want to render the text, we want to scrape Reddit's front page and then render the titles and links.

### Scraping a Page

Let's start small and grab the front page like we did before in IRB. Just for fun we'll also try to see what happens when we render the retrieved document.

```ruby
def scrape_reddit
  require 'open-uri'
  doc = Nokogiri::HTML(open("http://www.reddit.com/"))
  render text: doc 
end
```

Go back to the browser, refresh and see. No you're not dreaming, it is actually pulling and displaying the reddit front page itself!

Of course this is overkill, so let's get more specific and paste our previous code into the `scrape_reddit` action so that we can grab just the titles and links.

```ruby
def scrape_reddit
  # Pull in the page
  require 'open-uri'
  doc = Nokogiri::HTML(open("http://www.reddit.com/"))

  # Narrow down on what we want and build the entries array
  entries = doc.css('.entry')
  entriesArray = []
  entries.each do |entry|
    title = entry.css('p.title>a').text
    link = entry.css('p.title>a')[0]['href']
    entriesArray << Entry.new(title, link)
  end

  # We'll just try to render the array and see what happens
  render text: entriesArray
end
```

If you tried to refresh and run the changes above, you'll encounter an error. That's because we forgot to define our `Entry` class! So let's do that right now, outside of the action itself:

```ruby
class ApplicationController < ActionController::Base
  
  ...

  # Define the Entry object
  class Entry
    def initialize(title, link)
      @title = title
      @link = link
    end
    attr_reader :title
    attr_reader :link
  end

  def scrape_reddit
    require 'open-uri'
    doc = Nokogiri::HTML(open("http://www.reddit.com/"))

    entries = doc.css('.entry')
    entriesArray = []
    entries.each do |entry|
      title = entry.css('p.title>a').text
      link = entry.css('p.title>a')[0]['href']
      entriesArray << Entry.new(title, link)
    end

    render text: entriesArray
  end
end
```

Try that.

No error anymore! But all we see is an array of '#' symbols. What's happening?

Well that's actually the entriesArray! It's filled with 35 `Entry` objects as expected. But since there is no string representation of our `Entry` object, it's just '#'.

### Rendering Template/Views

Okay, we need a smart way to be able to render the `Entry` objects as a list that the user can see. For this, we need to use a `view`. This is basically the skeleton with which we will send our data.

Let's make a new file here: `app/views/scrape_reddit.html.erb`

This will be our template/view that we render after grabbing the information. So, instead of rendering text at the end of our `scrape_reddit` action, we are going to make it render the view instead, with the `entriesArray`. We'll need to make a couple of changes to our `scrape_reddit` action in order to do this:

```ruby
class ApplicationController < ActionController::Base
  
  ...

  def scrape_reddit
    require 'open-uri'
    doc = Nokogiri::HTML(open("http://www.reddit.com/"))

    entries = doc.css('.entry')
    @entriesArray = []
    entries.each do |entry|
      title = entry.css('p.title>a').text
      link = entry.css('p.title>a')[0]['href']
      @entriesArray << Entry.new(title, link)
    end

    render template: 'scrape_reddit'
  end
end
```

We've made two changes:

1. We have pre-pended the `entriesArray` variable with an `@` symbol. This is to make that variable an instance variable so that we can use it inside our view.

2. At the end, we have now changed `render text: entriesArray` to `render template: 'scrape_reddit'` so that Rails will know to send the data to the `scrape_reddit` view with the data context.

Finally, let's go back to our view at `app/views/scrape_reddit.html.erb` and add the following:

```
<h1>Reddit's Front Page</h1>
<% @entriesArray.each do |entry| %>
  <p><%= entry.title %></p>
  <p><%= entry.link %></p>
<% end %>
```

So now that we have access to our `@entriesArray` variable, we can try to display each entry's title and link property. To do that, we have:

- `@entriesArray.each do |entry|` to start things off so that Rails will know to duplicate the following for each entry in `@entriesArray`
- `entry.title` and `entry.link` will allow Rails to render that `<p>` element with the corresponding data

That's it! It's actually qutie straight-forward. You can now go into your browser, refresh the page, and see Reddit's front page entry titles and links!