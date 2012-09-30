---
layout: post
title: "NMatrix 0.2.0 and slicing"
date: 2012-09-30 19:43
comments: true
categories: [ruby, math]
---

A few days ago the [new version][1] of NMatrix has been released. Among a lot new features it has slicing operation. The slicing has been implemented by two ways. First of they it's a slicing by reference. It is provided by **#[]** method:

{% codeblock lang:ruby %}
require "nmatrix"
require "pp"

# Create dense matrix with size [3,3] and fill it.
m = NMatrix.new(:dense, [3,3], (0..8).to_a) 
r = m[0..1, 1..2] #=> [1,2] [4,5]
r.is_ref?         #=> true

r[0,0] = 999    

pp r              #=> [999,2] [4,5]
pp m              #=> [0,999,2] [3,4,5] [6,7,8]
{% endcodeblock %}

As you see both matrices has been changed when we assign a new value to element of _r_ matrix. The **#[]** method doesn't allocate memory,  but creates new matrix with reference to base matrix. You can built chain of same slicing and all of new matrix will be references for one base. 

<!-- MORE --> 

**Note:** The NMatrix uses ATLAS for multiplication, which surely cannot work with references. And so when you try to multiply references they will be copped to temporary matrices before multiplication. Remember it using a big data. Also it is for several operations such as casting. 

Second method of slicing by copying is **#slice**. This method allocate memory for slice and copies all elements from base matrix. Example:

{% codeblock lang:ruby %}
require "nmatrix"
require "pp"

# Create dense matrix with size [3,3] and fill it.
m = NMatrix.new(:dense, [3,3], (0..8).to_a)

r = m.slice(0..1, 1..2) #=> [1,2] [4,5]
r.is_ref?         #=> false

r[0,0] = 999    

pp r              #=> [999,2] [4,5]
pp m              #=> [0,1,2] [3,4,5] [6,7,8]
{% endcodeblock %}

This time the _r_ matrix is independent object. All changes of it doesn't concern base matrix and you can use it as usual matrix.

I ought to remind that slicing operations is pretty raw. Will be careful to use it. If you will have a bug, feel free to report about it [here][2].

Thanks, Aleksey.

[1]: http://sciruby.com/blog/2012/09/24/second-nmatrix-alpha-released/
[2]: https://github.com/SciRuby/nmatrix/issues?direction=desc&milestone=2&sort=created&state=open
