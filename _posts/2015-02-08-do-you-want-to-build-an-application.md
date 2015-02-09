---
layout: post
title: "Do you want to build an application?"
description: ""
category: 
tags: []
---
{% include JB/setup %}

### How this all started

When you are *good with computers*, every now and then someone will get to you and suggest that you build an app for [this](https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewSoftware?id=284400182&mt=8) an [that](https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewSoftware?id=301579705&mt=8). It's often hard to explain that, yes, [the simple app](https://itunes.apple.com/WebObjects/MZStore.woa/wa/viewSoftware?id=299879416&mt=8) will take more than 15 minutes to develop and that no, it won't get you rich because a lot of people will pay the 99 cents. It's usually easy to get over the 45 second attention span that this brilliant app idea deserve by simply pointing to one of the [stupid little apps](http://www.pcmag.com/slideshow_viewer/0,3253,l=236567&a=236567&po=1,00.asp) that already exists.

But sometime, someone has an idea that deserves a little more consideration. One of my friend who that plays in local hockey leagues pitched me his idea about an application to help the league's manager reach backup players. Today, of course, everything is an app so no matter what it does, it's got to be on a phone. Looked like a fun project with real users that wanted to get the app so we made a deal and registered [maligue.ca](maligue.ca).    

A fun project but not that simple. We needed a web site, a mobile app, integration with email, sms and payment services. So I reached out to one of my friend and asked: [Do you want to build an application?](https://www.youtube.com/watch?v=5xGEMyn4DKY).

### Sooooo... we are building an application

Ok, now what? We are both [professional](http://8d.com/) developpers... java developpers. But java isn't fun anymore. I mean, I love java. I have developed a [lot](http://sonia.etsmtl.ca/en/) [of](http://www.grassvalley.com/) [stuff](https://montreal.bixi.com/) in java. But it ain't the cool kid anymore and I wanted to learn something new. I like robust, maintenable, typed language. I have strong opinions and i'm a little stubborn. For example, I hate python just because the space indentation reminds me [COBOL](http://en.wikipedia.org/wiki/COBOL), I hate javascript just because it's [javascript](http://johnkpaul.github.io/presentations/empirejs/javascript-bad-parts/#/) and don't talk me about CSS since this isn't real coding anyway. *But* this is just a pet project and we are here to ship something and have a little fun doing it.

I played a little with [nodejs](http://nodejs.org/) and [coffeescript](http://coffeescript.org/) to build a [robot to watch hudson/team city builds](https://github.com/MacKeeper/jobot) and loved it. I'm not sure exactly why but I liked it. Almost like java when I started using it 15 years ago. The amazing amount of ~~stable~~ libraries available for node js is impressive and coffeescript is like a breath of fresh air when you compare to javascript. 

### Choosing a stack

So it is nodejs and coffescript. Well, let's write some code! Wait a minute... there is nome than a vm and a language.

* Scaffolding ([what the heck this means anyway?](http://en.wikipedia.org/wiki/Scaffold_%28programming%29)): [sails.js](http://sailsjs.org/#/), [yeoman](http://yeoman.io/)
* Framework: [sails.js](http://sailsjs.org/#/), [meteor](https://www.meteor.com/), [barebone express](http://expressjs.com/)
* Architecture: [single page app](http://en.wikipedia.org/wiki/Single-page_application), traditional client-server
* Templating: [Jade](http://jade-lang.com/), [\{\{ mustache }}](https://mustache.github.io/)
* Client side framework/MVVM: [AngularJS](https://angularjs.org/), [http://emberjs.com/](http://emberjs.com/), [http://knockoutjs.com/](http://knockoutjs.com/)
* Style: [SAAS](http://sass-lang.com/), [{LESS}](http://lesscss.org/)
* Other: [Bootstrap](http://getbootstrap.com/)