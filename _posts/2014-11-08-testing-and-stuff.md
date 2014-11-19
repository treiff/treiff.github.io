---
layout: post
title:  "Testing and stuff"
date:   2014-11-08 19:29:30
categories: blog
comments: true
---

Lately, things have been going great with my development progress. I’ve been getting in a solid 6 - 9 hours per day during the week which has been awesome.  I realized how much more efficiently I can work when I am focused on one thing and it’s not just for an hour here and there.  

I’ve actually been mixing it up quite a bit, spending a bunch of time with some projects from [upcase](http://www.upcase.com) which have been great.  Mostly going through their video tutorials etc focusing on BDD with Capybara and Rspec.  Up to this point I haven't spent a lot of time with test at all really.  I did a little bit messing around with Rspec and test unit but thats it.  

Here is a quick example of a “smoke test” basically a super broad test to see if things are functioning properly. 

{% highlight ruby linenos %}
require 'rails_helper'

feature "User visits homepage" do
  scenario "successfully" do
    visit root_path

    expect(page).to have_css 'h1', text: 'Todos'
  end
end
{% endhighlight %}

So, the nice thing here that I really like is the test will fail and then you work your way forward writing just enough code to get the test to pass.  Most likely you’ll encounter multiple errors while trying to run the test and each time you’ll add a new bit of code to get to the next error until your green..pretty sweet.  

{% highlight ruby linenos %}
require 'rails_helper'

feature "user sees own todos" do
  scenario "doesn't see others' todos" do
    Todo.create!(title: "buy eggs", email: "someone_else@example.com")

    sign_in_as "someone@example.com"

    expect(page).not_to have_css ".todos li", text: "buy eggs"
  end
end
{% endhighlight %}

Heres another test, I am sure just by reading through it you’ll understand what it does.  That is another great thing about capybara is it’s super easy to read (at least at this level). 

Aside from that, I’ve been spending some time revising some old exercises with javascript and jQuery that I struggled with in the past.  Also I finally got my dotfiles setup how I want and feel ive gotten the hang of vim and tmux enough now that I am faster with it that I was with sublime text...More on that in another post.