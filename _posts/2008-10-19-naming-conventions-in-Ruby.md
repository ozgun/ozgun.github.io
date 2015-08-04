---
title: Naming Conventions in Ruby
tags: ruby 
layout: post
---

It’s good to follow Ruby convetions while writing Ruby programs. In this short tutorial I will a cover a few rules about naming variables, methods, classes and modules in Ruby.

### Global Variables

Golabal variables in Ruby start with a dollar sign: `$`

{% highlight ruby %}
$i_am_a_global_variable
{% endhighlight %}


### Class Variables

Class variables are prefixed with two “at” signs: @@

{% highlight ruby %}
@@i_am_a_class_variable
{% endhighlight %}

### Instance Variables

Instance variable starts with an “at” sign: @

{% highlight ruby %}
@i_am_an_instance_variable
{% endhighlight %}

### Local Varaibles

Local variables are in lowercase letters.

{% highlight ruby %}
i_am_a_local_variable
{% endhighlight %}

### Constants

Constants start with an uppercase letter.

{% highlight ruby %}
PI
FeetPerMile
{% endhighlight %}

### Class Names

Class names begin with an uppercase letter.

{% highlight ruby %}
class String 
  ...
end
class ReadOnlyAssociation
  ...
end
{% endhighlight %}

###  Module Names:

Module names start with an uppercase letter like class names.

{% highlight ruby %}
module Kernel 
  ...
end
module ActiveRecord
  ...
end
{% endhighlight %}

### Method names and parameters 

In Ruby, method names and parameters should start with smallcase letter. If
method name or parameters has includes than one words, you should separate them
using underscores.


{% highlight ruby %}
def my_method(param1, param2)
  ...
end
{% endhighlight %}

## Resources

[Programming Ruby – The Pragmatic Programmer’s Guide](http://www.rubycentral.com/book/)
