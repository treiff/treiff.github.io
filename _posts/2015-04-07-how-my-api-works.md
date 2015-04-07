---
layout: post
title:  "How my Rails API works"
date:   2015-04-07 07:13:00
categories: blog
comments: true
---

I wanted to write a post basically describing the functionality of the Rails API I am building for a project.  It's been almost a week since I've worked on it and when I opened VIM I had that "wait, hows this work again" moment.  Needless to say, I made a few post and get requests to refresh my memory, I also figured it would be a good time to explain the inner workings.

Before we start, a little background.  There are three tables (users, jobs, api_keys), a user can have many jobs and api keys (multiple sessions).

Lets start with creating a user, then we'll create a job and can see how api_keys plays a role.  Here I am using the postman chrome extension for simplicity sake.

![Screenshot]({{ site.url }}/images/user_post.png)

So above, we post to ```api/vi/users``` with standard JSON headers. You can see we received a 201 response along with ```api_key``` object returned.  Okay, now lets walk through the process.

We send the post request with the new users parameters.  This is routed to the users create action as seen from the routing below.

```api_v1_users POST   /api/v1/users(.:format)    api/v1/users#create {:format=>"json"}```

The create method is pretty standard for rails, however if the user is saved we call the ```session_api_key``` class method on the user object.

{% highlight ruby %}
  def create
    user = User.create(user_params)
    if user.save
      render json: user.session_api_key, status: 201
    else
      render json: user.errors, status: 422
    end
  end
{% endhighlight %}

Back in the User model, we have this method. So basically, this is a method that is being called on a user, checking if that user already has a api_key that is scoped as a active, session.  In the ApiKey model, there are a few scope definitions.  That said, if any of the chained methods return nil, a new api_key will be created (which of course will happen in our case as it's a new user).

Also worth noting, is a ```before_create``` callback in the ```ApiKey``` model that generated a ```SecureRandom``` access_token as well as sets the token expiration depending on the keys scope.

So to summarize, when that new user was created, it immediately called its ```session_api_key``` method which since there was no api key for this new user yet, created one.  

{% highlight ruby %}
# app/models/api_key.rb

scope :session, -> { where(scope: 'session') }
scope :api, -> { where(scope: 'api') }
scope :active, -> { where('expired_at >= ?', Time.now ) }

...
{% endhighlight %}

{% highlight ruby %}
def session_api_key
  api_keys.active.session.first_or_create
end
{% endhighlight %}

Next, the session is created.  Keep in mind if there was already a session the current access_token would be returned and ```sessions#create``` would never be invoked.  

{% highlight ruby %}
def create
  user = User.where(email: params[:user][:email]).first
  if user && user.authenticate(params[:user][:password])
    render json: user.session_api_key, status: 201
  else
    render json: {}, status: 401
  end
end
{% endhighlight %}

Also, here you can see the server log of that single post.

![Screenshot]({{ site.url }}/images/user_post_server.png)

Now we have a user with a valid API key stored in our db.  Lets post a new job.  

![Screenshot]({{ site.url }}/images/job_post.png)

Here again is a basic post request that gets routed to ```jobs#create```.  I won't bother posting the create action, the thing to know is that there is a ```before_filter``` for certain actions including ```create```. This filter checks if a user is authorized?.  

This is accomplished by searching the ```apikeys``` table for keys that match the ```active``` scope we discussed earlier and where the access token matches that passed in the headers.  If the token matches, ```api_key.user``` is returned, if not it is considered a unauthorized request.  Here are a couple of screen shots showing the server logs when a new job is created along with the parsed response.

![Screenshot]({{ site.url }}/images/job_post_server.png)

![Screenshot]({{ site.url }}/images/json_response.png)

Also, to note if the session expires (4hrs now) and the user tries to create a new job that will return a unauthorized.  The user would then have to request a new token by posting to ```api/v1/sessions``` and the same create method above is run, retrieving the user and assigning a new token. 

Granted, now this all has to be piped into a front end that will have forms etc to do the dirty work for us. 

Hopefully this kind of made some sense.  I find it super helpful sometimes to walk though an example like this.  Also, as I frequently am reading blogs (a great resource for this can be found [here](http://coderberry.me/))and other code it's super important if your going to use parts of it to make sure they are fully understood.

Until, next time! 