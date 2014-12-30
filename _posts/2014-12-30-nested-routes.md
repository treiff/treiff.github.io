---
layout: post
title:  "Nested Routes with Devise"
date:   2014-12-30 12:37:00
categories: blog
comments: true
---

I have been spending a lot more time on my brewing app lately.  I really want to get all the backend sorted so I can work more on how it's best to present.  I had one issue the other day I thought might help others.  When I first started building the app I used [devise](https://github.com/plataformatec/devise) to setup my User model along with associated views/controller.  

This was great until I decided I wanted to add some more info into the User model.  Basically, I am using the [geocoder](http://www.rubygeocoder.com/) gem to interpret addresses to interact with the gmaps API.  I figured the easiest thing would be to create a migration for the User model and add the address column.  No big deal. 

Everything was working nicely, you would just add your location -- (city, state..lat/long..etc) when registering, the field was added to the form in the generated devise view.  Ok, enough rambling.  

Later, I wanted to create another model with brew sessions (yes, my app is beer related).  The brew session model thus belongs to user.

{% highlight ruby %}
class BrewSession < ActiveRecord::Base
  belongs_to :user
end
{% endhighlight %}

Also, the user model has many brew sessions.

{% highlight ruby %}
class User < ActiveRecord::Base
  has_many :brew_sessions
  ...
end
{% endhighlight %}

I knew that in my routes I would need to have brewsessions nested within users.

{% highlight ruby linenos %}
Rails.application.routes.draw do
  resources :users
    resources :brew_sessions
  end

  devise_for :users

  root 'pages#index'
  match 'help',     to: 'pages#help',      via: 'get'
  match 'contact',  to: 'pages#contact',   via: 'get'
  match 'dashboard', to: 'dashboard#index', via: 'get'
end
{% endhighlight %}

This created these routes.

![Screenshot]({{ site.url }}/images/2014-12-30-1.png)

Here is the problem, because devise created its necessary routes when generating the user model, the code above basically created duplicate users routes, so now we have a conflict between devise and these new routes.  Here is how I got around it.  

{% highlight ruby linenos %}
Rails.application.routes.draw do
  resources :users, :only => :none do
    resources :brew_sessions
  end

  devise_for :users

  root 'pages#index'
  match 'help',     to: 'pages#help',      via: 'get'
  match 'contact',  to: 'pages#contact',   via: 'get'
  match 'dashboard', to: 'dashboard#index', via: 'get'
end
{% endhighlight %}

This creates the routes below, and now we see the user routes (not associated with devise) are removed.  By adding the :only => :none. Here we are telling rails not to create any routes for users, but created the nested routes for the brew sessions needed.  

![Screenshot 2]({{ site.url }}/images/2014-12-30-2.png)

I am sure there is a way to mess with the devise routes to work around this a different way, I was just looking for the simplest solution that worked for me, and so far so good with this one.  Hopefully, this helps anyone out in the future dealing with devise and routing.