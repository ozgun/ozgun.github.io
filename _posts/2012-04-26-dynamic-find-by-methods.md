---
title: Dynamic "find_by" methods (Rails 2.x vs Rails 3.x)
tags: ruby rails
layout: post
---

**Rails 2.x**

{% highlight ruby %}
Right.find_by_site_id_and_user_id(Site.find(1), User.new)
{% endhighlight %}

Generated query:

{% highlight sql %}
SELECT * FROM `rights` 
WHERE (`rights`.`site_id` = 1 AND `rights`.`user_id` = NULL) 
LIMIT 1
{% endhighlight %}

**Rails 3.x**

{% highlight ruby %}
Right.find_by_site_id_and_user_id(Site.find(1), User.new)  
{% endhighlight %}

Generated query:

{% highlight sql %}
SELECT `rights`.* FROM `rights` 
WHERE `rights`.`site_id` = 1 AND `rights`.`user_id` IS NULL 
LIMIT 1
{% endhighlight %}
