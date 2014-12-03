---
layout: post
title:  "Javascript Testing"
date:   2014-12-03 08:41:12
categories: blog
comments: true
---

The more time I’ve been spending with ruby and speaking with other developers is the notion of TDD or BDD.  I came to the conclusion that I should be testing my code.  One thing I like most is how it forces me to think more about what exactly I am trying to accomplish with my code.  That said, I have been trying to implement integration and unit tests in all my code when it’s practical.  

More recently, working with javascript I was looking for a testing solution and came across [Mocha](http://mochajs.org/) and [Chai](http://chaijs.com).  I stumbled across these while looking through the epicodus [curriculum](https://www.learnhowtoprogram.com/lessons/learn-how-to-program).  This post is basically a summary on the js testing setup.

![screenshot]({{ site.url }}/images/2014-12-03-2.png)

So, here you see the result of running the mocha tests.  I used mochas browser output feature but you can also run tests in the terminal if you would like.  If you look at the image below, this shows my screen layout where I have HTML to the right and my script on the bottom left with the spec about it.  

![screenshot2]({{ site.url }}/images/2014-12-03.png)

So, you're probably wondering how to set it all up, that was the point of the post.  So here is how I did it. I am sure it’s not the only way but this worked for me. 

clone or download the mochajs repo on github [here](https://github.com/mochajs/mocha).
Create a spec folder in your projects root if you don’t already have one.  Copy the mocha.js and mocha.css files to the spec folder.
visit the chaijs page [here](http://chaijs.com).  Where it says “download chai” click on browser, then click the download link.  Now you can just save the page to your spec directory.
Create a new html file in your spec folder called “spec-runner.html”.  On the mocha homepage you’ll see a link in the documentation for “browser support” this will bring up a section of html that you can paste into your “spec-runner” file.

Now, your pretty much setup except for a couple of things.  When I first tried this, i was getting errors because my spec-runner file wasnt setup properly.  Here is how I ultimately set mine up.

{% highlight html linenos %}
<html>
  <head>
    <meta charset="utf-8">
    <title>Mocha Tests</title>
    <link rel="stylesheet" href="mocha.css" />
  </head>
  <body>
    <div id="mocha"></div>
    <script src="../js/jquery-1.11.1.min.js"></script>
    <script src="../js/scripts.js"></script>
    <script src="mocha.js"></script>
    <script src="chai.js"></script>
    <script>mocha.setup('bdd')</script>
    <script src="specs.js"></script>
    <script src="test.array.js"></script>
    <script src="test.object.js"></script>
    <script src="test.xhr.js"></script>
    <script>
      should = chai.should();
      mocha.checkLeaks();
      mocha.globals(['jQuery']);
      mocha.run();
    </script>
  </body>
</html>
{% endhighlight %}

Basically, it’s a little different from what you'll find on the site.  You need to have your jquery setup (in my case in a js directory), also you have your actual script file (in my case scripts.js).  Then you have your mocha, chai and specs files.  

So now, all you have to do is write some specs in your spec.js file and you can open the spec-runner in a browser and you're good to go!  