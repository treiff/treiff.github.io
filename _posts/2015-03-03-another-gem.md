---
layout: post
title:  "Another Gem"
date:   2015-03-03 19:25:00
categories: blog
comments: true
---

Have you ever been sitting at your computer coding, say perhaps on a Thursday or Friday.  Suddenly, you realize your missing all the PGA tour action! Seriously, this must happen all the time.  Rather than have to pull up some website to check why not just have this info ready and available in the console. So, I present to you PGA leaderboard!

![Screenshot]({{ site.url }}/images/pga2.png)

Check it out, obviously it's a must have development tool.  It's great running in its own tmux pane, updating scores every three minutes...What more could you ask for.  Okay, all joking aside it actually was a great exercise for me building another gem, this time with an executable and multiple classes.  Granted it's still super basic, but it definitely make me think some more about design.

The first unique challenge was figuring out where to extract the leaderboard data.  I actually spent quite a bit of time searching but every API I could find was big $.  I later combed though the PGA tour sites leaderboard page and managed to figure out their JSON feed.  However, after reading their policy that was going to be a "no go".  

I finally came across a old blog post about ESPN's bottomline widget.  Basically, their is a endpoint which provides a URI encoded string.  Heres a screen-shot of what that looks like.

![Screenshot]({{ site.url }}/images/pga3.png)

As you can see it's a bunch of player data with your typical ```%20``` for spaces along with some id's.  I ended up making two classes, "Tournament" and "CLI".  The tournament class handles creating response objects with the help of the httparty gem.  Tournament has some methods available to essentially cleanse the data and package it neatly in an array.  

It was actually pretty fun playing around with ```gsub``` some more and regex's.  You can tell ```gsub``` could be extremely powerful if you know how to use it.  

With the "CLI" class, I used ```thor``` for the first time to build my executable.  Thor's documentation is pretty good and it wasn't to difficult to create the executable.  The trickiest thing for me eventually was figuring out how to update the leaderboard every three minutes.  I chose three because that is how often ESPN updates the bottomline feed. 

{% highlight ruby %}
module PgaLeaderboard
  ENDPOINT = "http://sports.espn.go.com/sports/golf/bottomLineGolfLeaderboard"
  INTERVAL = 180

  def run
    while true
      yield
      sleep INTERVAL
    end
  end
end
{% endhighlight %}

Basically, I created this ```run``` method which will execute whatever I pass in the block where it says yield.  Problem I was originally having was I was caching an instance of ```PgaLeaderboard::Tournament``` so the program would sleep then when it ran it was the same instance of ```PgaLeaderboard::Tournament``` where it should have been a new one.  

This is my "CLI" class and what I came up with, here you can seen where I have the ```run``` method.

{% highlight ruby linos %}
require 'thor'

module PgaLeaderboard
  class CLI < Thor
    require 'colorize'
    include PgaLeaderboard

    attr_reader :tournament

    def initialize(*args)
      super
    end

    desc 'leaderboard', 'see current tournament leaderboard'
    def leaderboard
      run {
        @tournament = PgaLeaderboard::Tournament.new
        output(tournament)
      }
    end

    private

    def output(array)
      puts
      tournament.heading
      puts "--------------------------------------------".colorize(:red)
      puts
      tournament.body
      puts
    end
  end
end
{% endhighlight %}

This ended up working nicely.  However, there is one catch though.  If ESPN ever changes their bottomline feed I'll have to rework the program.  Also, the feed is fairly limited but gets the job done.  

So, there you have it.  Crazy stuff. I really enjoyed building this, didn't take much time and it's pretty much exactly what I was looking for.  I'm sure millions out there feel the same way.. :)