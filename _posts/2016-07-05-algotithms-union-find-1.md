---
layout: post
title:  "Algorithms: Union Find(part 1)"
date:   2016-07-05 20:50:00
categories: blog
comments: true
---
I've decided to spend some time and work on getting a better understanding of algorithms and system design. I find the topic super interesting and think there are tremendous benefits to learning and applying these algorithms.  So, I begin my foray into algorithms with [courseras](https://www.coursera.org/) algorithms course from Princeton which so far has been amazing.

I'm planning on doing a write up on most of the algorithms I'm learning, this helps me understand and keep the material fresh in my head, so stay tuned.

###Union-Find (disjoint set)

Basically, I’m going to discuss three different variations of the union find algorithm. 

 * Quick-Find
 * Quick-Union
 * Weighted Quick-Union (with path compression)

I’ll try and point out the pros, cons and complexities of each.  All these variations will share two common methods, a `connected(p,q)` and a `union(p,q)` method, both of which take the array index of the nodes we’re comparing or uniting. Enough rambling, lets have a look.

###Quick-Find

We’ll start with probably the simplest of the three, the quick-find.  Here is the class initializer which will initialize the object with `N+1` nodes.

{% highlight ruby %}
class QuickFind
  attr_accessor :node_array

  def initialize(arry_size)
    @node_array = Array (0..arry_size)
  end
end
{% endhighlight %}

This should give us an object containing our array. Visually, this would look something like this:

![initialized array]({{ treiff.me }}/images/initialized_array.png)

As it stands, we can think of each node as its own set, at this point nothing is connected.  Lets add the `union` method and do some union-ing?.

{% highlight ruby %}
class QuickFind
  attr_accessor :node_array

  def initialize(arry_size)
    @node_array = Array (0..arry_size)
  end

  def connected(p,q)
    node_array[p] == node_array[q]
  end

  def union(p,q)
    return "already connected" if connected(p,q)
    p_val = node_array[p]
    q_val = node_array[q]
    node_array.each_with_index do |v, i|
      next if v != p_val
      node_array[i] = q_val
    end
  end
{% endhighlight %}

So, actually this is the complete class as I’ve got `union` calling the `connected` method to see if  nodes are connected before continuing. So we’re going to union a few nodes then come back and look at the method.

![quickfind output]({{ treiff.me }}/images/quick_find_output.png)

First, we call `qf.union(3,2)`, we want to set the value at array index 3 to 2. First, is the call to `connected` checking if the values at index p equals value at index q. If they are connected, we return from the method.

Moving on, we find the p and q’s values based on the passed in array index. Next, we iterate over the array and check each value against the `p_val` if they are equal we know we have to change the value at that particular array index to the `q_val`.

![quickfind complete]({{ treiff.me }}/images/quick_find_complete.png)

I'm sure your starting to notice the problem with this implementation, finds are quick O(1) array access which is great, however every `union` command needs to iterate over the whole array replacing every node with a value of `p` with the value at `q` making this O(n).  This algorithm has the potential to be a `quadratic` or 
run in O(n^2) time, assuming the `union` function costs `n` accesses and we have `n` `union` operations.

Okay, that's about all i've got for now. I'll post a link to part II (quick-union and weighted quick-union) once I have it up.
