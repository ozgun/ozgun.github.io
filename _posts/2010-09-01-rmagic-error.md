---
title: RMagick error
tags: ruby
layout: post
---

### The Problem: (occurred in Ubuntu 10.04.1)

{% highlight text %}
This installation of RMagick was configured with ImageMagick 6.5.5 but ImageMagick 6.5.7-8 is in use.
{% endhighlight %}

### The Solution:

Uninstall “rmagick” gem

{% highlight bash %}
$ sudo gem uninstall rmagick
{% endhighlight %}

Compile and install RMagick from source code

{% highlight bash %}
$ wget http://files.rubyforge.vm.bytemark.co.uk/rmagick/RMagick-2.13.1.tar.gz
$ tar xzf RMagick-2.13.1.tar.gz
$ cd RMagick-2.13.1
$ ruby setup.rb
$ sudo ruby setup.rb install
{% endhighlight %}

Test and see if it works

{% highlight bash %}
$ irb
irb(main):001:0> require "RMagick"
=> true
{% endhighlight %}

### Alternative solutions

https://bugs.launchpad.net/ubuntu/source/librmagick-ruby/bug/565461

### Conclusion

[These pretzels are making me thirsty](http://www.youtube.com/watch?v=QBrNkPyr8I8)
