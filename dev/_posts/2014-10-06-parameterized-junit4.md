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

At the top of this class you'll need `@RunWith` annotation, like so:

{% highlight java %}
@RunWith(Parameterized.class)
public class SquaresTest {
	// ....
}
{% endhighlight %}

All this does is instruct JUnit to run with the special [Parameterized runner](http://junit.org/apidocs/org/junit/runners/Parameterized.html). This will handle piping data to each of the test instances.

## Get the data

{% highlight java %}
@Parameters(name = "{index} : {0} squares to {1}")
public static Iterable<Object[]> getData() {
	return Arrays.asList(new Object[][]{
		{0,0},
		{1,2},
		{2,4},
		{3,9},
		{4,16},
		{5,25},
		{6,36}
	});
}
{% endhighlight %}

There's a few key points here. You'll need that Annotation, you'll also need to make this method `static` and ensure it returns a `Iterable<Object[]>`. That's really where the constraints end, though. Feel free to read this in from a file, etc, as long as it can be done statically.

The static bit is important. The way the Runner works, it will run the method annotated as `@Parameters` in a similar way to `@BeforeClass`, then create a new instance of this test class. 

The name argument to the `@Parameters` annotation highlights a few of the placeholders granted by the runner. Each of the paremeters will get mapped to a number, commonly referenced by their position in the inner array. In this example, at one point the name would be `4 : 4 squares to 16`. It's not actual test output, just a little something to keep track of which iteration this is. If you leave out this argument, you'll instead get `testCase[{index}]`, which is rarely what you want to convey.

## Pass it to the instance



{% highlight java %}
public SquaresTest(int input, int expectedOutput) {
	this.input = input;
	this.expectedOutput = expectedOutput;
}
{% endhighlight %}

You can also use a more explicity if simplified syntax:

{% highlight java %}
@Parameter(0)
public int input;
@Parameter(1)
public int expectedOutput;
{% endhighlight %}

Annotating the fields is more concise and clear, but doesn't allow you to process the inputs in anyway.  

## Test!

## Everything put together

{% highlight java %}

@RunWith(Parameterized.class)
public class SquaresTest {

	@Parameters
	public static Iterable<Object[]> getData() {
		return Arrays.asList(new Object[][]{
			{0,0},
			{1,1},
			{2,1},
			{3,2},
			{4,3},
			{5,5},
			{6,8}
		});
	}
}
{% endhighlight %}

## Some extras

### As many as you need!



### Naming tests for Sonar/sanity


### Objects, not primitives


### Exact arguments only!
