---
title: Social Icons Rails Plugin
tags: rails
layout: post
---

I’ve created a simple Rails plugin called Social Icons to put social media/network icons to your website easily.

### Installation:

{% highlight bash %}
$ ./script/plugin install git://github.com/ozgun/social_icons.git
{% endhighlight %}

The command above will copy a simple css(social_icons.css) file to “public/stylesheets” folder and “social_icons” folder to “public/images” folder.

Although it is not necessary, you can load css file like above: (i.e. application.html.erb)

{% highlight text %}
<%= stylesheet_link_tag 'social_icons' %>
{% endhighlight %}

### Usage:

You can call “print_social_icons” method in your views or partials.

{% highlight text %}
<%= print_social_icons %>
{% endhighlight %}

### Icons supported so far:

* Facebook like button
* Facebook
* Twitter
* Delicious
* Reddit
* Digg

### Images (Icon pack)

“social.me” icons from “Quaqe9” are used.

### Showtime

![Plugin in action](/files/social-icons-screenshot.png)

### Links

* [social_icons plugin](http://github.com/ozgun/social_icons)
* [Social.me icon pack](http://www.iconspedia.com/pack/social-me-1467/)
