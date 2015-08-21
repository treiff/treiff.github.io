---
layout: post
title:  "PHP db migrations?!"
date:   2015-08-20 19:34:00
categories: blog
comments: true
---

The past few days while we've been waiting for some feedback on the rails project I usually work on I've had the opportunity to dig a bit more into our core libraries for our CMS.  This is all written in PHP and isn't my strong suit, but when asked if I could come up with some sort of migration system to update all the client db's I was pretty excited. 

Needless to say, I continued my streak of awesome bash scripts with another...Ha.. Like I mentioned before, I love writing these little procedural scripts.  There is something strangely satisfying about running them and watching them make your job easier.

So, without further ado...The script!  Now, let me explain...

![Screenshot]({{ site.url }}/images/bash.png)

I understand it's probably not super exciting for everyone else, however I'll walk through it and then you'll either have thought it's kinda cool or maybe left by now.  Anyway, like I mentioned the purpose of the script is to update different db's on different servers whenever we make a a global change.  Funny thing is, before this I was informed the method was to just manually update the db/table for each client individually! Like 50 of them!

Back to the script.  First, you call ```updatedb``` at the command line.  The script checks if any arguments were passed, if not we spit out the little usage blurb and exit.  Next, I define a function that loops through each client folder in the directory where we have all client projects.  You'll notice the ```maxdepth 1``` and ```type d``` flags specifying a directory and don't go deeper than the current search directory.  We set a ```RESULT``` variable to the basename of the ```$d``` path.  In this example ```$d``` will be a full path and the ```basename``` call will return the directory off the end.  

Now, we call a PHP script that migrates the DB.  It's actually pretty simple, and makes the connection then runs the direct SQL queries on the various MySQL databases. 

Lastly, you'll see at the bottom we execute the function depending on the parameter that was passed.  This determines the environment.  The cool thing is for instance if you chose ```web``` we actually ssh into the web server then call ```typeset``` with a -f flag which declares what were passing as a function, then simply call the function.

There you have it.  Now we can just keep a file with dated and labeled migrations and run the script.  I'm sure in the end there are many different ways I could have tackled this however, this was quick and pretty simple and best thing is it works!