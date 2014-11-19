---
layout: post
title:  "It's Working...Finally!!"
date:   2014-05-26 22:13:00
categories: blog
comments: true
---

For the last few days I have been struggling getting my one month rails app to upload pins to amazon's S3 service.  Basically, I wouldn't get any error but just a broken link icon. I tried for a couple of hours the first night debugging it but just got frustrated and moved on to some other ruby exercises. Yesterday, I went back starting fresh looking for a solution.  No luck, no matter what I searched for I couldn't find anything.  I even checked my code against the one month rails source and couldn't find any differences.

So tonight I started really digging into it, I had send an email off to one month rails and got a couple of suggestions which I tried and didn't work.  Finally, I found a problem...

{% highlight erb linenos %}
  <div class="form-group">  
   <%= f.label :image %>  
   <%= f.file_field :description, class: "form-control" %>  
  </div>  
{% endhighlight %}

See the problem there...Well, I must have cut and pasted from another line and forgot to change "description" in f.file_field to "image". Anyway, saved it, pushed it...error..

Now, I started getting errors about a missing bucket.  Fast forward an hour of so and I realize in the production environment I had:
{% highlight ruby linenos %}
 :bucket => ENV['S3_BUCKET']  
 instead of  
 :bucket => ENV['AWS_BUCKET']
{% endhighlight %}  

I went ahead and fixed this error, went through the github/heroku dance, opened the page and another error. Looking into heroku logs I see:

{% highlight iex %}
 2014-05-27T01:40:46.117103+00:00 app[web.1]: Paperclip::Errors::MissingRequiredValidatorError (Paperclip::Errors::MissingRequiredValidatorError):  
 2014-05-27T01:40:46.117104+00:00 app[web.1]:  app/controllers/pins_controller.rb:21:in `create'  
{% endhighlight %}

A little while later along with some advice from stackoverflow and the paperclip documentation, I find you now need a validator for the image attachment in the pin model.  I added this and what do you know....Success!

Aside for the rails app I am working on, I've been doing some of "ruby the hard way" which is great so far.  I've found it to be fairly easy but granted I am not that far along.  I suppose some of the other ruby tutorials/books/courses I've taken are paying off.  Till next time...