---
title: Mocking methods ending with equal sing (=) in (RR) Double Ruby
tags: ruby testing
layout: post
---

So, you are using Double Ruby as mocking framework instead of Rspec’s default mocking framewok, and you want to mock a method which ends with the equal sign(=). Normally, if your method ends with other special characters such as “!” or “?”, you can safely mock those methods as below:


{% highlight ruby %}
mock(@user).delete_account! { true }
mock(@user).admin? { true }
{% endhighlight %}

However, this doesn’t work if your method ends with the “=” sign. Here comes the solution:

{% highlight ruby %}
mock(@user).__send__('name=').with('Paranoid Android') { }
{% endhighlight %}

Now, in your controller, you can expect the following:

{% highlight ruby %}
@user.name = 'Paranoid Android'
{% endhighlight %}

But, why did I use __send__ instead of the simple send ? [Take a look at this](http://stackoverflow.com/questions/4658269/ruby-send-vs-send)

### Resources

* [Double Ruby - RR](https://github.com/btakita/rr)
* [rspec](https://github.com/dchelimsky/rspec)

