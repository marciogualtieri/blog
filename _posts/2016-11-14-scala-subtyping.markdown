---
layout: post
title:  "Scala Subtyping"
date:   2016-11-14 19:25:59 +0000
categories: scala fp
---

Given the following function types:

{% highlight scala linenos %}
type T1 = A1 => B1
type T2 = A2 => B2
{% endhighlight %}

And the following function subtyping:

{% highlight scala linenos %}
T1 <: T2
A1 => B1 <: A2 => B2
{% endhighlight %}

Implies:

{% highlight scala linenos %}
A1 <: A2 # Contravariant
B1 :> B2 # Covariant
{% endhighlight %}

Supposed there is a function 'f()' which originally takes a function type 'T2'
as an argument:

What are the conditions that would allow 'T1' be passed instead?

As far as 'f()' is aware, it calls the function passed as an argument with an
input of type 'A2' and stores its return value in a variable of type 'B2'.

'f()' can only call 'T1' with an argument 'A2' if 'A1' is a super-type of 'A2'

{% highlight scala linenos %}
A1 :> A2
{% endhighlight %}

Therefore contra-variant in 'A1' ('A1' is a subtype of 'A2' when 'T1' is a subtype of 'T2').

'f()' can only store a return value 'A1' in a variable of type 'B2' if 'B2' is
a super-type of 'B1'

{% highlight scala linenos %}
B1 <: B2
{% endhighlight %}

Therefore covariant in 'B1' ('B1' is a subtype of 'B2' when 'T1' is a subtype of 'T2').
