---
layout: post
title:  "Building a Gem"
date:   2014-12-23 11:11:11
categories: blog
comments: true
---

So one of my other hobbies, besides brewing beer is sailing.  I grew up on the water and was fortunate enough to have friends with boats.  After college my wife and I also bought a small sailboat and sailed it down to the caribbean for the winter.  Anyway, I quickly grew passionate about boats and their design.  

Long story short, I was looking for a gem that would provide the formula for some of these sailboat design ratios.  These can be useful when comparing similar boats.  I couldn't find one so I decided to build one which I plan to use in a future project.  

Getting started was simple as I already had bundler installed.  All I needed to do was tell bundler. (sailcalc is the gem name I built)

{% highlight bash %}
$ bundle gem sailcalc
{% endhighlight %}

Once completed you will have a new directory for your gem along with associated files.  I started by first modifying the .gemspec file.  It’s this file in which you define the name, authors, dependencies etc.  Given that I have been practicing TDD all I did was add a development dependency for rspec and change some of the naming. 

Inside your `lib/` directory is where most your code will end up.  There is also a file for versioning inside `lib/<gemname>/version.rb`

My gem is fairly straight forward, I changed from the “default” module provided to a class, and basically after writing my tests wrote out my class methods for the different ratios.  The reason I changed from a module to class was I figured at some point myself or someone else may want to modify the code so a instance of the class could be created.

One thing I would definitely recommend doing is to test your gem locally before publishing.  To do this you can run gem build, with the name of your gemspec file. Like so:

{% highlight bash %}
$ gem build sailcalc.gemspec
Successfully built RubyGem
Name: sailcalc
Version: 0.0.0
File: sailcalc-0.0.0.gem
{% endhighlight %}

You’ll notice the gem appear in the root of your project, after that all you have to do is install.

{% highlight bash %}
$ gem install ./sailcalc-0.0.0.gem
Successfully installed sailcalc-0.0.0
1 gem installed
{% endhighlight %}

Then just fire up irb, require your gem which should return true, and you're good to test it out. 

As for publishing, that was a little tricky for me, I have some issues getting my computer setup with my RubyGems account.  However, once this was sorted it was just a simple:

{% highlight bash %}
$ gem push sailcalc-0.0.0.gem
Pushing gem to RubyGems.org…
Successfully registered gem: sailcalc (0.0.0)
{% endhighlight %}

Overall, it was a pretty easy process...The gem has been up for about 24hrs now and it’s had around 50 downloads. I was hoping just one person may find it useful, so thats a bonus.  I plan to add some more formulas to it and improve the tests, but for now it will do.  Check it out here - [sailcalc](https://rubygems.org/gems/sailcalc).
