---
layout: default
title: Parameterized JUnit 4
---

A friend of mine's been working to introduce [Cucumber](http://cukes.info/) into their automation stack. As we've worked, I got my first taste of parameterizing tests. While Cucumber certainly does it better and cleaner, here's what I've cobbled together in an attempt to try it with JUnit (4).

At a high level, you need a few things:

1. A way to get the parameters. (Static method)
2. A way to know which one you're running. (Constructor/Annotations)
3. One or more tests.

But here's a breakdown on how to actually implement this strategy.

## Run with
{% highlight java %}

@RunWith(Parameterized.class)
public class ParamTest_Simple {
	// ....
}
{% endhighlight %}

## Get the data

## Pass it to the instance

## Test!

## Some suggestions

### Naming tests for Sonar/sanity


### Objects, not primitives

{% highlight java %}

@RunWith(Parameterized.class)
public class ParamTest_Simple {
	
}
{% endhighlight %}