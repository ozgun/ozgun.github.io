---
title: Ruby on Rails Code Quality Checklist
tags: rails
layout: post
---

[Matt Moore](http://www.matthewpaulmoore.com/) wrote a great article about how you can improve quality of your rails code called [Ruby on Rails Code Quality Checklist](http://www.matthewpaulmoore.com/articles/1276-ruby-on-rails-code-quality-checklist).

In my opinion it is a very useful article for especially Rails beginers.

Here is the checklist:

* Each controller action only calls one model method other than an initial find or new. (Make custom `.new` or `.update` methods in the model with all necessary).
* Only one or two instance variables are shared between each controller and view.
* All model and variable names are both immediately obvious (to a new developer) and as short as possible without using abbreviations.
* All custom “finds” accessed from more than one place in the code use `named_scope` instead of a custom method.
* `.find` or `.find_by_` is never called in a view or view helper.
* There is zero custom code that duplicates functionality of a built-in function in rails.
* Code has been aggressively DRYed during development.
* All functionality used in two or more models has been turned into a library/module.
* All logic duplicated between two or more apps has been turned into a gemified plugin.
* STI is not used anywhere
* Every design choice should yield the most simplistic design possible for the need of users at the current time. 
* No guesses for future functionality were designed into the application.
* Close to full test coverage exists at the highest level of the application: on and between controller actions. 
* Coverage is highest for code used by the most number of end users.
* All tests pass before code is merged into a shared repository.
* Every fixed defect on a deployed product has tests added to prevent regression.
* Every plugin installed has been code reviewed.

He also explains all the items in the checklist in a very clear way in his blog so I strongly suggest you read that article

Another good article about this topic: http://www.fortytwo.gr/blog/18/9-Essential-Rails-Tips
