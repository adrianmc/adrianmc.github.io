---
layout: post
title: "Code Snippets: Part 2"
comments: True
tags: code_snippet
---

It's that time again. I've learned a lot more since the last time I posted code-snippets. A lot of these were small issues where the solution was not immediately obvious and took a non-insiginificant amount of time for me to find it. Amongst them, I've also included a few references that I think are useful resources.

# 1. Fix: Site looks zoomed-out on mobile

Due to the high resolution of mobile devices these days, it's not all that straight-forward to use media-queries to detect the width of the device. Or maybe it is and I'm just not very good at it. In any case, remember to put the following `meta` tag into your page's `<head>` section.

```html
<meta name="viewport" content="width=device-width, target-densitydpi=high-dpi" />
```

# 2. Reset the database of deployed apps on meteor.com

Pretty simple, the first line deletes the app and the second line uploads it again.

```
meteor deploy myapp.meteor.com --delete
meteor deploy myapp.meteor.com
```

# 3. Ultimate Meteor Reference Guide

This particular [blog post](http://joshowens.me/getting-started-with-meteor-js/) covers a good deal of Meteor. Now, I don't think it is required or even a good thing for an entpreneur to know every single little thing about Meteor (unless you want to sell Meteor books). But it is a good reference nonetheless.

# 4. Customizing meteor packages for your project

This is actually surprisingly simple. This is the kind of code you'll need to use:

```
> cd your/project/path
> mkdir packages && cd packages
> git clone https://github.com/meteor-useraccounts/semantic-ui.git
> cd ..
> meteor add useraccounts:semantic-ui
> meteor
```

To summarize:

1. Create a directory named `/packages` in your project directory.
2. Use `git clone` to download the package you want to use into the `/packages` directory.
3. Make the changes you want to the package source.
4. Install the package as usual with `meteor add`.

**Note:** It's probably a good idea to fork it first on github, and clone that into your packages directory. That way, if you ever wanted to contribute your changes back to the original source, you could easily push your commits up to github and do a pull request.

# 5. Loaded Boilerplate Meteor Project

This [boilerplate project](https://github.com/yogiben/meteor-starter) is based on Bootstrap (not my preferred framework of choice, I prefer Foundation), but it is very impressive and quite comprehensive. Instead of using Yogiben's admin, I also prefer the [Houston admin](https://atmospherejs.com/houston/admin).

# 6. CSS nth-child selectors and responsive design

I had a series of divs and I wanted it to be in rows of three when the screen had the room for it, but rows of two when it didn't. Finally, if the screen width really was too small, it would end up just being layed out vertically. The way I did this was with the nth-child selector and then selectively clearing either every third or every second div. Note that all these divs in question already had `float: left` being applied to them.

```css
.datablock:nth-child(2n+1) {
  clear: left;
}

@media only screen and (min-width: 62.5em) {
  .datablock:nth-child(3n+1) {
    clear: left;
  }
  .datablock:nth-child(2n+1) {
    clear: none;
  }
}
```

# 7. Spacebars Reference Guide

This [link](http://meteorcapture.com/spacebars/#each-ex-1) has a lot of good examples showing how to work with Spacebars in Meteor.

# 8. Using template variables in meteor upon render

When Iron Router loads a template, it can pass in some variables. It's pretty straightforward how to use these variables in the template or the template helper functions themselves. However, it's a little different if you want to use those same variables on render.

Instead of using `this.varName`, you have to use `this.data.varName`. Observe:

```javascript
Template.cityDataset.helpers({
  airports: function() {
    console.log("helper-name: " + this.name);        // New York City
    console.log("helper-country: " + this.country);  // United States

    return CityDatasets.findOne({name: this.name, country: this.country}).airports;
  },
});

Template.cityDataset.rendered = function() {
    console.log("name: " + this.name);        // undefined
    console.log("country: " + this.country);  // undefined
    console.log("name: " + this.data.name);        // New York City
    console.log("country: " + this..data.country);  // United States

    // doSomethingCoolToDOM(this.name, this.country);

}
```

# 9. Preventing form submit and substituting your own action

This will prevent the form from submitting:

```javascript
$("form").submit(function( event ) {
    event.preventDefault();
  });
```

This is how you can bind your own event handler to the enter key:

```javascript
$('#search').bind("enterKey",function(e){
    e.preventDefault();
    var input = $('#search').val();
    
    // do stuff with the input

  });
  $('#search').keyup(function(e){
      if(e.keyCode == 13)
      {
          $(this).trigger("enterKey");
      }
  });
```


# 10. Using Meteor's findOne() in a non-case-sensitive way

There are two options, one is to basically create (in the collection) a fully uppercased or lowercased version of the parameter you want to match. The second (and the option that I chose) is to use regex to match the parameter instead.

```javascript
return Cities.findOne({
  name: {$regex : new RegExp(this.params.name, "i")},
  country: {$regex : new RegExp(this.params.country, "i")}
});
```

# 11. Taking care of form submits in Meteor

This will require doing things in two files, namely the template helper (or controller in MVC parlance) and then collection javascript file. The reason for this is simple. The template helper simply lets the browser know what to do when the user hits the submit button. Ultimately, this ends up calling a function from the collection javascript file. 

The collection javascript file manages what methods can be run on the server. Notice in the example below, it checks whether or not the user's email is the admin's email.

Remember, Meteor Methods can only run on the server, so there's no issue of allow or deny here. The debate between when to use allow/deny or Meteor Methods can be found [here](https://www.discovermeteor.com/blog/meteor-methods-client-side-operations/).

**city_submit.js**

```javascript
Template.citySubmit.events({
  'submit form': function(e) {
    e.preventDefault();   // prevent default behaviour

    // create an object to house the info from the form
    var city = {
      name: $(e.target).find('[name=name]').val(),
      country: $(e.target).find('[name=country]').val()
    };
    
    // call the function named 'cityInsert' with our object
    Meteor.call('cityInsert', city, function(error, result) {
      // display the error to the user and abort
      if (error)
        return alert(error.reason);
      Router.go('cityPage', city);  // redirect to the cityPage template
    });
  }
});
```


**cities.js**

```javascript
Cities = new Mongo.Collection('cities');

Meteor.methods({
  cityInsert: function(cityAttributes) {
    check(Meteor.userId(), String); // check the userId is a String
    check(cityAttributes, {         // check the passed in attributes
      name: String,
      country: String
    });

    // does it already exist?
    var cityWithSameNameCountry = Cities.findOne({
      name: cityAttributes.name, 
      country: cityAttributes.country
    });
    
    // if so, throw an error
    if (cityWithSameNameCountry) {
      throw new Meteor.Error("already-exists",
        "That city already exists!")
    }

    var user = Meteor.user();
  
    // verify admin and then submit
    if (user.emails[0].address === "admin@myapp.com") {
      var city = _.extend(cityAttributes, {
        submitted: new Date() // add the current time into the object
      });
      Cities.insert(city);    // insert the object
      return city;
    } else {
      throw new Meteor.Error("not-admin",
          "You have to be the admin.");
    }
  }
});
```

