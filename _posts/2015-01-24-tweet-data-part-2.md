---
layout: post
title:  "Tweet Data -- Part II"
date:   2015-01-24 11:55:00
categories: blog
comments: true
---

A couple of days ago, I wrote about the "tweet analyzer" I was working on.  Really, it was just an excuse to play around with a popular API and sinatra.  Going into the project, there was no real ultimate goal...However, there were a couple of small goals.

  1. Learn something new, this goes without saying it it happens all the time, easy one!
  2. Practice with HTTP request methods
  3. Pull data from the API
  4. Interpret data from API
  5. Visually represent data.
  6. Try and make it as simple as possible.

In the finished product, all the goals were accomplished.  Since there was never really that "ultimate goal", it's slightly random in the different data it visually represents.  However, it's very easy to create another method to modify the data in some way then visually graph it.  

Also, if you remember from my last post I was having an issue with the [groupdate](https://github.com/ankane/groupdate) gem (see previous post for details).  Anyway, the I ended up removing it from the project and just writing my own methods to do what the gem was essentially doing.  

In a way, the nice thing about not having a endpoint for the project is I can just add more easily, not feeling bound by some constraint.  That is actually what I did with the [twittercounter](http://www.twittercounter.com) API along with [HTTParty](http://johnnunemaker.com/httparty/). 

Here's an example using the httparty along with the twittercounter. 

{% highlight ruby %}
def get_user_history
  HTTParty.get("http://api.twittercounter.com/?apikey=#{ENV["TC_KEY"]}&twitter_id=#{@id}")
end
{% endhighlight %}

This is pretty simple stuff, just a method that sends the request to twittercounter returning a hash (in this case with dates/follower_counts).  One thing about the JSON returned, the date was a strange format that had text in front of it.  I ended up writing another method to "scrub" this text from the date.

{% highlight ruby %}
def scrub_date(hash)
  scrubbed_hash = {}
  hash.each do |key, value|
    scrubbed_hash[key.gsub(/[a-z]/, '')] = value
  end
  scrubbed_hash
end
{% endhighlight %}

Thats about it, most of the graphs I used could take a hash as a parameter, which made displaying the data easy.  Heres what it looks like.

![Screenshot]({{ site.url }}/images/graph2.png)

In the end, this was a fun project with lots of opportunities to experiment and play around with new things.  I definitely learned a lot.  Check it out live here, [tweet data](https://tweetdata.herokuapp.com/).  Also, the repo is [here](https://github.com/treiff/t_api).  Keep in mind styling wasn't a priority with this project :) . 