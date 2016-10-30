---
layout: post
title:  "Ruby Hash Iteration"
date:   2016-10-29 20:07:00
categories: blog
comments: true
---

So a couple weeks ago I realized a cool way you can iterate over hashes in ruby.  This is probably nothing new for a lot of people, but awesome nonetheless.
For some reason I needed the index, so I'll show that in my example here.

## Basic iteration

{% highlight bash %}

2.2.3 :005 > h
 => {:a=>1, :b=>2, :c=>3, :d=>4}
2.2.3 :006 > h.each_with_index do |p, i|
2.2.3 :007 >     puts "Object is: #{p}, with index of: #{i}"
2.2.3 :008?>   end
Object is: [:a, 1], with index of: 0
Object is: [:b, 2], with index of: 1
Object is: [:c, 3], with index of: 2
Object is: [:d, 4], with index of: 3
 => {:a=>1, :b=>2, :c=>3, :d=>4}

{% endhighlight %}

Okay, so what's the problem there.  Actually, nothing really if that's the output you want.

I was hoping to be able to iterate over the hash and fetch keys and values direcly,
without doing something like `p[0]` or `p[1]` to obtain the key and value. (referencing example above)

## Solution

{% highlight bash %}

2.2.3 :009 > h.each_with_index do |(k, v), i|
2.2.3 :010 >     puts "Key is: #{k}, with value of #{v}, with index of: #{i}"
2.2.3 :011?>   end
Key is: a, with value of 1, with index of: 0
Key is: b, with value of 2, with index of: 1
Key is: c, with value of 3, with index of: 2
Key is: d, with value of 4, with index of: 3
 => {:a=>1, :b=>2, :c=>3, :d=>4}

{% endhighlight %}

Notice the parentheses passed in the block. Obiously, the output difference is clear.

## Performance

I was curious as to if there were any performance difference between the two. So I wrote a quick benchmark script.

{% highlight ruby %}

require 'benchmark'

n = 5_000_000
TestHash = {a: 1, b: 2, c: 3, d: 4}

def parentheses(n)
  n.times do
    TestHash.each_with_index { |(k, v), i| b = "1" }
  end
end

def no_parentheses(n)
  n.times do
    TestHash.each_with_index { |k, v| b = "1" }
  end
end

Benchmark.bm do |x|
  x.report("no_parentheses:") { no_parentheses(n) }
  x.report("   parentheses:") { parentheses(n) }
end

{% endhighlight %}

![bench]({{ treiff.me }}/images/bench.png)

Turns out, virtually no difference in performance. Technically, with parentheses was slightly slower (very slightly).  This example is `5,000,000` iterations.

Just thought this was cool, hopefully it helps somone else out.
