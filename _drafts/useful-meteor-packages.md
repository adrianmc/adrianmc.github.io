---
layout: page
title: "Useful Meteor Packages"
comments: True
---

This is where I keep a list of useful Meteor packages. This will be my go-to page whenever I start a new Meteor app. Notice that I tend to prefer Foundation to Bootstrap.

# Required

```
meteor add underscore
meteor add ewall:foundation
meteor add iron:router
```

# Accounts

If the app requires accounts, consider using the following packages:

- [accounts-password](https://atmospherejs.com/meteor/accounts-password)
- [useraccounts:foundation](https://atmospherejs.com/useraccounts/foundation)

```
meteor add accounts-password
meteor add useraccounts:foundation
```

# Pretty

- [natestrauser:font-awesome](https://atmospherejs.com/natestrauser/font-awesome) - pretty icons
	- `meteor add natestrauser:font-awesome`
- [sergeyt:typeahead](https://atmospherejs.com/sergeyt/typeahead) - textbox with autocomplete dropdowns
	- `meteor add sergeyt:typeahead`
- [russ:weather-icons](https://atmospherejs.com/russ/weather-icons) - pretty weather icons
	- `russ:weather-icons`
- [mfpierre:chartist-js](https://atmospherejs.com/mfpierre/chartist-js) - pretty charts
	- `meteor add mfpierre:chartist-js`

# Optional

- sacha:spin - a loading spinner, typically used when the client is waiting
	- `meteor add sacha:spin`
- accounts-facebook - Facebook sign-in
	- `meteor add accounts-facebook`
- [selaias:meteor-simpleweather](https://atmospherejs.com/selaias/meteor-simpleweather) - a nice simple weather api
	- `meteor add selaias:meteor-simpleweather`