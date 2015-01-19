---
layout: post
title:  "Tweet Data -- Part I"
date:   2015-01-19 11:17:00
categories: blog
comments: true
---

For a couple of days I have been working on a basic site that would allow someone to enter a twitter handle then would pull some data using the twitter [REST API](https://dev.twitter.com/rest/public) , do some “stuff” to that data and show you a pretty graph.  When I say “stuff”, for instance I mean something like figure out the average tweets per day for the user.  This is all information that is there, we just need a way to sort it to our liking.  

My last post began with some issues I had getting my environment variables setup, well not really issues but more of a general service announcement.  Anyway, what I am building is a basic sinatra app with basically three routes and some helper methods for making the calculations.  

Note:  I keep adding new things I want to graph or show visually, thus my gemfile is growing and I am starting to pull from multiple API’s.  I’m really just building this to mess around, but it’s teaching me a ton which is great!

I am using a few gems to make this happen.  see my gemfile below:

{% highlight ruby %}
source "https://rubygems.org"

gem 'sinatra', "~>1.4.5"
gem 'twitter', "~>5.13"
gem 'chartkick', "~>1.3.2"
gem 'geocoder', "~>1.2.6"
gem 'groupdate', "~>2.4"
gem 'httparty', "~>0.13"
{% endhighlight %}

Sinatra, that’s a given.  The [twitter gem](https://github.com/sferik/twitter) which gives so great methods for pulling data from the API.  [Chartkick](https://github.com/ankane/chartkick) is what I am using to display most of this data, you can do a lot of customization which is pretty awesome.  [Geocoder](http://www.rubygeocoder.com/), this is for another route where you can input a search term, location and radius and a list of tweets will be returned.  This feature is on the back burner while I work on the first part (figuring a users avg tweets per week day).  That brings me to [group date](https://github.com/ankane/groupdate). 

Now, I can get back to really the whole point of this post...I had an issue with groupdate. Could be operator error, I acatuly opened my first github issue regarding it [here](https://github.com/ankane/groupdate/issues/72).  Let me walk you through the problem.

{% highlight bash %}

2.1.5 :011 > timeline = CLIENT.user_timeline("tlreiff", :count => 200, :exclude_replies => true)   
 => [#<Twitter::Tweet id=555177339097989122>, #<Twitter::Tweet id=553733567722061825>, #<Twitter::Tweet id=553174629981102080>, #<Twitter::Tweet id=552637098621435904>, #<Twitter::Tweet id=551147036545122304>, #<Twitter::Tweet id=55003427
8403563522>, #<Twitter::Tweet id=548146688339476483>] ……..

2.1.5 :012 > timeline.class
 => Array

2.1.5 :013 > days_hash = timeline.group_by_day_of_week { |t| t.created_at }
=> {0=>[], 1=>[#<Twitter::Tweet id=547069643920125952>, #<Twitter::Tweet id=536714696649629698>, #<Twitter::Tweet id=531822153562599424>, #<Twitter::Tweet id=529400501755191296>, #<Twitter::Tweet id=521796741839196160>, #<Twitter::Tweet id=519204949163384832>, #<Twitter::Tweet id=518913626275848192>], 2=>[#Twitter::Tweet id=..............]}

2.1.5 :014 > days_hash.merge(days_hash) { |k, v| v.count }
 => {0=>0, 1=>19, 2=>27, 3=>21, 4=>25, 5=>21, 6=>15} 

{% endhighlight %}

Note:  I truncated the tweet returns above because there are a couple hundred..

So, we have a variable “timeline” that is an array of tweets, each tweet contains information about that particular tweet.  The CLIENT global variable is used in my app, I just cut and paste it (contains my api keys and other identifying info so i can use the api).  The #user_timeline method is provided by the twitter gem which takes a twitter handle as the first argument along with some options.  

Great, so now we have an array of tweets.  For my chartkick gem to display data, especially when not using a database it wants that data in a hash or an array of arrays.  I created the variable days_hash calling #group_by_day_of_week method provided by the groupdate gem.  This returns a hash with keys corresponding to the days of the week (0 = sunday, 1 = mon...etc).  

Next I call #merge on the days_hash, merging with itself and passing in the block to return counts of tweets for the different days.  So, do you see the problem with the return value?

I guess I have never tweeted on a Sunday...Now I know I am not a huge twitterer (word?) but I am pretty sure I have tweeted on a sunday...Lets go back to the timeline array and see..
{% highlight bash %}

2.1.5 :032 > count = 0                                                                                                                                                                                                                        
 => 0 
2.1.5 :033 > timeline.each do |tweet|                                                                                                                                                                                                         
2.1.5 :034 >     if tweet.created_at.wday == 0                                                                                                                                                                                                
2.1.5 :035?>     count += 1                                                                                                                                                                                                                   
2.1.5 :036?>     end
2.1.5 :037?> end

2.1.5 :038 > count
 => 16 

{% endhighlight %}

So indeed there is something going on here.  I tried playing around with timezone etc, thinking that may be it, but eliminated that.  Thus, I opened the issue on github.  Since I was still waiting on a response and wanted to keep going on the project, I figured I could just make my own method to do it for me...This is what I came up with, I actually like it better because I can use the weekday abbreviation vs [0..6].  

I should be able to refactor this, but for now it gets me working again.  What I don’t like about it how for the key I have [tweet.created_at.strftime(“%a”)] three times.  

{% highlight ruby %}
    
    def tweet_day(array)
      day = {}
      array.each do |tweet|
        if day.has_key?(tweet.created_at.strftime("%a"))
          day[tweet.created_at.strftime("%a")] += 1
        else
          day[tweet.created_at.strftime("%a")] = 1
        end
      end
      day
    end

{% endhighlight %}

Here is the result..

{% highlight bash %}
2.1.5 :041 >     def tweet_day(array)
2.1.5 :042?>         day = {}
2.1.5 :043?>         array.each do |tweet|
2.1.5 :044 >               if day.has_key?(tweet.created_at.strftime("%a"))
2.1.5 :045?>                 day[tweet.created_at.strftime("%a")] += 1
2.1.5 :046?>               else
2.1.5 :047 >                   day[tweet.created_at.strftime("%a")] = 1
2.1.5 :048?>               end
2.1.5 :049?>           end
2.1.5 :050?>         day
2.1.5 :051?>       end
 => :tweet_day 
2.1.5 :052 > 
2.1.5 :053 >   tweet_day(timeline)
 => {"Tue"=>22, "Fri"=>20, "Thu"=>23, "Mon"=>26, "Wed"=>23, "Sat"=>9, "Sun"=>16} 

{% endhighlight %}

And finally, it in action rendered in view…

![Screenshot]({{ site.url }}/images/graph.png)

Obviously, this isn't styled yet at all...I even show the return value from the tweet_day method just to make sure the chart corresponds.  

That’s about it, next part I want to briefly discuss the [twittercounter](http://www.twittercounter.com) API and how I used HTTParty to work with requests.