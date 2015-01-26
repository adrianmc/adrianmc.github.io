---
layout: page
title: "Walkthrough Building a Boilerplate Project - Part 1"
comments: True
tags: walkthrough
---

I'm going to walk you through creating a boilerplate project in Meteor. This was written primarily for the purposes of me getting to know the framework better, but I think it would also be a great guide for those learning Meteor as well.

# Let's begin

Run the script: `bash setup.sh myapp`

1. 































# Let's begin

Run the following:

```
meteor create myapp
cd myapp
mkdir client server lib public
```

1. The first step is of course to start a meteor project. That can be done easily by simply typing `meteor create myapp` into the terminal. Go into the folder that was just created and delete the three generated files there, you won't need them.

2. After this, you'd want to setup the file structure by creating the following four directories: `/client`, `/server`, `/lib`, `/public`. You can do this in one command by navigating to your app folder and typing the following line: `mkdir client server lib public`.

At this point, we will talk about what we're going to do in the `/client` directory. Here, we will be setting up most of the front-end stuff.

----

# Client

Run the following command: 

```
cd client
mkdir stylesheets templates templates/includes templates/application
```
1. Let's start building out this part of our file structure by creating the following three folders: `/stylesheets`, `/templates`. To do that in one line: `mkdir stylesheets templates`.
	- The `/stylesheets` folder is where we will put our CSS files. The `/templates` folder is where most of the fun happens. This will be where we place our templates and the logic that goes behind them.

2. Inside of our `/templates` folder, we should also create two folders: `/includes` and `/application`. In one line: `mkdir includes application`.
	- Under the `/application` directory, we will put the templates for our general application. This will be things like, a 404 page, a loading template, and also our general layout as well. As for the `/includes` folder that would be things like the header, the footer, etc.


