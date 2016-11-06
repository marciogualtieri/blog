---
layout: post
title:  "Java Subtyping"
date:   2016-11-05 19:25:59 +0000
categories: scala java fp
---
If you (like me) came from a Java background, you might be familiar with the
covariance rules in Java.

Let's try the following code snippet:

{% highlight java linenos %}
List<NonEmpty> nonEmptyThingies = new ArrayList<>();
nonEmptyThingies.add(new NonEmpty());
nonEmptyThingies.add(new NonEmpty());

// I'm sorry, Dave. I'm afraid you can't do that.
List<Thingy> thingies = nonEmptyThingies;
{% endhighlight %}

Where 'Thingy' is an interface which both 'Empty' and 'NonEmpty' implement.

If you try that, your code won't compile:

{% highlight text %}
incompatible types: java.util.List<blog.covariance.NonEmpty> cannot be converted
 to java.util.List<blog.covariance.Thingy>
{% endhighlight %}

You can get around this quibble using unbounded wildcards:

{% highlight java linenos %}
List<NonEmpty> nonEmptyThingies = new ArrayList<>();
nonEmptyThingies.add(new NonEmpty());
nonEmptyThingies.add(new NonEmpty());

// Alright, Dave. Since you said pretty please...
List<? extends Thingy> thingies = nonEmptyThingies;
{% endhighlight %}

Let's try something naughty:

{% highlight java linenos %}
List<NonEmpty> nonEmptyThingies = new ArrayList<>();
nonEmptyThingies.add(new NonEmpty());
nonEmptyThingies.add(new NonEmpty());

List<? extends Thingy> thingies = nonEmptyThingies;
thingies.add(new Empty());
{% endhighlight %}

Once again, your code won't compile:

{% highlight text %}
incompatible types: java.util.List<blog.covariance.NonEmpty> cannot be converted
 to java.util.List<blog.covariance.Thingy>
{% endhighlight %}

Well, you're trying to add an Empty thingy to a list of NonEmpty thingies, buddy...

The whole point of these code snippets is to show you that Java 'List' is
invariant, that is, 'List<NonEmpty>' is not a subtype of 'List<Thingy>' like
'NonEmpty' is a subtype of 'Thingy'.

Wanna play the same game with Java 'Array'?

{% highlight java linenos %}
NonEmpty[] nonEmptyThingies = new NonEmpty[3];
nonEmptyThingies[0] = new NonEmpty();
nonEmptyThingies[1] = new NonEmpty();

// Whut? It compiles...
Thingy[] thingies = nonEmptyThingies;
{% endhighlight %}

Let's try that naughty thing we tried to do with 'List':

{% highlight java linenos %}
NonEmpty[] nonEmptyThingies = new NonEmpty[3];
nonEmptyThingies[0] = new NonEmpty();
nonEmptyThingies[1] = new NonEmpty();

// Wut? Wut? Still compiles...
Thingy[] thingies = nonEmptyThingies;
thingies[2] = new Empty();
{% endhighlight %}

If you try to execute this snippet though:

{% highlight text %}
java.lang.ArrayStoreException: blog.covariance.Empty
{% endhighlight %}

So Java 'Array' is covariant in the types of the objects they hold.
'Array<NonEmpty>' is a subtype of 'Array<Thingy>'and if you try naughty things,
like adding an Empty to a list of NonEmpty, you will get a nasty exception.

Java 'Array' being covariant is legacy design from when Java didn't support
generic types, which is a much safer and elegant way to accomplish the same
things people were trying to achieve before Java 5, with covariance, when
generic types weren't available.

My initial plan was to get into covariance in Scala by starting from a familiar
place, Java, but since this post is already quite long, I'll leave it to next
time. Covariance in Scala is quite a bit more fun, and tricky to get your head
around it, since we can subtype and pass functions as arguments.
