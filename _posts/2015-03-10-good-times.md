---
layout: post
title:  "Good Times"
date:   2015-03-10 09:06:00
categories: blog
comments: true
---

We began our "capstone" projects at [CodeUnion](http://codenunion.io) last week, so needless to say I've been cranking away on that.  I had a really hard time figuring out what to build.  I was only seeing the finished product, which in reality would probably be version 10.  Basically, I'll struggle to think of something, and when I finally do it's crazy complicated.  

The longer I write code the more I have realized how important simplicity is.  That's not to say you won't have complicated pieces, but more when trying to tackle a new project, rather than "think" you need "this" and "that" just define your MVP, write up some user persona and user stories and build absolutely the simplest thing possible.  Expand from there.

Over the past few days, this is my v1 release of my "dinner gathering" app.  I haven't been able to think of a cool name yet.  Although, when I deployed to heroku, I wanted to change the default subdomain provided by heroku.  I tried "dinner party", but that was taken...Eventually, I looked up some synonyms to "dinner" and found "major munch".  For some reason I got a kick out of that so decided to go with it.  Okay, enough rambling...heres the app:

![Screenshot]({{ site.url }}/images/mm_home.png)

![Screenshot]({{ site.url }}/images/mm_dinner.png)

![Screenshot]({{ site.url }}/images/mm_rsvp.png)

So, what is it.  Basically, you go to the site, create a dinner event. This provides you with a custom URL to e-mail to friends who may want to attend.  From there, friends will click the link opening the RSVP page. Here you would enter your name along with what your bringing.  Simple. 

Obviously, there is some styling that needs to be refined especially on the RSVP page (where you see the dinner details). As this was my MVP I just wanted to "ship it".

One thing I feel you quickly realize when your building anything is things are never as easy as your think they will be.  A couple of the more difficult things with this project so far have been the custom URL along with nested forms. 

The URL was a little tricky, I ended up creating a private method in my ```Dinner``` model that generates the unique URL.  Here is my dinner model:

{% highlight ruby linenos%}
class Dinner < ActiveRecord::Base
  validates :email, email: true, case_sensitive: false, presence: true
  validates :name, presence: true
  validates :date, presence: true
  validates :time, presence: true
  validates :location, presence: true

  has_many :courses
  has_many :guests

  accepts_nested_attributes_for :guests, reject_if: ->(a) { a[:name].blank? }
  accepts_nested_attributes_for :courses, reject_if: ->(a) { a[:name].blank? }

  attr_readonly :email, :name, :date, :time, :location

  before_create :generate_unique_url

  def custom_url
    "http://majormunch.herokuapp.com/dinners/#{unique_url}/edit"
  end

  # Overrides rails default to_param
  # When params called on a dinner object unique_url
  # replaces :id
  def to_param
    unique_url
  end

  private

  def generate_unique_url
    self.unique_url = SecureRandom.hex
  end
end
{% endhighlight %}

On line 31 you can see the method used to generate the unique URL string.  There is a ```before_create``` that calls the method and populates the column in the db.  Also, on line 25 the ```to_param``` method overrides rails default ```id```.  So, something like ```dinner_url(@dinner)``` will now use ```Dinner#unique_url``` vs. ```Dinner#id``` as the URL parameter.

Now in my controller, I can have something like:

{% highlight ruby %}
  def edit
    @dinner = Dinner.find_by_unique_url(params[:id])
  end
{% endhighlight %}

Currently, I have three tables ```dinners```, ```guests``` and ```courses```.  I was trying to separate the classes as much as possible and ensure "single responsibility"  however, I am beginning to think it may make more sense to just have the ```dinners``` table along with maybe a ```rsvps``` table.  This way the ```RSVP``` handles the guest name along with what they are bringing.  This would also simplify my nested forms and relationships.  

Also, my method there on line 47 with the custom URL, I am pretty sure it probably shouldn't be in the model.  I am going to be working on my V1 re-factor the next couple of days, so I should be able to clean this up a bit.  

This project has been a lot of fun, practicing time boxing features knowing when to find help. I was able to build this fairly quickly and I realize if I never focused on the simple MVP, there still wouldn't be anything to show.  

I was planning on writing a little about my nested forms, but I'll save that for next time along with explaining some of my refactors.