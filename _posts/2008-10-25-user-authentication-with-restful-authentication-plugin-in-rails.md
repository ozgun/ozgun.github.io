---
title: User Authentication with restful-authentication Plugin in Rails
tags: rails
layout: post
---

This is a tutorial about how to integrate [restful-authentication](http://github.com/technoweenie/restful-authentication/tree/master) plugin to your Ruby on Rails application. restufl-authentication is a plugin that makes user authentication easy for you. Installation and configuration is rather simple.

Before starting, I want to give information about my system. I’ve Linux Ubuntu 7.04 installed on my computer. Installed Ruby version is 1.8.6. And the Rails version running on my desktop is 1.2.1.

### Preparing/Installing Requirements

First of all, we’ll handle requirements by installing required gems. This step is necessary if you plan to use rspec tests in your rails apps. Since our installation requires this feature, first we need to install the following 2 gems.


{% highlight bash %}
$ sudo gem install rspec
$ sudo gem install rspec-rails
{% endhighlight %}

Create your rails app which will be used during this tutorial. I called it “restauth”. You are free to give any name you like. For the sake of simplicty i chose to use “sqlite” instaead of “mysql” database. If you want to use mysql rather than sqlite, run the “rails” command with “-d mysql” parameter. After creating your rails app, change your current working directory to your newly created rails app.


{% highlight bash %}
$ rails restauth
$ cd restauth
{% endhighlight %}

### Installation


{% highlight bash %}
$ cd vendor/plugins
$ git clone git://github.com/technoweenie/restful-authentication.git restful_authentication
$ cd ../..
$ ./script/generate authenticated user sessions --include-activation --stateful --rspec
{% endhighlight %}

Install `acts_as_state_machine` plugin.


{% highlight bash %}
$ ./script/plugin install http://elitists.textdriven.com/svn/plugins/acts_as_state_machine/trunk 
{% endhighlight %}

Running the commands above will add some files to our rails app and also add piece of code to existing files.

Routes created: (config/routes.rb)


{% highlight ruby %}
map.logout '/logout', :controller => 'sessions', :action => 'destroy'
map.login '/login', :controller => 'sessions', :action => 'new'
map.register '/register', :controller => 'users', :action => 'create'
map.signup '/signup', :controller => 'users', :action => 'new'
map.resources :users
map.resource :session
{% endhighlight %}

Models generated:


{% highlight text %}
app/models/user_mailer.rb
app/models/user_observer.rb
app/models/user.rb
{% endhighlight %}

Controllers generated:


{% highlight text %}
app/controllers/sessions_controller.rb
app/controllers/users_controller.rb
{% endhighlight %}

Views generated:


{% highlight text %}
app/views/sessions/*
app/views/user_mailer/*
app/views/users/*
{% endhighlight %}

Migration generated: (xxxxxxxxxxxxxx depends on the time you create migration)


{% highlight text %}
db/migrate/XXXXXXXXXXXXXX_create_users.rb
{% endhighlight %}

rspec tests and stories generated:


{% highlight text %}
spec/
stories/
{% endhighlight %}

So far so good. If you still haven’t run into any errors it’s time to run migration:


{% highlight bash %}
$ rake db:migrate
{% endhighlight %}

Add user activation routes to `config/routes.rb` file. (Because we used `—include-activation` parameter.)


{% highlight bash %}
map.activate '/activate/:activation_code', 
             :controller => 'users', 
             :action => 'activate', 
             :activation_code => nil
{% endhighlight %}

We used “—stateful”, so we need to add an observer to enviroments and resource for users to routes.

Add user observer to `config/environment.rb` file (inside “Rails::Initializer.run do |config|” block)


{% highlight bash %}
config.active_record.observers = :user_observer 
{% endhighlight %}

Add users resource to `config/routes.rb` file.

	
{% highlight bash %}
map.resources :users, :member => { :suspend   => :put,
                                   :unsuspend => :put,
                                   :purge     => :delete }
{% endhighlight %}

### Configuration

As you may remember, we had used “—include-activation” parameter while we were installing the plugin, so we need to make a few quick and easy changes.

Now, open the file `app/models/user_mailer.rb` and find and replace `YOURSITE` with your website url, `ADMINEMAIL` with your email address, `[YOURSITE]` with your website’s title/name.


{% highlight bash %}
@body[:url]  = "http://YOURSITE/activate/#{user.activation_code}"
@body[:url]  = "http://YOURSITE/"
@from        = "ADMINEMAIL"
subject     = "[YOURSITE] "
{% endhighlight %}

If you don’t want to use randomly generated site key, you could change the value of `REST_AUTH_SITE_KEY` located in `config/initializers/site_keys.rb` file to something you want.


{% highlight bash %}
REST_AUTH_SITE_KEY = "Life, Don't talk to me about life!"
{% endhighlight %}

More Customization

If you want to change the contents of the activation email or signup notification mail sent to users, feel free to alter the following templates to meet your needs.


{% highlight bash %}
app/views/user_mailer/activation.erb
app/views/user_mailer/signup_notification.erb
{% endhighlight %}

Default installation of the plugin doesn’t enable the “Remember Me” option. However, you don’t need to worry about it, it’s so very easy to enable it. Just edit the file `app/views/sessions/new.html.erb` and uncomment the following lines.


{% highlight bash %}
<p><%= label_tag 'remember_me', 'Remember me' %>
<%= check_box_tag 'remember_me', '1', @remember_me %></p>
{% endhighlight %}

Show Time!

Now, fasten your seatbelt and prepare to take off!


{% highlight bash %}
$ ./script/server
{% endhighlight %}

Signup: Make sure your mail server is running and enter a valid email address of yours.  

{% highlight bash %}
hppt://localhost:3000/signup
{% endhighlight %}

Login

 
{% highlight bash %}
htpp://localhost:3000/login
{% endhighlight %}

That’s all. If you have any questions about this tutorial don’t hesitate to ask. Thanks for reading.

### Resources:

* [restful-authentication at github](http://github.com/technoweenie/restful-authentication/tree/master)
* [Restful Authentication with all the bells and whistles](http://railsforum.com/viewtopic.php?id=14216)
