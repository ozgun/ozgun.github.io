---
title: Upgrading Redmine from 0.7.3 to 0.8.5
tags: ruby
layout: post
---

I’ve just upgraded Redmine from 0.7.3 to 0.8.5.Here are the notes I’ve taken while upgrading. Please don’t forget to refer to Redmine’s offical documentation before you do something bad :)

* http://www.redmine.org/wiki/redmine/RedmineUpgrade
* http://www.redmine.org/wiki/redmine/RedmineInstall#Requirements

First off all, before you start, backup your existing redmine installation. backup! backup! backup! backup! backup! backup! backup! backup! backup! backup! backup! backup!.. Both files and database.

{% highlight bash %}
$ mkdir -p manual-backups/redmine
$ cp -r redmine/ manual-backups/redmine/redmine-2009.10.01
$ cd manual-backups/redmine/
$ mysqldump redmine > redmine-2009.10.01.sql
{% endhighlight %}

Upgrade rubygems to 1.3.5 and rails to 2.3.4. I had sent a message to my hosting company(hostingrails.com) explaining that I needed a new version of rubygems and they upgraded rubygems in 10 minutes. I also realized that they had already installed “rails-2.3.4”, so this step was the easiest one for me.

{% highlight bash %}
$ gem install rails -v 2.3.4
{% endhighlight %}

Install ruby-openid for openid logins

{% highlight bash %}
$ sudo gem install ruby-openid
{% endhighlight %}

Since I had installed Redmine using svn, I’m going to use the same method to upgrade.

{% highlight bash %}
$ cd ~/redmine
$ svn update
{% endhighlight %}

Generating `session_store.rb`

{% highlight bash %}
$ rake config/initializers/session_store.rb RAILS_ENV="production"
{% endhighlight %}

Clean up

{% highlight bash %}
$ rake tmp:cache:clear RAILS_ENV="production"
$ rake tmp:sessions:clear RAILS_ENV="production"
{% endhighlight %}

Run migrations for redmine and its plugins

{% highlight bash %}
$ rake db:migrate RAILS_ENV="production"
$ rake db:migrate:upgrade_plugin_migrations RAILS_ENV="production"
$ rake db:migrate_plugins RAILS_ENV="production"
{% endhighlight %}

Restart your application server. Since mine is hosted one a fastcgi hosting, I just kill the fcgi processes.

{% highlight bash %}
$ ps -ef |grep fcgi
$ kill -9 XXXX XXXX XXXX
{% endhighlight %}

Installing backlogs plugin:

{% highlight bash %}
$ cd ~/redmine
$ cd vendor/plugins
$ git clone http://github.com/relaxdiego/redmine_backlogs
$ cd ../../
$ rake db:migrate_plugins RAILS_ENV="production"
{% endhighlight %}

Restart Redmine

{% highlight bash %}
$ ps -ef |grep fcgi
$ kill -9 XXXX XXXX XXXX
{% endhighlight %}

Enable “backlogs” in your project’s setting tab.

{% highlight bash %}
Your Project > Settings > Modules
{% endhighlight %}

Chart Data Generator: The author of the plugin says: “You may schedule a cron job to run the rake task named redmine:backlogs_plugin:generate_chart_data. I recommend you run it a few minutes after midnight to ensure that your backlogs have data everyday even when no user views the charts.”

{% highlight bash %}
$ rake redmine:backlogs_plugin:generate_chart_data RAILS_ENV="production"
{% endhighlight %}

I’d like to thank to Mark Maglana for this great “Scrum/Agile” plugin.

Following the steps above, I successfully upgraded Redmine and installed “backlogs” plugin. If you encounter any problems please refer to "Redmine’s official documentation:http://www.redmine.org/projects/redmine/wiki
