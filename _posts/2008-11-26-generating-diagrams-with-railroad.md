---
title: Generating diagrams with Railroad for Rails apps
tags: rails
layout: post
---

[RailRoad](http://railroad.rubyforge.org/) is a class diagrams generator for Ruby on Rails applications.

### Installation

{% highlight bash %}
$ sudo apt-get install graphviz
$ sudo gem install railroad
{% endhighlight %}

### Generating Diagrams

Generating models diagram in .dot file format and converting it to png:


{% highlight bash %}
$ railroad -o models.dot -M
$ dot -Tpng models.dot > models.png
{% endhighlight %}

Models diagram in svg format.


{% highlight bash %}
$ railroad -M | dot -Tsvg > models.svg
{% endhighlight %}

Models diagram with all classes showing inheritance relations


{% highlight bash %}
$ railroad -a -i -o full_models.dot -M
{% endhighlight %}

Controllers diagram in png format.


{% highlight bash %}
$ railroad -C | neato -Tpng > controllers.png
{% endhighlight %}

### Resources

* [Railroad gem](http://railroad.rubyforge.org/)
* [Graphviz](http://www.graphviz.org/)
* [Ruby on rails diagram generator – RailRoad](http://www.blainekendall.com/index.php/archives/2007/04/11/ruby-on-rails-diagram-generator-railroad/)
