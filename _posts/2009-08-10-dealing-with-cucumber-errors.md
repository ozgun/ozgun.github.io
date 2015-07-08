---
title: Dealing with Cucumber errors
tags: ruby testing
layout: post
---

To fix the following error:

{% highlight text %}
undefined method `fork=' for #<Cucumber::Rake::Task:0xb770bab8>
{% endhighlight %}

You need to upgrade cucumber, rspec and rspec-rails.


{% highlight bash %}
$ gem install cucumber
$ gem install rspec
$ gem install rspec-rails
{% endhighlight %}

If you run into the following error:


{% highlight text %}
cucumber database is not configured (ActiveRecord::AdapterNotSpecified)
{% endhighlight %}

Then you should run the cucumber script to update your database.yml file:


{% highlight bash %}
$ script/generate cucumber
{% endhighlight %}

If are getting the following error after your run “cucumber features”


{% highlight text %}
no such file to load -- spec/test/unit
{% endhighlight %}

Then you may need to install or upgrade “hoe” gem, U’ not sure though, but it worked for me.


{% highlight bash %}
$ gem install hoe
{% endhighlight %}

Can’t you install “factory girl” with the following command?


{% highlight bash %}
$ gem install thoughtbot-factory_girl
{% endhighlight %}

Why not adding Github’s gem archive to your gem source list?


{% highlight bash %}
$ gem sources -a http://gems.github.com
$ gem install thoughtbot-factory_girl
{% endhighlight %}

You upgraded to cucumber 0.3.103 and webrat 0.5.3, you run your features and get the following error:


{% highlight text %}
undefined method `use_transactional_fixtures' for Cucumber::Rails:Module (NoMethodError)
./features/support/env.rb:32
{% endhighlight %}

It is easy to fix. Replace


{% highlight ruby %}
Cucumber::Rails.use_transactional_fixtures
{% endhighlight %}

with


{% highlight ruby %}
Cucumber::Rails::World.use_transactional_fixtures = true
{% endhighlight %}

You fixed the error above, but this time you see the following error:


{% highlight text %}
undefined method `bypass_rescue' for Cucumber::Rails:Module (NoMethodError)
./features/support/env.rb:37
{% endhighlight %}

Replace


{% highlight ruby %}
Cucumber::Rails.bypass_rescue
{% endhighlight %}

with


{% highlight ruby %}
ActionController::Base.allow_rescue = false
{% endhighlight %}

By the way, why don’t you just create a new features/support/env.rb by running the following command?


{% highlight bash %}
$ ./script/generate cucumber
{% endhighlight %}

So, you upgraded to cucumber 0.3.103 and you tried to run a feature with “—tags” or “-t” parameter like the following:


{% highlight bash %}
$ cucumber -r features/ -t focus features/feedback.feature
{% endhighlight %}

And you get the following error:


{% highlight text %}
0 scenarios
0 steps
0m0.000s
You have a nil object when you didn't expect it!
You might have expected an instance of ActiveRecord::Base.
The error occurred while evaluating nil.[] (NoMethodError)
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/formatter/console.rb:132:in `print_tag_limit_warnings'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/formatter/console.rb:130:in `each'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/formatter/console.rb:130:in `print_tag_limit_warnings'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/formatter/pretty.rb:232:in `print_summary'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/formatter/pretty.rb:27:in `after_features'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/ast/tree_walker.rb:170:in `__send__'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/ast/tree_walker.rb:170:in `send_to_all'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/ast/tree_walker.rb:168:in `each'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/ast/tree_walker.rb:168:in `send_to_all'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/ast/tree_walker.rb:164:in `broadcast'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/ast/tree_walker.rb:14:in `visit_features'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/cli/main.rb:55:in `execute!'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/../lib/cucumber/cli/main.rb:24:in `execute'
/usr/lib/ruby/gems/1.8/gems/cucumber-0.3.103/bin/cucumber:9
/usr/bin/cucumber:19:in `load'
/usr/bin/cucumber:19
{% endhighlight %}

It is obvious that none of your scenarios run, why? Because as “aslakhellesoy” said: “you have to use -t @focus since 0.3.102”. But don’t worry, he also added that he was going to hack the code to display you a friendly message if you try to use tags without “@” symbol. So, to get rid of the error above, run your tagged feature like this:


{% highlight bash %}
$ cucumber -r features/ -t @focus features/feedback.feature
{% endhighlight %}

[aslakhellesoy](http://github.com/aslakhellesoy) kept his promise and updated [Cucumber](http://wiki.github.com/aslakhellesoy/cucumber) to display a friendly message in case you use ‘-t’ parameter withouth ‘@’ symbol. Related commit can be found [here](http://github.com/aslakhellesoy/cucumber/commit/50efb6789a724bb7991614950309e27ff4a0af38)

### Resources:

* [Cucumber on Github](http://wiki.github.com/aslakhellesoy/cucumber)
* [Cucumber](http://cukes.info/)
