# Simple Scraping Tutorial

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

```
irb(main):001:0>
```

You can try to play around with it, but we won't spend too much time on explaining the IRB itself. You can do simple math like this:

```
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
```
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

```
entries = doc.css('.entry')
```

Let's check how many we've got:

```
entries.length
=> 35
```

Great! If you go to the front page of Reddit.com in your browser, you can count exactly 35 links on the front page. We're getting somewhere!

Now let's try to get the specific title of just one post. We'll try with just the first post for now, so we'll use `entries[0]` as our starting point and try to get more specific from there.


Try the following:
```
entries[0].css('p.title>a')
```

If you take a careful look, you should see a representation of exactly the anchor tag we are looking for. In order to get the title and the link, we will use the following code:

```ruby
entries[0].css('p.title>a').text         # title
entries[0].css('p.title>a')[0]['href']   # link
```

The code for the title is quite self-explanatory, it is simply the text of the tag itself. For the link, we have to find the `href` attribute, and attributes are stored as the first child of the object, so that is why we type `[0]['href']` to get the link.

Now let's see if we list the titles for all 35 entries. Let's use the .each construct:

```
entries.each do |entry|
	puts entry.css('p.title>a').text
	puts entry.css('p.title>a')[0]['href']
end
```

Notice how we use `puts` so that Ruby knows to display it onto our screen.


### Making a Class

```
class Entry
	def initialize(title, link)
		@title = title
		@link = link
	end
	attr_reader :title
	attr_reader :link
end
```

```
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

### Refactor

```
entriesArray = []
entries.each do |entry|
	title = entry.css('p.title>a').text
	link = entry.css('p.title>a')[0]['href']
	entriesArray << Entry.new(title, link)
end
```

### Refactor Again?

```
entriesArray = []
entries.each do |entry|
	entriesArray << Entry.new(entry.css('p.title>a').text, entry.css('p.title>a')[0]['href'])
end
```

Not readable, not so good.

Let's make this a website now.

## Step 2: Make this a Rails App

