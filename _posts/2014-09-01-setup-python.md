---
layout: post
title: Setup Python
date: 2014-09-01 10:20:10
published: True
categories: ['python']
tags: []
---

If you are using <code>Pythin < 3.3</code>, install [virtualenv](http://virtualenv.readthedocs.org/en/latest/) and [virtualenvwrapper](http://virtualenvwrapper.readthedocs.org/en/latest/).
{% highlight bash %}
# virtualenvwrapper lists virtualenv as a dependency, so it's automatically installed
$ sudo pip install virtualenvwrapper
{% endhighlight %}

Virtualenv lets you create an isolated environment for each project. Especially useful when you use different versions of packages in different projects.
Virtualenv wrapper provides a few nice scripts to make things a little easier.

#### IPython
IPython is an alternative to the standard <code>Python REPL</code> that allows for auto complete, quick access to docs, and a slew of other features that really should have been in the standard REPL.

Simply <code>pip install ipython</code> and type ipython (you can even create an alias in your shell <code>alias py=ipython</code>)

#### Testing
If you are using <code>Pythin < 3.3</code> install <code>mock</code> for testing. Also install <code>nose</code>.
{% highlight bash %}
$ pip install mock nose
{% endhighlight %}
Read more: https://docs.python.org/3/library/unittest.mock.html, https://nose.readthedocs.org/en/latest/

#### Debugging
<code>iPDB</code> is an excellent tool to figure out some weird bugs. Install it <code>pip install ipdb</code> and add the line <code>import ipdb; ipdb.set_trace()</code> in your code, and you get a nice interactive prompt in your running program.
{% highlight bash %}
? for help
? l for help for command l
l for "some more context"
s for "step into"
n for "step over"
c for "continue"
{% endhighlight %}

Use <code>pprint</code> for pretty printer.

[That's all folks](https://www.youtube.com/watch?v=gBzJGckMYO4)
