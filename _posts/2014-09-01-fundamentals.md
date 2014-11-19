---
layout: post
title:  "Fundamentals"
date:   2014-09-01 19:58:00
categories: blog
comments: true
---

Lately, I've been feeling a little stuck currently in my path to become a developer.  I have had a lot going on with my job and family life that my time spent coding has taken a hit.  I was averaging around 18-20 hours per week and now I am around 12-15 or so.  I don't necessarily feel guilty, actually given my current situation I am actually pleased with that, although I would prefer it to be 40/hrs a week.  The grand total is now up to 288 hrs. 
         
I feel I am at a point where I should find a mentor.  I feel I have the basics down.  I can do the usual things such a build a basic rails app, write a script to solve some computational problem, integrate an API, etc..However, this is just scratching the surface. I have done almost every on-line coding tutorial I can find.  Basically, I just need to dig deeper and be pushed harder. I know I can do this myself, however sometimes that extra push is all you need.  Ultimately, This is something I really want and I know I'll make it happen, it's just a matter of time.  
          
Other than that, I picked up [“Practical Object-Oriented Design in Ruby”](http://www.amazon.com/Practical-Object-Oriented-Design-Ruby-Addison-Wesley/dp/0321721330) by Sandi Metz.  This book is amazing, and I would highly recommend it to any programmer.  I am not quite done with it yet but the class design and managing dependencies has been super helpful. Also, been doing some more straight ruby.  I actually have a brewing rails app that I've been working on a little.  I just need to figure out where I want to go with it.  
        
Also, I came back to an [exercism.io](http://exercism.io/) exercise that I had struggled with a month ago or so and was able to solve it in about 10 minutes, so that was pretty cool.  I know the code isn't really dry...actually, anyone have any suggestions on that?  Have a look.
        
{% highlight ruby linenos %}
class Complement
  @dna_to_rna = { 'G' => 'C', 'C' => 'G', 'T' => 'A', 'A' => 'T', 'U' => 'A'}
  @rna_to_dna = { 'C' => 'G', 'G' => 'C', 'U' => 'A', 'A' => 'U', 'T' => 'A'}

  def self.of_dna(rna)
    result = []
    rna.split("").each do |char|
      if @rna_to_dna.has_key?(char)
        result << @rna_to_dna[char]
      else
        puts "ERROR"
      end
    end
    result.join
  end

  def self.of_rna(dna)
    result = []
    dna.split("").each do |char|
      if @dna_to_rna.has_key?(char)
        result << @dna_to_rna[char]
      else
        puts "ERROR"
      end
    end
    result.join
  end
end

{% endhighlight %}