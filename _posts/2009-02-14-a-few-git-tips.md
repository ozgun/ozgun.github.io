---
title: A few git tips
tags: git
layout: post
---

### Spell checking

To enable spell checking with git-gui, Install aspell and aspell-en packages.

{% highlight bash %}
$ sudo pacman -S aspell aspell-en
{% endhighlight %}

### Colorful outputs

[Over Here](http://kozgun.net/articles/21-Lets-bring-some-colors-to--git.html)

### swap files

To ignore all “.swp” files add the following line to your “.git/info/exclude” file.

{% highlight text %}
*.swp
{% endhighlight %}

### .gitignore

My .gitignore file.

{% highlight text %}
tmp/**/*
log/*.log
db/*.sqlite3
db/*.sql
db/schema.rb
config/database.yml
config/deploy.rb
.loadpath
.project
.DS_Store
cache/views/*
test/fixtures/settings.yml
{% endhighlight %}

### Initializing a bare repo

{% highlight bash %}
$ mkdir myapp.git
$ cd myapp.git
$ git --bare init
{% endhighlight %}

### Creating a remote branch

{% highlight bash %}
$ git checkout -b mynewbranch
$ git push origin mynewbranch
{% endhighlight %}

### Branches

Playing with branches(create, switch, merge)

{% highlight bash %}
$ git branch #Display local branches
$ git checkout -b newbranchname # Create a new branch and switch to it.
$ git checkout master # Switch back to master
$ git checkout newbranchname # Switch back to newbranchname
$ git merge master #Merge master branch into newbranchname
$ git checkout newbranchname # Switch back to newbranchname
{% endhighlight %}

### Adding a new remote repository

{% highlight bash %}
$ git remote add origin ssh://user@example.com/home/user/repositories/myapp.git/
$ git remote add origin file:///home/user/repositories/myapp.git/
{% endhighlight %}

### Undo(revert) committed changes

{% highlight bash %}
$ touch file.txt
$ git add file.txt
$ git commit -a -m "Adding file.txt"
$ git log # ==> commit 9972713b5677302580959644aebd33196eefa38e 
$ git-revert 9972713b5677302580959644aebd33196eefa38e
$ git status
{% endhighlight %}

To revert the most recent commit

{% highlight bash %}
$ git revert HEAD
{% endhighlight %}

Lets say you changed some files in your working tree and you haven’t committed yet, but suddenly you decide that it was a big mistake and you want to go back to last committed state. You could undo uncommitted changes with the following command.

{% highlight bash %}
$ git reset --hard HEAD 
{% endhighlight %}

### Links:

* [Git – The Fast Version Control System](http://git-scm.com/)
* [The Git Community Book](http://book.git-scm.com/index.html)
* [Git Manual](http://www.kernel.org/pub/software/scm/git/docs/)
* [Git Wiki](http://git.or.cz/gitwiki)
* [Git cheat sheet](http://ktown.kde.org/~zrusin/git/git-cheat-sheet-medium.png)
