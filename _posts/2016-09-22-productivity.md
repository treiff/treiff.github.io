---
layout: post
title:  "Quick productivity hack"
date:   2016-09-22 21:10:00
categories: blog
comments: true
---

## Here’s the problem

### JRuby + Rspec + rvm + tmux = ¯\\_(ツ)_/¯

Let me explain, we use `JRuby` and test with `Rspec`. Problem is, you can’t run `Rspec` in `JRuby` and we need to switch rvm over to `mri` to run the specs. 

Add to this, I typically divide up one of my monitors with multiple windows split via either tmux or vim.

At least a couple of time a day, I would find myself  running `rspec` only to get:

![rspec]({{ treiff.me }}/images/rspec.png)

This is because through my file shuffling etc, the shell I happen to be in at the time is still running `JRuby`.

## The solution

I wanted to be able to run something like `rspec spec/models/user_spec.rb` and if inadvertently in the wrong shell, automatically switch to the proper ruby and gemset and
run the spec.

So, what is happening when I run `rspec` in `JRuby`? Well, the system is looking for the executable `rspec` in the selected gem path.  In my case,
`~/.rvm/gems/jruby-1.7.20.1@ne_profile/bin`. Why not add a `rspec` executable there? Actually, I'm sure there are reasons not to do this..(I did say hack right?).

## Bash script
{% highlight bash %}
#!/bin/bash

echo Ooops! You can\'t run your specs with JRuby!

if [[ -s "$HOME/.rvm/scripts/rvm" ]] ; then
  source "$HOME/.rvm/scripts/rvm"
elif [[ -s "/usr/local/rvm/scripts/rvm" ]] ; then
  source "/usr/local/rvm/scripts/rvm"
else
  printf "ERROR: An RVM installation was not found.\n"
fi

echo Switching rubies...
rvm use 2.2.2@ne_profile
echo
echo Running: rspec $1
echo
rspec $1 $2 $3
{% endhighlight %}

## The result

![bash_result]({{ treiff.me }}/images/bash_result.png)

So, like I said a little hacky, I'm actually going to look into a better solution, but this works for now. 
