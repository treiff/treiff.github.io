---
layout: post
title:  "Module Enumerable"
date:   2014-12-11 13:40:00
categories: blog
comments: true
---

Today I had a revelation.  Lately, I have been doing a lot of coding challenges such as [code wars](http://www.codewars.com) and [exercism.io](http://exercism.io).  I like going back to some of the ones I had difficulty solving in the past and see if I can solve them.  For the most part I usually can, it’s great to get that positive reinforcement showing that I am actually learning new things even though sometimes it feels like it’s slow going.

The other reason I like doing these little coding challenges is often I am forced to dig into the documentation.  I think its easy to get stuck in the mindset of the methods you know and try to figure out your problem using those methods.  However, often (at least for me) there is some better method and today I found it!

Someone told me once that when looking for a simpler method or way of doing something check module enumerable first.  Basically, enumerable is a [mixin](http://en.wikipedia.org/wiki/Mixin) that allows classes that include #each to use.  Today, it finally dawned on me how awesome the #inject is.

So, for instance I am trying to find the sum of all the array elements.  Here is how I may have done it in the past.

{% highlight ruby %}
array = [1, 2, 3, 4, 5]

sum = 0

array.each do |item|
  sum += item
end
{% endhighlight %} 

Running this, we return 15, yay! Now, here is in my opinion a much cleaner way to solve the same problem.  This time using enumerable #inject.

{% highlight ruby %}
array = [1, 2, 3, 4, 5]

array.inject { |sum, item| sum + item }
{% endhighlight %}

This also returns 15.  Basically, what is happening is the block will execute 5 times once for each element in the array.  Each iteration through the block we will set sum equal to sum + whatever item were on...So the sums would be (1, 3, 6, 10, 15) as we iterate through the block.  

One even simpler way to write it would be.

{% highlight ruby %}
array = [1, 2, 3, 4, 5]

array.inject(:+)
{% endhighlight %}

You can check out the [Enumerable docs](http://ruby-doc.org/core-2.1.5/Enumerable.html) and see all the cool things you can do.  I didn’t really get enumerable #inject when I started out but now that I do its really changed the way I think about problems.