---
layout: post
title: "How to Learn Web Development"
comments: True
tags: opinion
---

I've been running into problems trying to teach people how to do web development. A lot of people lack the iterative mindset that is so crucial for a developer/designer. People are always in a rush to get things done and it often holds them back. Here is a short tutorial on my personal mindset when trying to make a webpage that changes the background colour to red whenever the user clicks something.

This is a rather simple thing to make happen, but pay close attention to the approach. This is the same mindset that would be helpful whenver you are learning any type of programming even years later. Please **do not** skip anything and skim this article. Please read through every detail until the end!

# Steps

We will start by first trying to capture the user's input and then doing something with it. Afterwards, we will change the background colour. Simple!

1. User clicks something, and the page does something.

2. User clicks something, and the page changes background colour to red.

# 1. Click and Do

Here's the basic structure of pretty much any webpage:

```html
<html>
  <head>
  	<script>
  		// your javascript here
    </script>
  </head>
  <body>
  	<!-- your basic markup here -->
  </body>
</html>
```

Alright, so first thing we want to do is allow the user to click something. Let's insert an anchor tag.

```html
<html>
  <head>
  	<script>
  		// your javascript here
    </script>
  </head>
  <body>
  	<!-- your basic markup here -->
  	<a href="">Click Me!</a>
  </body>
</html>
```

Great, now we have something for the user to click. But hmm, we need it to *do something* when the user clicks it. Doing something requires some kind of logic, possibly javascript. I wonder how we can run some javascript when a user clicks on an anchor tag.

So I literally just Google'd "how can we run some javascript when a user clicks on an anchor tag."

