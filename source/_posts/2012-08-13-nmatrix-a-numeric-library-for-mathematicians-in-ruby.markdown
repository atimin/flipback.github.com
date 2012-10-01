---
layout: post
title: "NMatrix -a numeric library for mathematicians in Ruby"
date: 2012-08-13 08:37
comments: true
categories: [Ruby, math, English]
---

A few words about very interesting project - [NMatrix](https://github.com/SciRuby/nmatrix). At the moment Python is the best chose of dynamic languages to work with math. It has many very stable and quality libraries such as [NumPy](http://numpy.scipy.org/), [SciPy](http://www.scipy.org/), [matlib](http://matplotlib.sourceforge.net/) and etc. But all may change... 

<!-- more -->

The [SciRuby](http://sciruby.com/) a project has goal to give powerful math instruments for Ruby programmers. It consists of several libraries for calculation and visualisation. The NMatrix is core library of the SciRuby for linear algebra written in C and C++. It has architecture that likes to [NArray](http://narray.rubyforge.org/) and aims to maximum performance. Currently the library have status pre-alpha and is reborning to more clear code with C++ templates. But a new release will come very soon (this month). 

I'm contributing the project and working around slicing operation. It's very interesting for me and I feel I do something important. Because NMatrix has three types(__dense__, __list__, __yale__) to storage elements I need to implement three different algorithms (currently I have done a __dense__ type). I hope it maybe interesting for readers and I want to write tree articles and try to describe structures of data and algorithms. 

But while, you can play (for install [see](https://github.com/SciRuby/nmatrix/wiki/NMatrix-Installation)) :


{% codeblock lang:ruby %}

require 'nmatrix'

q = NMatrix.new([3,6], [1,2,3])
q.pretty_print

{% endcodeblock %}

For more information see [wiki](https://github.com/SciRuby/nmatrix/wiki/NMatrix). 
