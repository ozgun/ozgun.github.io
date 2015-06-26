---
title: Working with git submodules
tags: linux git
layout: post
---

### Cloning the main project

{% highlight bash %}
$ cd ~
$ git clone git@github.com:ozgun/main_project.git
{% endhighlight %}

### Initializing the submodule (sub project):

Create ~/main_project/.gitmodules file and add the followings lines to it:

{% highlight bash %}
[submodule "sub_project"]
        path = sub_project
        url = git@github.com:ozgun/sub_project.git
{% endhighlight %}

Run the following commands to initialize the submodule:

{% highlight bash %}
$ cd ~/main_project
$ git submodule init
$ git submodule update
{% endhighlight %}

“git submodule update” command does not checkout to “master” branch. So we need to create master branch and then switch to it as below:

{% highlight bash %}
$ cd ~/main_project/sub_project
$ git checkout master
{% endhighlight %}

### When something changes in sub project

Since sub_project is a submodule in main_project, when something changed in submodule, main projects also needs to be notified(synced) about this change.

#### Adding a new file to submodule and pushing to sub_project’s repository

{% highlight bash %}
$ cd ~/main_project/sub_project
$ git checkout master # make sure you are working on master branch
$ touch README
$ git add README
$ git commit -am "Added README to sub_project - submodule update"
$ git push origin master
{% endhighlight %}

#### Pushing the main_project to remote repository

After we updated sub_project, we need to update main_project too.

{% highlight bash %}
$ cd ~/main_project
$ git checkout master
$ git status
{% endhighlight %}

After you run “git status” you should see something like this:

{% highlight text %}
# On branch master
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   sub_project
#
no changes added to commit (use "git add" and/or "git commit -a")
{% endhighlight %}

Your main projects understands that something has changed inside the sub_project, so it is considered as “modified”. This means that we need to commit and then push to remote git repository as below:

{% highlight bash %}
$ cd ~/main_project
$ git add sub_project # Do not append "/" !
$ git commit -am "Submodule sub_project updated"
$ git push origin master
{% endhighlight %}

### Resources:

* [git Submodules Explained](http://longair.net/blog/2010/06/02/git-submodules-explained/)
* [git-submodule-head](http://stackoverflow.com/questions/2155887/git-submodule-head)
* [GitSubmoduleTutorial](https://git.wiki.kernel.org/index.php/GitSubmoduleTutorial)
* [Pragmatic Guide to git](http://pragprog.com/book/pg_git/pragmatic-guide-to-git)
