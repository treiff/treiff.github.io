---
layout: post
title:  "Code Union - So far so good!"
date:   2015-01-29 18:50:00
categories: blog
comments: true
---

I am nearing the completion of my second week enrolled with [code union](http://codeunion.io). Things have been great so far, I figured I would write a quick post about what we've been up to and the things I am learning.

Before I even enrolled in the program, I got a great impression from my discussions with one of the founders [Jesse](https://twitter.com/jfarmer).  He was very helpful answering any questions I had and from our short pairing exercise I could tell he was a great teacher.  So I decided to give it a go.

Code union offers a few different work shops, I am enrolled in the 8-week professional engineering session.  The focus is primarily rails, TDD and professional engineering.  It's that last item that so far has been a great learning experience.  Really, when I decided to enroll I was hoping for something that would give me a idea, or feel for real world team projects.  The program has delivered so far in that respect.

So, what do I mean by that.  Lots of things.  In our group sessions, which we do twice a week for 2 hours each (although they tend to run long) we practice [Mob programming](http://mobprogramming.org/mob-programming-basics/).  We've been using [zoom](http://www.zoom.us) for our sessions which is great, it allows anyone to drive, has great video, sound etc.  I've learned a ton about work flow, our team sessions are treated like that of a dev team and there are very high standards to producing production quality code.  

One thing in particular I realized was that my Git work flow was pretty bad.  I would create a project, work on it, commit/push...repeat.  Ut-Oh, yeah this was all on branch master.  Now, I know that's kind of a lame mistake but it's deeper than that.  For instance, new work flow is more like.

* ```git co -b <working branch>``` 
* work on feature
* ```git add -p```
* ```git commit``` (no commmit -m)
* ```git co master```
* ```git difftool <master> <working branch>```
* ```git merge <working branch>```

What are we doing here.  Ok, we checkout a new branch for our feature.  Great, now we cant screw up master. Next, we work on our feature and when were ready to commit again we add -p?  Basically this will take you chunk by chunk through your staged files giving you the option to stage.  See screen-shot below:

![Screenshot]({{ site.url }}/images/gitadd.png)

Here you see the ```git add -p``` command which then shows me version a (---) and version b (+++).  As you can see I removed ```10.times do``` replacing it with ```100.times do```.  It then asks if you want to stage the hunk.  

Now what...Well, we've added we should commit. so we ```git commit```.   Wait, why not ```git commit -m```. A couple of reasons.  First, you get to look again at what files are staged for commit. Yes, you should know by now by adding etc. But it never hurts to see once more. Second, you can take time to craft your commit message, since you editing in vim, you can make mistakes and change them before you save and commit.  See below for what that looks like.

![Screenshot]({{ site.url }}/images/commit.png)

Okay, finally were almost done.  Now we want to merge, I've setup [kaleidoscope](http://www.kaleidoscopeapp.com/) as a git difftool.  So when I run the command ```git difftool <master> <workingbranch>```.  Kaleidoscope will open comparing any changes between the two branches. 

![Screenshot]({{ site.url }}/images/kalid.png)

Everything looks good, and we merge.  I think the great thing about an approach like this is you get to see your diff's multiple times, your commit messages are better, and your 100% when you merge you know what your changing.  

That's just a simple example of one type of thing were learning about.  And by no means is this the only nor the best way for your git work flow.  This is just an example that I have adopted which is working for me.

I'll definitely be posting more about things I am learning with code union.  I kind of got carried away here with the git work flow...next time!  

  

