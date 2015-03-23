---
layout: post
title: "Code Snippets: Part 4"
comments: True
tags: code_snippet
---

It's been a while since my last post, but here it is again. Some new code snippets so I won't forget what I've learned! Note that I have since converted to using Coffeescript and Jade instead of plain Javascript and HTML respectively. I know that it may not be the lowest common denominator, but I urge you all to consider learning these two helpful languages.

# 1. Cycling through images for the background (with fade and other effects)

The goal was to change up the background image of a website every 5 seconds or so. For this, I used the `kidovate:maximage2` meteor package, and that installed MaxImage 2.0 which manages this effect. You will need to list the paths to the images in the HTML like this:

```jade
#maximage
  img(src="http://path/to/file1.jpg")
  img(src="http://path/to/file2.jpg")
  img(src="http://path/to/file3.jpg")
```

And initialize it in the JS file like this:

```coffee
Template.templateName.rendered = ->
  $ ->
    $('#maximage').maximage
      cycleOptions:
        speed: 5000
```

More documentation on how to use this library [here](http://www.aaronvanderzwan.com/maximage/).

# 2. OAuth

http://stackoverflow.com/questions/26172628/meteor-js-accounts-google-package-not-working-for-me

```
meteor add service-configuration
```

In the app:

```javascript
ServiceConfiguration.configurations.upsert(
  { service: "weibo" },
  {
    $set: {
      clientId: "1292962797",
      loginStyle: "popup",
      secret: "75a730b58f5691de5522789070c319bc"
    }
  }
);
```