The first result I get is [here](http://stackoverflow.com/questions/134845/href-attribute-for-javascript-links-or-javascriptvoid0). It seems that the top answer recommends using this:

```html
<a href="javascript:void(0)" onclick="myJsFunc();">Run JavaScript Code</a>
```

Okay, so let's add that in:

```html
<html>
  <head>
  	<script>
  		// your javascript here
    </script>
  </head>
  <body>
  	<!-- your basic markup here -->
  	<a href="javascript:void(0)" onclick="myJsFunc();">Click Me!</a>
  </body>
</html>
```

Apparently, the `myJsFunc()` is the function that is supposed to be run whenever someone clicks on the `<a>` tag. I wonder if it actually does that. Let's test it! But first, I'll need to have that function somewhere.

Let's declare that function and see if it actually runs. But how do I declare a javascript function?

I Google'd "how to make a function in Javascript", and the [first link](http://www.w3schools.com/js/js_functions.asp) shows me the syntax.

```html
<html>
  <head>
  	<script>
  		// your javascript here
  		function myJsFunc(){
  			// something to indicate that this has run
  		}
    </script>
  </head>
  <body>
  	<!-- your basic markup here -->
  	<a href="javascript:void(0)" onclick="myJsFunc();">Click Me!</a>
  </body>
</html>
```

Okay, I've written the function. But how do I find out if it has run? I need to get it to *do something*, maybe I need a pop-up message. So I Google'd "javascript popup message" and went to the first link [here](http://www.w3schools.com/js/js_popup.asp).

Oh, an Alert Box eh? The example shows `alert("I am an alert box!");`. Hmm, let's try it:

```html
<html>
  <head>
  	<script>
  		// your javascript here
  		function myJsFunc(){
  			// something to indicate that this has run
  			alert("I am an alert box!");
  		}
    </script>
  </head>
  <body>
  	<!-- your basic markup here -->
  	<a href="javascript:void(0)" onclick="myJsFunc();">Click Me!</a>
  </body>
</html>
```

**Click!**

Oh it works! Wonderful. Now the user clicks something, and the page responds by doing something.

WE HAVE COMPLETED STEP ONE!

Let's celebrate our victory and have some cake. Maybe we can take a break too, whew that was a lot of work.

# 2. Click and Change Colour

Alright, now we're back. We've found out how to make javascript do something whenever the user clicks on something. Now let's find out how to change the background colour of the page with Javascript.

Let's Google "How to change the background colour of the page with javascript?" First link [here](http://www.w3schools.com/jsref/prop_style_backgroundcolor.asp) shows an example:

```
document.body.style.backgroundColor = "red";
```

**Wow!** This looks like just what we needed! Let's put that in and see if it actually works.

```html
<html>
  <head>
  	<script>
  		// your javascript here
  		function myJsFunc(){
  			// something to indicate that this has run
  			alert("I am an alert box!");
  			document.body.style.backgroundColor = "red";
  		}
    </script>
  </head>
  <body>
  	<!-- your basic markup here -->
  	<a href="javascript:void(0)" onclick="myJsFunc();">Click Me!</a>
  </body>
</html>
```

**It does! Another victory!**

Great! But it's kind of annoying that the Alert Box still pops up. It's served its purpose so I can probably remove it. I wonder if I'll need to find out if the function is still running later on though. Hmm, I can just comment it out for now.

```html
<html>
  <head>
  	<script>
  		// your javascript here
  		function myJsFunc(){
  			// something to indicate that this has run
  			// alert("I am an alert box!");
  			document.body.style.backgroundColor = "red";
  		}
    </script>
  </head>
  <body>
  	<!-- your basic markup here -->
  	<a href="javascript:void(0)" onclick="myJsFunc();">Click Me!</a>
  </body>
</html>
```

Excellent! No pesky alert box wasting my time. Man, programming is tiring!

# Discussion

## Constant Googling

Notice how I Google'd every single "stupid" question I might have had? Well, it turns out that this is how most programmers work. Not everyone can remember every little thing about programming. For example, it's kind of difficult to expect people to memorize `document.body.style.backgroundColor` is the proper property to use when setting background colours. So it is absolutely reasonable and even recommended that you look this up whenever you need it.

In fact, a refusal to look things up and an insistence on trying to remember these things will probably stop you from landing a job in programming. You have to focus on what's important and ruthlessly cut out what you don't need to know off the top of your head. It's important to keep things simple.

## Multiple Pathways

There are always going to be multiple ways to do something. So for example, I could've used a `<button>` element instead of an anchor tag. But the process would be the same. I would've Google'd "how to run javascript when a button tag is pressed" and I would likewise have found a way to do what we did. There are also multiple ways to run javascript when an anchor tag is clicked.

It doesn't really matter what is the "best" way because we can always refactor. What's important is getting something that works. You can always go back and clean things up or use a *better* way of writing code. This brings me to the next point.

## Small Steps in the Right Direction

It's about taking small, tiny, even miniscule steps towards your goal. The journey of a thousand miles begins with a single step. If you let yourself get frustrated, then you'll never reach your goal. Keep cutting things down, keep trying to break apart what you need to do to get just a tiny bit closer to your goal. I hope the example above has made it clear how I do this personally.

## Always Test

Notice how the first thing I did every time I added something new was to test if things were working. You have to constantly be testing and making sure something is working so you can build that understanding of how things are tangibly working inside your head.

It may have seemed like it'd waste a lot of time to throw in that Alert Box. You may have thought, "Why are you wasting time on this?! Maybe you should just change the colour in the javascript and see if it changes it to red!"

Of course, I totally could've done that. But I wanted to take tiny steps. And for me, it was very important that I knew the function was in fact running before I went any further. It's not a rush, and it'll do you much good to have a better understanding of what's going on. So take the time to test everything.

## This is Real

You may think that I am "dumbing myself down" with this tutorial just to explain something to newbies. You would be wrong.

I actually had to go through this not too long ago because I hadn't done web development in a while. The fact of the matter is, programmers Google things ALL THE TIME. Sometimes it feels like you're starting off as a baby each time. There have been countless times where I've had to Google "if statement in Python" or a "For loop in Java" because I hadn't used that language in a while.

This is good, because you shouldn't be memorizing these things (i.e. the syntax). It's way more important to just have an understanding of the things you need to look up.

# Going Forward

Now you may want to proceed to styling things. Well, CSS is the language of choice here. And whatever you seek to do you can probably just Google it. If you would like to generate a random colour instead of just red, then I would suggest googling "javascript generate random color" and you'll find the top post to be very helpful.

Whatever the case, you need to be unafraid to test things. You need to be able to take the time to pare things down to their simplest elements and then only attempt those very small victories. Otherwise, maybe not now, but eventually you will burn out and probably tell yourself that "oh I guess I'm just not good at programming."

You have to be humble. A lot of people (adults especially) try to rush into things and they think it shouldn't take them that much effort and hassle to change a background colour to red. Well it is this arrogance that drives people crazy and eventually makes them give up programming and web development.

Don't let this happen to you. Be humble, take small steps, and enjoy the process. You'll get to the end soon enough.