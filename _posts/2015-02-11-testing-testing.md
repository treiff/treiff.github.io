---
layout: post
title:  "Testing - getting better?"
date:   2015-02-10 19:49:00
categories: blog
comments: true
---

A lot of what I've been focusing on lately is testing my code.  Now, I didn't always do this, in fact I actually didn't really do this at all until fairly recently.  I suppose you could say I had a on-off relationship with testing. I am happy to announce, that lately this has evolved into an "on" relationship.  Let me explain.

When I first started testing, I was following along with a tutorials or reading the docs it was easy to get failing tests and write the code necessary to make them pass.  Where this becomes a little more difficult is once you take the training wheels off and are implementing it in your own project.  

There seem to be a lot of questions to ask.  Where do I start?  Do I write an integration spec, and work "outside in" or do I start with some unit tests? Where do I put these tests?  Ughh, lots to think about and that doesn't even factor in some of the extra baggage. e.g. helpers, factories, setup etc.

Like everything I have experienced so far in web development is that it's a evolving process.  It wasn't until recently that I actually started writing my tests first.  In fact sometimes if a feature is particularly difficult for me I will actually implement the feature and then test it.  I know that's backwards and as I get better I am confident that will change.  Although, lately I have been forcing myself to just write the test and figure it out!

So, where was I.  For me my primary testing tools are rSpec, capybara, factory girl, faker and database cleaner if needed.  I found there to be quite a bit of setup which deserves a post in and of itself. Lets get to an example, here is a recent example of part of a feature spec I wrote for user sign-in.

Keep in mind, these tests are probably far from perfect but they do accomplish the goal of testing what I want them to test, I typically go back after and re-factor my tests after they are green.

{% highlight ruby linenos %}
# spec/features/create_new_user_spec.rb

require 'rails_helper'

feature "User signs up" do
  before(:each) do
    visit(root_path)
    click_link 'Sign up'
  end

  scenario "with valid credentials" do
    valid_sign_up_credentials
    click_button 'Sign up'

    expect(page).to have_content("Logged in as:")
    expect(page).to have_css(".alert-success")
  end

  ....
{% endhighlight %}

The spec itself is pretty straight forward, however there is one thing I'd like to point out and that's the `valid_sign_up_credentials` line.  So, what I did there is extract building a valid user into its own method.  You can see that below.

{% highlight ruby linenos %}
# spec/support/features/session_helper.rb

module Features
  module SessionHelpers
    def valid_sign_up_credentials
      user = build(:valid_user)

      fill_in "Name", with: user.name
      fill_in "Email", with: user.email
      fill_in "Username", with: user.username
      fill_in "Password", with: user.password
      fill_in "Password confirmation", with:  user.password_confirmation
    end
  end

  .....
{% endhighlight %}

As you can see, I am calling the build method provided by factory girl, to build a valid user.  That `:valid_user` would live in `/spec/factories/`. Also, in order for rSpec to use this helper, we have to tell it where to find it.

{% highlight ruby linenos %}
# spec/rails_helper.rb

  # Include helper module for sessions
  config.include Features::SessionHelpers, type: :feature
  # Include helper module for omniauth
  config.include Features::OmniAuthMocks, type: :feature
{% endhighlight %}

Ok, now back to the feature spec.  We build the user which fills in all the form fields clicks sign up then expects a couple of things to happen.  I found you need to have good understanding of what exactly the feature your testing is going to do, as well as have your architecture planned out in terms or the model etc. After implementing the feature and getting my tests green, I would typically move onto the model specs. Something like.

{% highlight ruby linenos %}
# spec/models/user_spec.rb
require 'rails_helper'

RSpec.describe User, :type => :model do
  it "has a valid factory" do
    expect(create(:user)).to be_valid
  end

  it "is invalid with out a email" do
    expect(build(:user, email: "  ")).to_not be_valid
  end

  ....
{% endhighlight %}

Again, fairly simple.  Here were using factory girl as well, along with faker to create the user.  I found that typically I seem to have 3 to 4 times as many unit tests as I do integration tests.  Lastly, I'll move onto controller specs as seen below.

{% highlight ruby linenos %}
require 'rails_helper'

RSpec.describe UsersController, :type => :controller do

  describe "POST create" do
    context 'with invalid credentials' do
      it 'renders new action' do
        user = create(:user)
        post :create, user: { email: user.email, password: user.password, password_confirmation: 'ut-oh' }

        expect(response).to render_template(:new)
      end
    end

    context 'with valid credentials' do
      it 'creates user' do
        user = create(:user)

        expect{ post :create, user: attributes_for(:user) }.to change(User, :count).by(1)
      end

      it 'redirects to root path' do
        user = create(:user)
        post :create, user: attributes_for(:user)

        expect(flash[:notice]).to match(/^Welcome to brewblog/)
        expect(response).to redirect_to root_path
      end
    end
  end

end
{% endhighlight %}

Here were testing the create action.  Again, you'll see some factory girl methods.  One we can do here is call the REST method (post) and specify the action `:create` followed by the parameters.  Also, you'll notice down near the bottom I used `attributes_for` which is a factory girl method.  Essentially, that is just shorthand for doing something like.

```
user: { email: user.email, password: user.password, password_confirmation: 'ut-oh' }
```

`attributes_for` will just return a hash with whatever parameters you setup in your factory.  

Well, so far that's the rough outline I follow when creating specs.  It seems to be working for the most part, I am definitely learning a lot about rSpec and testing methods, along with finding new ways to improve my code so overall it has been a lot of fun.

One last thing I'll say is, testing has made my development process slower, but as a result I've noticed the quality of my code is better and I am finding new and better ways to re-factor.  I'll take the temporary decrease in productivity for better code any day.