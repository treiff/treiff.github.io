---
layout: post
title:  "Environment Variables"
date:   2015-01-16 08:33:00
categories: blog
comments: true
---

This post may be pretty basic, but it's something that I dealt with a little when I first started and was working through the [one month rails](https://onemonth.com/) course.  Also, I just plan to cover one of the many ways to setup your environment variables.  

So why environment variables.  Short story is your have a project using some kind of service that wants a key, password, username...etc.  These are all things you probably don't want to be be public in your repository when you push it to github.  

Enter the environment variable.  As far as I know this method works on unix systems (Mac, Linux), I am not sure about windows.  So the first thing you want to do is you need to know which shell your using (bash, zsh, something else).  If you run: 

{% highlight bash %}
# Inside shell

echo $SHELL
=> /bin/zsh
{% endhighlight %}

Also, I belive most shells will tell you at the top of the shell window.  So now we know the shell, in my case zsh.  So all we have to do is open your .zshrc or if using bash .bashrc and add a line similar to below. 

{% highlight bash %}
# Twitter API environment variables

export CONSUMER_KEY="my key here"
export CONSUMER_SECRET="my key here"
export ACCESS_TOKEN="my token here"
export ACCESS_TOKEN_SECRET="my token here"
export BEARER_TOKEN="my token here"
{% endhighlight %}

As you can see, where inside the quotes would be our keys, or password, email etc.  You then save this file, restart your shell, then inside your project you would have something like this:

{% highlight ruby %}
# Inside your project

CLIENT = Twitter::REST::Client.new do |config|
  config.consumer_key             = ENV["CONSUMER_KEY"]
  config.consumer_secret          = ENV["CONSUMER_SECRET"]
  config.bearer_token             = ENV["BEARER_TOKEN"]
end
{% endhighlight %}

This is actually and example from a sinatra app I am working on utilizing the twitter API.  So we just reference the variables using the ENV["variable name"] syntax.  And we're done, now you don't have to worry about your private keys, passwords getting compromised.

Lastly, I will say there are other ways to do this, and this may not be the best way depending on your situation.  There are a few gems [dotenv](https://github.com/bkeepers/dotenv) and [figaro](https://github.com/laserlemon/figaro) which can help as well, there are probably even more out there.  So check those out, depending on your situation they may be a better fit.