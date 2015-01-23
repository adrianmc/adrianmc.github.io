---
layout: post
title: "Code Snippets: Part 2"
comments: True
---

It's that time again.

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

