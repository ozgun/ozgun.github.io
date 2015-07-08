---
title: Lets bring some colors to git
tags: git
layout: post
---

Create your .gitconfig file in your home directory.


{% highlight bash %}
vim ~/.gitconfig
{% endhighlight %}

Copy and paste the following lines to ~/.gitconfig file.


{% highlight bash %}
[color]
  ui = auto
[color "branch"]
  current = yellow reverse
  local = yellow
  remote = green
[color "diff"]
  meta = yellow bold
  frag = magenta bold
  old = red bold
  new = green bold
[color "status"]
  added = yellow
  changed = green
  untracked = cyan
{% endhighlight %}

Now, run some git commands to see some colorful output:

{% highlight bash %}
$ git status
$ git branch
$ git log -p
{% endhighlight %}

Bonus: Lets add some aliases to .gitconfig.

{% highlight bash %}
[alias]
  st = status
  df = diff
  lg = log -p
{% endhighlight %}

Wanna learn more about git? Lets cheat!

{% highlight bash %}
$ sudo gem install cheat
$ cheat git
{% endhighlight %}
