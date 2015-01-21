---
layout: post
title: "7 lessons from my first app failure"
---

In the summer of 2013, I had the idea to make a mobile app, so I called up one of my friends and got to work. Over a year later, we launched [Texty Time](https://play.google.com/store/apps/details?id=com.aheadstudios.talkalytics). The project took so long and not too many people used it, so I decided I never wanted to touch or even think about it again. Recently, I was reading [@levelsio](https://twitter.com/levelsio)'s blog on how he planned to start [12 startups in 12 months](https://levels.io/12-startups-12-months/). For a lot of these projects, he had a project debrief page where he talks about what he learned from each of them. Suddenly, I realized that I myself have attemped (and failed) at making something akin to a "startup" as well (albeit very very humble and basic). This is the story of what I learned.

As James Joyce said:

> Mistakes are the portals of discovery

I didn't even realize it was a "failure", because I didn't consider it to be a serious attempt. However, I realized that it would be an even bigger mistake if I failed to learn from the experience. So instead of running away from failure, here goes my attempt in trying to extract what lessons I've learned from my first app failure.

----

# Development

Throughout the development process, what affected us the most was our crippling technology, inefficient workflow, and overall lack of a market need for the product itself.

## Crippling Technology

Technology was a big part of what determines if an app succeeds or fails. First of all, there was the issue of being unfamiliar with the technology we used. I had never coded for Android before, so we tried a few different things at first. 

Coming from web development, I tried PhoneGap because I believed in the power of HTML5. This was a mistake, because although it sounds great in theory, there was always too much lag and not enough control over how things would be rendered. The fact that we had to rely on web frameworks within a webview on an Android phone was bad enough, but dealing with a rapidly changing PhoneGap core made it even harder.

### Lesson #1: Do not use technology that is relatively immature and rapidly changing.

- Not only is it hard to make things work, it's even harder to keep things from breaking. When a technology is rapidly changing at its core, a lot of the things you'll be learning is going to change in the very near future.
- It was also nearly impossible to find tutorials and documentation on the more obscure but commonly used aspects of the technology. There was a severe lack of libraries and limited community support.
- It's important to get good at shipping a product, but how can you get good when you're trying to hit a moving target?

After a couple of months or so, we decided to switch to coding in Java on native Android so the user could enjoy a more responsive and lag-free experience. I soon realized that working on Android in Java was painfully low-level, and the Android API itself was too rigid (not good for prototyping fast). Android was also prone to version fragmentation, so we experienced some API changes throughout our development. Getting the app itself onto the app store and making sure we had all the in-app payment and everything setup properly was also a hassle. There was also the issue that Google could change anything on us at any time.

There were two major lessons here:

### Lesson #2: Do not use technology that is slow for prototyping.

- Java as a language is known to be verbose. It is very slow in terms of prototyping things. It would always take me a very long time to get some very simple things set up.
- Learn a little bit about a lot of languages so you get a good idea of what's out there and what you can use.
- Do not be afraid to try things out in a different language. Putting in a little time to figure out what works best is likely to be worth it in the long run.

### Lesson #3: Do not develop in a market where there is a lack of control.

- Control is one of the core aspects of entrepreneurship. If Google can force you to fit its guidelines (by threat of banning you), that is a bad sign.
- Always build something in a domain where you have the freedom to operate.
- All the hassle dealing with a controlled market will slow you down.
	- There is only one way to make payments, to setup screenshots, etc.
- Note that this is less of a problem if you're already very familiar with the market and can already prototype very quickly (maybe because you are an expert in the domain).

## Inefficient Workflow

We were just two people with full-time occupations doing this on the side. What we lacked was a sense of urgency. We probably could have done everything we did in 20% of the time. There were months in which we made no progress at all. This is not only an issue with personality, but also of vision and values.

### Lesson #4: Set hard deadlines to build a sense of urgency.

- You need to prototype fast. You can't afford to spend months working on something that nobody is going to use.
- You need to focus on this one thing. You cannot have other hobbies, because it will only distract you from finishing.
	- You *can* ignore this lesson and do things as a side project, but just don't expect it to be successful.

## Lack of Market Need

There was no clearly defined target market. It just sounded like something cool that people would pay for. We were banking on people who might be desperate to know about how their relationships were like through their texts. However, it was obvious that we overestimated the market need for such a product. As Seth Godin put it, you need to understand the "tribe" that will become your loyal early adopters.

It wasn't enough to simply know that there might be desperate individuals out there. We tried to scratch our own itch because it was something cool that we wanted. That's all fine and good, but we should have prototyped and launched fast to really figure out how much market need there really was.

### Lesson #5: Do market research.

- Do not try to scratch your own itch without further research (or better yet, launching).
- Understand the people who you will eventually market to.
- Clearly define the "tribe"
	- Find out where they hang out
	- Be valuable to them.

----

# Marketing

## Pricing Too Low

We set a freemium model for the app because we were scared people would not download it. In retrospect, we should have charged full price for the app. We were very timid and were scared that nobody would buy it if we did that. I now think that if what you made is not worth charging full price on day one, then you are almost certainly doomed. Social networks and other types of entrepreneurship aside, note that we are focusing on solo entrepreneurship through coding apps or webapps.

### Lesson #6: Aim to charge on day one, or re-evaluate your idea.

- If you don't even believe in your product, why should anyone else?
- If you developed a product that was valuable, you should be able to see how you might pay for it yourself.
	- The fact that you are hesitant to charge for your product should get you to question whether your product is all that valuable to the market in the first place.

You could also use ads, but that is a mixed bag. Unless you know that people will have your app open for long periods of time, this is likely not going to work for you. This works well in games and radio apps, but for almost everything else it is unlikely to be a useful strategy.

## Lack of Focus

Admittedly, this is still an area which I don't know much at all. However, there is a lot of great content out there, and I intend to read it. When we were marketing this app, we just haphazardly sent it to a lot of people we knew along with some random bloggers. There was no strategy at all, and we probably should have asked for more advice on the web along with doing a lot more research on our part. Later on, we even dumped some money into Google Adwords and Facebook ad campaigns, but to no avail.

### Lesson #7: Have a focus and strategy in marketing.

- Don't just buy Facebook and Google ads haphazardly, but actually have a targeted approach.
	- Use long tail keywords.
- Make sure to calcualate the cost of acquisition and conversion, even if it's just an estimate.
- Look for the 'tribe' and seek out what others have done.

# Conclusion

<p class="message">Launching fast eliminates a whole class of problems.</p>

This was a very costly lesson. A year of your life (albeit mostly part-time) is a high price to pay. A lot of these problems and issues would have come to light much sooner if I set myself a a tight deadline and prototyped fast. I now realize that prototyping fast is not just for the sake of efficiency, but it also completely eliminates a whole bunch of problems.

In other words, a lot of the problems I experienced above could have come to light a lot sooner or even completely avoided if I only just prototyped faster and launched sooner.