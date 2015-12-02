---
title: Using gmail to send emails with Ruby on Rails
tags: rails
layout: post
---

This tutorial will cover how to send emails using gmail with Ruby on Rails. In my opinion this is not a good idea, for example there may be some restrictions on number of mails sent in a day, or if a problem occurs on the mail server you might not be able to send emails. Another issue that bothers me is that you have to store your email account’s username and password in a plain config file. However, in some occasions (never-ending client requests) you might have to use this approach.

First of all, we need to download and install a plugin called [Action Mailer tls](http://code.openrain.com/rails/action_mailer_tls/) which was developed by Marc Chung.

{% highlight bash %}
$ cd my_rails_app 
$ ./script/plugin install http://code.openrain.com/rails/action_mailer_tls/ 
{% endhighlight %}

After installing the plugin, lets copy `smtp_gmail.rb` into `config/initializers`.

{% highlight bash %}
$ cp vendor/plugins/action_mailer_tls/sample/smtp_gmail.rb config/initializers/
{% endhighlight %}

Now we need a configuration file which email account information will be stored.

{% highlight bash %}
$ cp vendor/plugins/action_mailer_tls/sample/mailer.yml.sample config/mailer.yml
{% endhighlight %}

OK, we created our config file. Now, with your favorite text editor open that file and write your gmail username and password in it.

{% highlight bash %}
$ vim config/mailer.yml
{% endhighlight %}

{% highlight ruby %}
---
  :address: smtp.gmail.com
  :port: 587
  :user_name: email@dress.com
  :password: h@ckme
  :authentication: :plain
{% endhighlight %}

So far so good. We completed necessary configuration to add gmail support to our rails app. Now lets create our mailer.

{% highlight bash %}
$ ./script/generate mailer MyMailer
{% endhighlight %}

If we successfully created our mailer, lets proceed to next step, creating the mailer method that sends email. Lets say we want to send a thank you message right after user registration. So we can give the name “thankyou_email” to our method.

{% highlight bash %}
$ vim app/models/my_mailer.rb
{% endhighlight %}

{% highlight ruby %}
def thankyou_email
  subject "Thank you!"
  recipients newuser@exemple.com
  from email@dress.com
  sent_on Time.now
  content_type "text/html"
end
{% endhighlight %}

Now, it’s time to create the template which will be used for our “thank you” email.

{% highlight bash %}
$ vim app/views/my_mailer/thankyou_email.erb
{% endhighlight %}

Lets put a few words in that template:

{% highlight text %}
Dear user,
Thank you for registering.
-Site Admin
{% endhighlight %}

So close to the happy ending. Now put the following code in your controller in the method that
will be responsible for sending thank you emails.

{% highlight ruby %}
def send_thankyou_email
  MyMailer.deliver_thankyou_email
  redirect_to "/"
end
{% endhighlight %}

Well of course you may give another name to that method, and you may want to extend this method a little
bit. For example you might want to check if the mail is sent successfully by adding a rescue block.

That’s it, we are cleared to take off! Just don’t forget to reset your rails app because plugin
installations need restart as far as i know.

**P.S:** I used this approach with Ruby on Rails versions 2.0 and 2.1. I’m not sure if it works with older versions of rails.

### Resources:

* [A good tutorial from Daniel Fischer](http://www.danielfischer.com/2008/01/09/how-to-use-gmail-as-your-mail-server-for-rails/)
* [Original readme of Action Mailer tls plugin](http://code.openrain.com/rails/action_mailer_tls/README)
