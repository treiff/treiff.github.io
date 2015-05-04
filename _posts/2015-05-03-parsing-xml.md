---
layout: post
title:  "Parsing XML"
date:   2015-04-21 19:30:00
categories: blog
comments: true
---

I'm quickly realizing that once your working 40/hrs a week, taking care of a family working on personal projects...etc, one tends to quickly forget about updating their blog.  Fact is, I haven't forgotten but rather just have preferred to spend that time working on my rails beer recipe app or working through some Javascript and PHP books/exercises.  

That said, at work I worked a bit on a pre-processer for which basically parsed data from uploaded XML files.  This was all in PHP especially there was some data that was missing which I had to work on getting recognized.  

Working on this rejuvenated my interest in a a rails project I started a while back.  The cool thing is it would also let me work with some libraries that I have wanted to play around with but didn't have a good opportunity.  Short story is I am trying to get XML formatted beer recipes (can be exported from brewing software) uploaded and parsed into a easily readable format for sharing and exchange.

A few days ago I got [paperclip](https://github.com/thoughtbot/paperclip) up and running uploading files to AWS S3, that's all pretty simple and straight forward. 

![Screenshot]({{ site.url }}/images/new-beer.png)

Here you can see you login with facebook or standard, create a recipe and upload the XML file associated.  I don't like the flow as is.  In fact, this project started as some blogging platform idea..I would like to basically just have the file upload then parse important info from the file to be automatically saved and displayed, along with access to the original upload.  

![Screenshot]({{ site.url }}/images/browse-beers.png)

Here is once it's uploaded, if someone were to click on that link they get the XML file containing the recipe obviously.  
What I have begun playing around with is a before save callback that will parse the XML file.  This will hopefully work alongside with [nokogiri](http://www.nokogiri.org/) to help with parsing the file.  

I actually just started working on the logic for the callback and how exactly I am going to do it.  I actually thought about maybe doing it client side with javascript and still may go that route.  Either way, should be interesting and fun.

Looks like I'll have to follow up with a part II once I get this figured out..Suggestions? 