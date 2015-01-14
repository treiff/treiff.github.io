---
layout: post
title:  "Fun with API's"
date:   2015-01-13 20:12:00
categories: blog
comments: true
---

Been super busy lately, was able to get most of my beer brewing stand complete which we all know is super important.  Also, Ive been working a lot on some [codeunion](http://codeunion.io) prep-work, just trying to get my mind back in the ruby mindset.  

One of the things involved in the work was building a basic sinatra app.  I actually had never messed with the sinatra framework until a couple days ago.  My first impressions is that its great, especially for more of a beginner.  One thing I really like about it is how easy it is to wrap your head around some of the routing and parameters which definitely seem to be more convoluted in rails.  

I decided I was going to build some sort of small sinatra app, using the twitter rest API essentially as my backend.  So far so good, I am amazed how quickly you can throw something together with sinatra.  I am not sure what the ultimate goal of this project is, but its giving me some good experience dealing with external APIs and the associated challenges.  

Here is screenshot from the app.

![Screenshot]({{ site.url }}/images/twitter_api.png)

The cool thing about it is I gotten to work with a bunch of different gems (twitter, geocoder, sinatra, chartkick).  Having to read all the docs etc and then figure it out is really a great way to learn..For me anyway.  So far I have sinatra setup with an index that has forms for sending two different GET requests depending on the input.  

Internally that data is then sorted and graphed using chartkick..I shouldn't say sorted but I've extracted what I want and show a graph.  I'm barley scratching the surface, but it's amazing how much data is available and with some thought the kind of trends etc, that could be figured out.  

I'm still working on the app, there is no styling and it could use some refactoring.  I want to keep going on it because it's such a fun project to mess around with.  Other than that, I am meeting with a former colleague to help him with a web-site. Should be interesting, pressures on!