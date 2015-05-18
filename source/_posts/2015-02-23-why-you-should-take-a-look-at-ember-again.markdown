---
layout: post
title: "Why you should take a look at Ember (again)"
date: 2015-02-23 08:36:24 +0100
comments: true
categories: [ ember, javascript, front-end, framework ]
---
> Update: The video from this talk given at [KarlsruheJS](http://karlsruhejs.org/post/113364758894/polymer-emberjs-tabrisjs) is now available [here](http://bit.ly/ember-video).

In January 2015 I gave a talk at the [FrankfurtJS](http://frankfurtjs.org) Meetup on this topic and it generated quite a bit of interest. The motivation behind it was actually quite selfish, I needed a bit of pressure to dive into Ember.js a bit more and what better way to do that than to try and explain it to a room full of people?

By now most front-end developers have certainly heard of Ember.js and many have even given it a try. I therefore tried to combine an introduction with a bit of current information on the framework as well as the Community around Ember.js.

There have been significant changes in the past 6 months that in my opinion warrant another look at the framework even if you have evaluated it thoroughly before.

<script async class="speakerdeck-embed" data-id="3d3f1dc0851801328fc62e7b8af14da2" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>
<!--more-->

Following is a brief summary of the talk and a list of all the resources that were discussed. The demo application that was written as part of the live coding session will be discussed in a separate post. If you want to take a look at the code you can do so for the [backend](https://github.com/hglattergotz/node-twitter-example) and the [Ember application](https://github.com/hglattergotz/ember-twitter-demo) itself.

##What is Ember.js?

Taken straight from the official website:

>A framework for creating ambitious web applications

What does that mean? Because Ember.js is highly opinionated (and proud of it) it allows for larger teams to be very productive with it. This comes from its stand on "Convention over Configuration", which basically means "The Ember way or the highway", but it also means that once you get over the admittedly steep learning curve you will be able to focus on building your application and won't be worrying about the mundane small stuff like where to put your files and how to name things.

Yehuda Kats gave a really good talk at [RailsConf](http://www.confreaks.com/videos/3337-railsconf-keynote-10-years) about the concept of "Cognitive Depletion". In a nutshell it means that the human brain only has a limited amount of cycles per day to make decisions and after that limit is reached you become fatigued and in turn less productive. He does a really good job of explaining this and even backs it up with scientific studies on this subject.

The strong conventions that Ember.js and [ember-cli](http://ember-cli.com) enforce also result in a project structure that lets new team members become familiar with a project very quickly.

And lastly *Ember.js is optimized for developer happiness.* Maybe this sounds cheesy, but this is what got my attention about Ember.js in the first place. In a talk or podcast that Tom Dale and Yehuda Kats were on they talked about their mission to create a framework that makes and keeps developers happy to do their job. This is a hard one to explain because it really has to do with every aspect of Ember.js and its community but once it clicks you will know what I mean.

##Who is behind Ember.js?

Yehuda Kats and Tom Dale are the two guys that started it, but as of March 2015 there are [11 other core team members](http://emberjs.com/team/) from a variety of companies and places around the world. And of course there are many community members that are contributers to Ember.js in a variety of ways.

##The Community

The Javascript community in general seems to be made up of a bunch of friendly enthusiastic people, but the Ember.js community in particular is extremely helpful and welcoming.

####IRC

On the #emberjs IRC channel I have never seen comments like "RTFM" or the likes. In fact, in the [opening keynote](https://www.youtube.com/watch?v=pON2erqemDY) at [EmberConf 2014](http://emberconf.com/) Tom and Yehuda spent considerable time on explaining the code of conduct for the Ember community and what it means to make people feel welcome. From my experience it shows.

####Questions & Discussions

[Stackoverflow](http://stackoverflow.com/questions/tagged/ember.js) is of course a great place to ask questions, I have [Alfred](http://www.alfredapp.com/) configured to look there for my programming questions.
For more involved discussions check out the [discussion forum](http://discuss.emberjs.com/) hosted on Discourse, which coincidentally is one of the first Ember.js apps to appear in the wild, and its [open source](https://github.com/discourse/discourse)!

####Conferences

There are at this point in time two Ember.js conferences. One is in the US - [EmberConf](http://emberconf.com) - and the other is in Europe - [Ember Fest](https://emberfest.eu/).

####Meetups

If you are fortunate enough to have an Ember.js meetup near you - GO! A list of the currently active meetups can be found [here](http://emberjs.com/community/meetups/).

##Useful Resources

####Website

Probably the best place to start is the [Ember.js website](http://emberjs.com). It has excellent documentation and tons of current examples. You will also find information about the current releases as well as upcoming releases.

####Newsletter

I find that the newsletter [emberweekly.com](http://emberweekly.com) is a great resource for staying on top of what is going on in ember land. It lists the developments of Ember.js as well as related projects and provides links to videos, blog posts and upcoming events.

####Podcasts

I get a lot of information from podcasts. They are a great source of current information directly from the people close to various projects and technologies.

Some of the more interesting ones for Ember.js developers:

* [__Ember Land__](http://www.ember.land/) is a brand new podcast dedicated to Ember.js
* [__Javascript Jabber__](http://devchat.tv/js-jabber/)
  There are some episodes dedicated to Ember.js. [121](http://devchat.tv/js-jabber/121-jsj-broccoli-js-with-jo-liss), [112](http://devchat.tv/js-jabber/112-jsj-refactoring-javascript-apps-into-a-framework-with-brandon-hays), [111](http://devchat.tv/js-jabber/111-jsj-the-ember-js-project-with-erik-bryn)
* [__Frontside the Podcast__](https://frontsidethepodcast.simplecast.fm)
  This is a fairly Ember.js centric podcast since the hosts are Ember.js consultants (well, they do most of their work in Ember.js).
* [__The Changelog__](http://thechangelog.com/)
  Episode [131](http://thechangelog.com/131/) is an interview with Tom Dale and Yehuda Katz
* [__Descriptive__](http://descriptive.audio/)
  Episode [2](http://descriptive.audio/episodes/2) is a really interesting discussion about refactoring to Ember.js

####Videos

* The Ember.js [Meetup in NYC](https://www.youtube.com/user/EmberNYC) is probably one of the most professional I have found.
  The meetings are all recorded and core team members speak there on a regular basis.
* EmberConf 2015 has a [playlist on YouTube](https://www.youtube.com/playlist?list=PLE7tQUdRKcyacwiUPs0CjPYt6tJub4xXU)
* EmberConf 2014 has a [playlist on YouTube](https://www.youtube.com/playlist?list=PLE7tQUdRKcyaOyfBnAndJxQ9PNVmKva0d)
* Ember London has a good [collection](https://vimeo.com/emberlondon/videos) of videos
* EmberFest 2014 has published a [playlist on YouTube](https://www.youtube.com/playlist?list=PLN4SpDLOSVkSbGTLohVaYGDB8hxWxGPBA)

##Tools

####ember-cli

In my opinion [ember-cli](http://www.ember-cli.com/) is probably one of the most significant improvements to come out in the last 6-12 months. What I mean by that is that it is a huge factor in reducing the barrier of entry to Ember.js. The reason for this is that it eliminates an number of complicated tasks required for getting an Ember.js production app going and lets you get right to it. Not only does it take care of the entire build process, but it also provides an application skeleton, a dev server, a watcher to rebuild your app, auto-reload in the browser, ES6 modules and a proxy to allow you to make calls to your backend.

If you are starting a new app, use ember-cli, period.

####Ember Inspector

There is a handy tool called the Ember Inspector for both Chrome and Firefox that provides insights into your Ember.js application, that not only helps with debugging but provides a visual representation of many of the applications components.

##Noteworthy features

####Ember Data

Ember data will allow you to talk to your backend API with very little effort IF it follows the conventions that Ember Data expects. So, if you are on a green field project where you are building your own API or if you have control of the API you are in luck.  But even if you have to use an existing API it might be possible to write custom serializers and still take advantage of Ember Data. In any case it is worth a look! It is very close to 1.0.0.

#### Components

[Ember components](http://emberjs.com/guides/components/) are trying to adhere to the official Web Components Specification as closely as possible with the intention to allow for a painless migration of the Ember Components to the W3C standard components that can then be used in other frameworks as well.

####Fastboot

There is currently an effort under way to implement a feature called [Fastboot](http://emberjs.com/blog/2015/01/08/inside-fastboot-faking-the-dom-in-node.html), which aims to provide server side rendering of the initial page. This effort will provide the ability load HTML and CSS immediately with the Javascript downloading in the background to provide fast initial page loads.

####Maintainability

The Ember.js project has adopted a six week release cycle very similar to the process that Google Chrome uses.  This provides a clear and predictable path forward with small incremental changes that make it manageable to keep your applications up to date with the most current framework release.  Because the changes are small-isch it is not as time consuming to move from one release to the next and does not require full rewrites.

##Live Coding Demo

The demo application will be discussed in one or more future posts. It consists of a [node.js backend](https://github.com/hglattergotz/node-twitter-example) and the actual [Ember.js frontend application](https://github.com/hglattergotz/ember-twitter-demo).

