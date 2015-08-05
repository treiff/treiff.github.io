---
layout: post
title:  "Real Programming"
date:   2015-08-04 19:34:00
categories: blog
comments: true
---

Okay, so this is going to be a fun post.  I really enjoy it when I write some code that does soemthing useful. I mean that's the whole point, but when sometimes our little CRUD operations and such can get stale.

That said, today I felt like a real programmer! Not sure exactly what that is, but you know most programmers like to solve real word problems and make things easier...Well today I accomplished these feats. I wrote my fist "useful" bash script.  I say useful because we've all written something that's echoed "Hello World" back to us.

Here's how this went down.  I've been in the process of upgrading one of our products at work from rails 2 to rails 4.  I'm at the point where I felt it would be good to get a "staging" environment that mirrors our production environment.  Essentially, get it going on a spare in-house physical server we have and make sure everything is playing nice in the production environment.

I recently "finished" getting the app running on passenger with nginx and was playing around, I quickly realized some of the changes I was making in dev I needed to restart the production server to make sure they took place.  This is how that was done.

![Screenshot]({{ site.url }}/images/nginx_kill.png)

Okay, so that shuts down the server..Oh, and before anyone asks because of the way nginx was installed on the server through ruby gems there is a different startup/shutdown procedure than typical. (from what I understand)

So, now we have to restart...

![Screenshot]({{ site.url }}/images/nginx_restart.png)

There, were done..Server restarted.  Well, I got sick of typing that all the time and decided to do something about it, enter the bash script.

{% highlight bash %}

#!/bin/bash

PID=`ps aux | grep '[n]ginx' | head -1 | awk '{print $2}'`
echo "Shutting down server ...PID = $PID"
if [ -z "$PID" ]; then
  echo "No process found."
else
  `sudo kill $PID`
fi
echo "Restarting nginx"
`sudo /opt/nginx/sbin/nginx`
PIDN=`ps aux | grep '[n]ginx' | head -1 | awk '{print $2}'`
if [ -z "$PIDN" ]; then
  echo "Something's not right!"
else
  echo "Server running ...PID = $PIDN"
fi

{% endhighlight %}

Now, a quick walk-through...We set a ```PID``` variable that basically finds the PID of the nginx process.  So, that bracket in the grep command actually searches for the character class ```n``` followed by ```ginx```.  More on that [here](http://askubuntu.com/questions/153419/how-does-this-tricky-bracket-expression-in-grep-work), this is piped in to return the first PID, followed by ```awk``` which I had never used, but is really helpful parsing rows and colums of data, we pass in the second item which is the PID...phew..

Okay, from here we have a simple conditional with the ```-z``` flag which will return true if the string length of ```"$PID"``` is 0.  Finally, we restart the server and check that it's running...Pretty simple actually.

And the results..

![Screenshot]({{ site.url }}/images/nx_restart.png)
