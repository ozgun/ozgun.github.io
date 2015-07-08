---
title: Upgrading your rails app from 1.2.x to 2.1.x
tags: rails
layout: post
---

These are the steps I followed while upgrading 2 of my rails applications. One of them was developed with Rails 1.2.2 and the other was 1.2.3. They are not complicated apps, so my job here was easy. If you have a sophisticated application you may need to work a little bit harder.

First of all, backup your rails app, in case something bad happens!


{% highlight bash %}
$ cp -r myapp myapp.backup
{% endhighlight %}

Backup your database.


{% highlight bash %}
$ mysqldump -u username -ppassword db_development > db_development.sql
$ mysqldump -u username -ppassword db_production > db_production.sql
{% endhighlight %}

Dive into your old applicaiton and remove all session files.


{% highlight bash %}
$ cd myapp
$ cd tmp/sessions
$ find . -name "*_sess*" -exec rm '{}' \;
{% endhighlight %}

Update `RAILS_GEM_VERSION` in your config/environment.rb file.


{% highlight bash %}
RAILS_GEM_VERSION = '2.1.2' unless defined? RAILS_GEM_VERSION
{% endhighlight %}

Update your rails app.


{% highlight bash %}
$ cd myapp
$ rake rails:update
{% endhighlight %}

Install necessary plugins. With Rails 2.x some integrated features turned to plugins such as acts_as tree, acts_as_list..


{% highlight bash %}
$ cd myapp
$ ./script/plugin install acts_as_tree
$ ./script/plugin install acts_as_list
{% endhighlight %}

Add secret key to your config/environment.rb file.


{% highlight bash %}
config.action_controller.session = { 
    :session_key => "_myapp_session", 
    :secret => "your secret here .. don't tell anyone" 
}
{% endhighlight %}

Install `classic_pagination` plugin if you used pagination in your application. The official source for this plugin should be `svn://errtheblog.com/svn/plugins/classic_pagination`. However, this url is unreachable. So we are going to try another workaround to install it.


{% highlight bash %}
$ cd myapp
$ cd vendor/plugins
$ git clone git://github.com/masterkain/classic_pagination.git
{% endhighlight %}

Find and replace all `find_all` with `find(:all)`. To find out:


{% highlight bash %}
$ cd myapp
$ grep find_all * -r
{% endhighlight %}

If you used `start_form_tag` and `end_form_tag` in you app, you should add 2 methods to application helper. If you don’t want to add these 2 methods, then you should convert your forms to code block. Check out this [google group discussion](http://groups.google.com/group/rubyonrails-talk/browse_thread/thread/a38549a4dc4d3feb?pli=1) for more info.


{% highlight bash %}
$ cd myapp
$ vim app/helpers/application.helper
{% endhighlight %}

{% highlight ruby %}
def start_form_tag(*args)
  form_tag(*args)
end
def end_form_tag
  '</form>'
end
{% endhighlight %}

I’ve realized that I didn’t use `RAILS_ROOT` in my application while setting file or directory paths. Rails 1.2.x version didn’t complain about that, however, Rails 2.x complains about not finding directory paths. I solved this issue by using `RAILS_ROOT` while creating directory or file paths. If you have file uploads in your application, I suggest you look over your codes.

Show time! Start your application server and test with your browser. I know you run Firefox


{% highlight bash %}
$ ./script/server
{% endhighlight %}

This is how I managed to work my old rails app work in Rails 2.1.2. They are not very complicated apps, so it was easy to switch to Rails 2.1.2. If you want to add something to my list please be my guest. Thanks!

### Resources:

[Deprecation](http://www.rubyonrails.org/deprecation)
