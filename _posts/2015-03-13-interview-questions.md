---
layout: post
title:  "Interviews"
date:   2015-03-14 08:26:00
categories: blog
comments: true
---

Now that I've started interviewing for a developer position, I've had the opportunity to answer different technical questions (or try my best).  For the most part I found the questions fair and interviewees easy to talk to and willing to explain if the response wasn't exactly what they're looking for.  

I've found the conversations great opportunities to think about my own code and projects and where I could improve.  One recent question pertained to a recent project I've been working on (see last post).  Essentially, it was along the lines of:  "hypothetically, you have two models that both have the same column, call it 'description' and you want a way to show that description in the view in 'pig-latin' how would you do that?" (paraphrasing)

I immediately thought model, however since this pig-latin method would have to apply to multiple models, it doesn't make sense to write it twice in two different models (i.e not DRY). I then knew that some helper would be necessary.   I thought of the ```app/helpers``` directory but was pretty sure that was just for view helpers and controllers/models didn't have access.

Also, I briefly thought possible the controller but nixed that idea.  Generally, it's my understanding we should be keeping our controllers as "thin" as possible. Also, it just didn't seem to make sense here. 

So, in the end I knew I wanted a helper a couple of different models.  

Ultimately, it's pretty simple that kind of logic would go in your ```lib``` directory as a module which could then be included in your models. Something like:

{% highlight ruby %}
# lib/pig_latin.rb
module PigLatin
  def pig_latin
    # my pig latin conversion method
  end
end
{% endhighlight %}

{% highlight ruby %}
# app/models/dinner.rb
class Dinner
  include PigLatin
  ....
end
{% endhighlight %}

{% highlight ruby %}
# config/application.rb
...
  config.autoload_paths += %W(#{config.root}/lib)
{% endhighlight %}

The last snippet is included as rails 4 doesn't automatically load the files in the lib directory.  This is just a basic example, now I could ```include PigLatin``` in the models I want.  

I wouldn't say in my interview I exactly got this correct.  Once we began discussing modules I totally had a flash-back of doing this on the zipshare project.  Although, in that case we created a class that generated unique tokens.  

Cool, so that kind of covers basic helper modules.  It's worth touching on view helpers which is where you could extract some kind of display logic...i.e -> iterating over a array of names and displaying them. In this case, your best bet would be a view helper.  

Overall, it was nice to get some feedback and ways to improve my code.  I find every-time I am in a situation like above it ends up being a great learning opportunity.  I would definitely encourage anyone who is getting feedback on their code, or just having casual conversations pertaining to programming to dig a little deeper, read up on the subject a bit after.  Most of time when I do this, I'm surprised just how much I learn.