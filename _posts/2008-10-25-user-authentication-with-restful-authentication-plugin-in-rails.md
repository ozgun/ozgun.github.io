---
title: User Authentication with restful-authentication Plugin in Rails
tags: rails
layout: post
---

This is a tutorial about how to integrate restful-authentication plugin to your rails application. restufl-authentication is a plugin that makes user authentication easy for you. Installation and configuration is rather simple.

Before starting, I want to give information about my system. I’ve Linux Ubuntu 7.04 installed on my computer. Installed Ruby version is 1.8.6. And the Rails version running on my desktop is 1.2.1.

Preparing/Installing Requirements

First of all, we’ll handle requirements by installing required gems. This step is necessary if you plan to use rspec tests in your rails apps. Since our installation requires this feature, first we need to install the following 2 gems.


$ sudo gem install rspec
$ sudo gem install rspec-rails
Create your rails app which will be used during this tutorial. I called it “restauth”. You are free to give any name you like. For the sake of simplicty i chose to use “sqlite” instaead of “mysql” database. If you want to use mysql rather than sqlite, run the “rails” command with “-d mysql” parameter. After creating your rails app, change your current working directory to your newly created rails app.


$ rails restauth
$ cd restauth
Installation


$ cd vendor/plugins
$ git clone git://github.com/technoweenie/restful-authentication.git restful_authentication
$ cd ../..
$ ./script/generate authenticated user sessions --include-activation --stateful --rspec
Install “acts_as_state_machine” plugin.


$ ./script/plugin install http://elitists.textdriven.com/svn/plugins/acts_as_state_machine/trunk 
Running the commands above will add some files to our rails app and also add piece of code to existing files.

Routes created: (config/routes.rb)


map.logout '/logout', :controller => 'sessions', :action => 'destroy'
map.login '/login', :controller => 'sessions', :action => 'new'
map.register '/register', :controller => 'users', :action => 'create'
map.signup '/signup', :controller => 'users', :action => 'new'
map.resources :users
map.resource :session
Models generated:


app/models/user_mailer.rb
app/models/user_observer.rb
app/models/user.rb
Controllers generated:


app/controllers/sessions_controller.rb
app/controllers/users_controller.rb
Views generated:


app/views/sessions/*
app/views/user_mailer/*
app/views/users/*
Migration generated: (xxxxxxxxxxxxxx depends on the time you create migration)


db/migrate/XXXXXXXXXXXXXX_create_users.rb
rspec tests and stories generated:


spec/
stories/
So far so good. If you still haven’t run into any errors it’s time to run migration:


$ rake db:migrate
Add user activation routes to “config/routes.rb” file. (Because we used “—include-activation” parameter.)


map.activate '/activate/:activation_code', 
             :controller => 'users', 
             :action => 'activate', 
             :activation_code => nil
We used “—stateful”, so we need to add an observer to enviroments and resource for users to routes.

Add user observer to “config/environment.rb” file (inside “Rails::Initializer.run do |config|” block)


config.active_record.observers = :user_observer 
p. Add users resource to “config/routes.rb” file.

	
map.resources :users, :member => { :suspend   => :put,
                                   :unsuspend => :put,
                                   :purge     => :delete }
Configuration

As you may remember, we had used “—include-activation” parameter while we were installing the plugin, so we need to make a few quick and easy changes.

Now, open the file “app/models/user_mailer.rb” and find and replace “yoursite” with your website url, “adminemail” with your email address, “[yoursite]” with your website’s title/name.


@body[:url]  = "http://YOURSITE/activate/#{user.activation_code}"
@body[:url]  = "http://YOURSITE/"
@from        = "ADMINEMAIL"
subject     = "[YOURSITE] "
If you don’t want to use randomly generated site key, you could change the value of “REST_AUTH_SITE_KEY” located in “config/initializers/site_keys.rb” file to something you want.


REST_AUTH_SITE_KEY = "Life, Don't talk to me about life!"
More Customization

If you want to change the contents of the activation email or signup notification mail sent to users, feel free to alter the following templates to meet your needs.


app/views/user_mailer/activation.erb
app/views/user_mailer/signup_notification.erb
Default installation of the plugin doesn’t enable the “Remember Me” option. However, you don’t need to worry about it, it’s so very easy to enable it. Just edit the file “app/views/sessions/new.html.erb” and uncomment the following lines.


<p><%= label_tag 'remember_me', 'Remember me' %>
<%= check_box_tag 'remember_me', '1', @remember_me %></p>
Show Time!

Now, fasten your seatbelt and prepare to take off!


$ ./script/server
Signup: Make sure your mail server is running and enter a valid email address of yours. If you don’t
have mail server installed on your computer you may want to glance at this tutorail which explains how to use gmail to send emails with your rails app.

 
hppt://localhost:3000/signup
Login

 
htpp://localhost:3000/login
That’s all. If you have any questions about this tutorial don’t hesitate to ask. Thanks for reading.

Resources:

restful-authentication at github
Restful Authentication with all the bells and whistles
