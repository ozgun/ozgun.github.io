---
title: Installing Ruby on Rails (Ubuntu)
tags: rails
layout: post
---

Updating your repository.

{% highlight bash %}
$ sudo apt-get update
{% endhighlight %}

Installing Ruby, MySQL, sqlite and build-essential

{% highlight bash %}
$ sudo apt-get install ruby ri rdoc build-essential
$ sudo apt-get install mysql-server mysql-client libmysql-ruby
$ sudo apt-get install libsqlite3-0 libsqlite3-dev libsqlite-ruby
{% endhighlight %}

Installing gem package

{% highlight bash %}
$ sudo apt-get install gem
{% endhighlight %}

Installing Ruby on Rails

{% highlight bash %}
$ sudo gem install rails --include-dependencies
{% endhighlight %}

Installing a few useful Ruby gems:

{% highlight bash %}
$ sudo gem install mongrel
$ sudo gem install capistrano 
$ sudo gem install ruby-debug
$ sudo gem install image_science
$ sudo gem install rmagick
$ sudo gem install RedCloth
{% endhighlight %}

Now you may create your first rails app:

{% highlight bash %}
$ rails myapp
{% endhighlight %}

If you want to use mysql instead of sqlite in your app:

{% highlight bash %}
$ rails myapp -d mysql
{% endhighlight %}

### Resources:

* [Installing Ruby on Rails and Ruby on different systems](http://wiki.rubyonrails.org/rails/pages/HowtosInstallation)
