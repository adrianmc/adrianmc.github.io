---
layout: post
title: "Code Snippets: Part 3"
comments: True
tags: code_snippet
---

Now that [b4YouGo](http://b4yougo.net) has been launched, it's time for my next project. As a result of this new endeavour, I'll be learning much more about developing on Meteor.

# 1. Setting up an accounts system

An accounts system can be complicated, but it needn't be. By installing the following packages, I was able to get up and running pretty fast (with some tweaking, see below):

- `accounts-password`
- `useraccounts:foundation`
- `useraccounts:core`

`useraccounts:core` is actually a dependency that will get automatically downloaded when you install the `useraccounts:foundation` package, but due to some of the tweaking I had to do, I had to download it as a custom package.

## Templates

At the minimum, you'll need to create a login and registration template. And if you are doing a webapp, I suggest also a dashboard (or home) template so that your logged-in users can get redirected there instead of your main landing page.

In your login template, make sure you have this: 

```
{% raw %}{{> atForm state='signIn'}}{% endraw %}
```

In your registration template, make sure you have this: 

```
{% raw %}{{> atForm state='signUp'}}{% endraw %}
```

In the nav bar, if you want a smart sign-in/sign-out button, then you'll need to add this:

```
{% raw %}{{> atNavButton}}{% endraw %}
```

## Routing

Great, now comes the hard part. We have to set up the actual routes like this in our `router.js`:

```javascript
// Login and Register Routes
AccountsTemplates.configureRoute('signIn', {
    name: 'login',
    path: '/login',
    template: 'siteLogin',
});
AccountsTemplates.configureRoute('signUp', {
    name: 'register',
    path: '/register',
    template: 'siteRegister',
    redirect: '/',
});
```

This takes care of the login and registration templates. Obviously this is assuming that your login and registration templates are named siteLogin and siteRegister respectively.

When a user is logged in and they try to access your root domain, you'd probably want to redirect them to the dashboard rather than the landing page. To do that, I used this code:

```javascript
Router.route('/', {
  name: 'landingPage',
  layoutTemplate: 'landingLayout',
  onBeforeAction: function (pause) {
      if (Meteor.user()) {
        Router.go('/home'); // send user to the dashboard
      } else {
        this.next();        // continue to landing page
      }
    }
});
```

Notice that I also have a special layout for it, called `landingLayout`. The dashboard route is also at the `/home` path, so I simply tell it to route there if there is a user logged in.

As for the dashboard, or in our case the `/home` path:

```javascript
Router.route('/home', {
  name: 'siteHome',
  onBeforeAction: AccountsTemplates.ensureSignedIn
});
```

Notice that I make use of an `AccountsTemplates.ensureSignedIn` function hook to make sure the user is signed in before being able to access the dashboard.

## Tweaking the packages

As of 2015-01-31, there seems to be something odd in the `useraccounts:core` package that prevents the navbar login/logout button from finding the correct sign-in link. As a result, in the `at_nav_button.js` template helper file, I replaced that particular line with `AccountsTemplates.linkClick("signIn");`. See example below:

```javascript
AT.prototype.atNavButtonEvents = {
    'click #at-nav-button': function(event){
        if (Meteor.user())
            AccountsTemplates.logout();
        else
            AccountsTemplates.linkClick("signIn");  // this is correct
    },
};
```

Another curious thing about the nav button template is that it (by default) comes wrapped in an `<li>` element. To see what I mean, take a look at `at_nav_button.html` inside the `useraccounts:foundation` package. I replaced:

```html
<template name="atNavButton">
  <li class="has-form">
    <a href="#" id="at-nav-button" class="button">{{text}}</a>
  </li>
</template>
```

with:

```html
<template name="atNavButton">
  <a href="#" id="at-nav-button">{{text}}</a>
</template>
```

This way, I can make sure that it will fit with however I wanted to style the nav bar.

## Other settings

There will likely be other settings you'll want to set. For that, you should create an accounts.js file in the server folder. Then, you can have a look at what the documentation says [here](https://github.com/meteor-useraccounts/core/blob/master/Guide.md#options) about what options you have.

# 2. Getting email verification up and running

## 1. Install the email package

This is pretty self-explanatory, but I also recommend that you add the houston admin console so you can easily check to see if your user's e-mails are verified. This step would just require you to type the following into the command line:

```
meteor add email houston:admin
```

## 2. Acquiring an SMTP Server

For this step, I simply signed up for a free account on mail-gun. There are a lot of options out there, particularly if you don't mind paying for it. On the free side, there's mail-gun's free-level offering which is sufficient for testing at this early stage. Apparently there's also Gmail's own SMTP server that you can use for free if you have a Gmail account, but I was not able to get that running. Besides, it was a lot more simple to use mail-gun so I recommend them.

## 3. Setting up your SMTP variables

Set your environmental variable like so:

```javascript
// server/smtp.js
Meteor.startup(function () {
  smtp = {
    username: 'username@example.com',
    password: 'p4$$w0rd',
    server:   'smtp.gmail.com',  // eg: mail.gandi.net
    port: 465
  }
  process.env.MAIL_URL = 'smtp://' + encodeURIComponent(smtp.username) + ':' + encodeURIComponent(smtp.password) + '@' + encodeURIComponent(smtp.server) + ':' + smtp.port;
});
```

If you used mailgun, it should look something like this:

```
process.env.MAIL_URL = 'smtp://postmaster%40somelongstringofnumbersandletters.mailgun.org:anotherlongstringofnumbersandletters@smtp.mailgun.org:587'
```

Note that the "%40" in "postmaster%40" represents the `@` symbol.

## 4. Configure routes

And finally, all you have to do is to configure the route for your e-mail verification page:

```javascript
AccountsTemplates.configureRoute('verifyEmail', {
    name: 'verifyEmail',
    path: '/verify-email'
});
```

# Removing Autopublish

Run command: `meteor remove autopublish`

This means we will have to write our own rules for publishing and subscribing collections.

```javascript
// client/main.js
Meteor.subscribe('dailyEntries');
```

```javascript
// server/publications.js
Meteor.publish('dailyEntries', function() {
  return DailyEntries.find({userId: this.userId});
});
```

We make sure to only publish the entries where the userId matches the current user's userId.

# Removing Insecure

Run command: `meteor remove insecure`

This means that we want to set restrictions on who can insert/update/remove items in a collection. First let's define a function that we can use to make sure the user owns a particular document in the collection:

```javascript
// lib/permissions.js
ownsDocument = function(userId, doc) {
  return doc && doc.userId === userId;
}
```

Now we write allow statements in to let Meteor know under what circumstances the insert/update/remove functions are allowed:

```javascript
// lib/collections/dailyEntries.js
DailyEntries.allow({
  
  // only allow insertion if you are logged in
  insert: function(userId, doc) { return !! userId;},

  // only allow update/remove if the user owns the document
  update: function(userId, doc) { return ownsDocument(userId, doc); },
  remove: function(userId, doc) { return ownsDocument(userId, doc); },
});
```