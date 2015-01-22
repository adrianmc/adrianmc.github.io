---
layout: post
title: "Code Snippets: Part 1"
comments: True
---

I learn a lot while building things, and I don't want to forget what I've learned. It has happened many times in the past, so this time I have decided to keep a record of what I have learned so that I won't have to redo any research that I've already done. Hopefully, this will be useful to other people as well.

The following is a list of things I learned recently about how to make websites with Meteor. A lot of these might be a lot more straight-forward or simple if you're doing a plain HTML file, but with Meteor there are some things to keep in mind. I've also included some non-Meteor specific techniques.

# 1. Making a nice full-length header bar based on Foundation

The [Top Bar](http://foundation.zurb.com/docs/components/topbar.html) documentation is great. However, since it relies on some fancy javascript listener, you have to apply the listener every time the template is loaded.

Just add the following code inside the corresponding javascript file for your template:

```javascript
Template.layout.rendered = function(){
	$(document).foundation('topbar', 'reflow');   // apply listener
};
```

# 2. Change an element's style when the user scrolls down

When the user scrolls down 50 pixels, the element with the `.top-bar` class will have the class `.scrolled` added to it. When the page is scrolled to the top, the `.scrolled` class will be removed.

Just add the following code inside the corresponding javascript file for your template:

```javascript
Template.layout.rendered = function(){
	$(document).ready(function() {
		$(window).scroll(function() {
			if ($(window).scrollTop() > 50) {
				$('.top-bar').addClass('scrolled');
			} else {
				$('.top-bar').removeClass('scrolled');
			};
		});
	});
}
```

# 3. Autocomplete or Typeahead

This one was really frustrating, but I finally figured out how to do it. I always find it frustrating when people ask about how to do something, and then the creator tells them to just take a look at the example but doesn't give specific instructions. This is understandable, but what is even more frustrating is when people eventually figure it out but doesn't explain how they figured it out. Well, the buck stops here.

- Run `meteor add sergeyt:typeahead` in the terminal to add the typeahead package.
- Use the following input element in your HTML template:

```html
<input class="form-control typeahead" name="team" type="text"
       placeholder="NBA teams"
       autocomplete="off" spellcheck="off"
       data-source="nba"/>
```

- Put the following code inside the corresponding javascript file for your template:

```javascript
Template.demo.helpers({
  nba: function() {
  	return Nba.find().fetch().map(function(it){ return it.name; });
  }
});

Template.demo.rendered = function() {
	Meteor.typeahead.inject($('.typeahead'));
}
```

The data-source in the HTML specifies `nba`. So the typeahead.js library will make note of that and run the `nba` function in the template helper. However, for that to work we have to let the typeahead.js library know to listen to the appropriate classes. This is the [link](https://atmospherejs.com/sergeyt/typeahead) to the package.

- Also, don't forget to style the dropdown. Here's mine for reference:

{% gist fb007a065cd5c0bef31d %}

----

The following techniques are not Meteor specific, but they are valuable lessons I learned which should be useful in the future.

# 4. Full Screen Background Images

I learned this from this [blog post](http://css-tricks.com/perfect-full-page-background-image/). Making the pretty full screen backgrounds that are popular these days is easy thanks to HTML5, all we have to do is use the following CSS:

```css
html { 
  background: url(images/bg.jpg) no-repeat center center fixed; 
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
```

# 4. Full Screen Sections

Another simple CSS trick. It's not really even a trick, but it's a trick to me because I didn't know about it before. This [blog post](https://medium.com/@ckor/make-full-screen-sections-with-1-line-of-css-b82227c75cbd) made it very simple to create divs that are 100% of the viewport height.

```css
.section { height: 100vh; }
```

# 5. Sticky Footer

[Ryan Fait](http://ryanfait.com/sticky-footer/) came up with a CSS sticky footer that "just works". I have reproduced it here for record's sake:

```css
* {
	margin: 0;
}
html, body {
	height: 100%;
}
.wrapper {
	min-height: 100%;
	height: auto !important; /* This line and the next line are not necessary unless you need IE6 support */
	height: 100%;
	margin: 0 auto -155px; /* the bottom margin is the negative value of the footer's height */
}
.footer, .push {
	height: 155px; /* .push must be the same height as .footer */
}
```