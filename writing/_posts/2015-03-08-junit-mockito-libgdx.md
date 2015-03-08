---
layout: post
title: Unit testing in LibGDX with Mockito and JUnit
---

It took me far too long to figure out how to run tests for LibGDX code.

# The Goal

After really bashing my head against it for many months, I've started to understand test-driven development. Naturally, I like applying new engineering concepts I learn to my games. However, it's not necessarily trivial, so here's what I do.

I've grown pretty comfortable with JUnit and Mockito, so that's what I'll be talking about.

## A brief aside on what (not) to test

Testing can be very useful for getting yourself an automated suite to make changes with confidence and catch regressions early. If you'd like to learn more about it in general, check out this article. [attach article, dummy]

As a general rule, it's useful for verifying logic and interactions, not necessarily for checking visuals or anything else heavy.

# Build.gradle 

The first step we'll want to accomplish is to add dependencies to the project. We'll only be adding them to the `core` sub-project.

{% gist icbat/6e0ba814c33d597a850e %}

Those lines add in the junit 4.11 and the latest 1.x version of Mockito. If your IDE doesn't immediately update, try using either `gradle idea` or `gradle eclipse` to regenerate your dependencies.

# Writing a simple first test

Here's a simple test class. 

{% gist icbat/dcf67d5aecc3760a4d8d %}

At a high level, this will run the method annotated as `@Before`, then run a method annotated as `@Test`. In this case, I'm using it to make sure that each `@Test` method gets a fresh list of Workshops and a fresh TurnTaker.

Other useful annotations are `@BeforeClass` that runs once before everything else, `@After` that runs after each `@Test` and `@AfterClass` that runs after everything else. The last two are usually used if you need to dispose of something or remove any test data you may have spawned.

The mocking is the magical part. In the actual test method, I'm adding two Workshops to the list as part of my Arrange section. But instead of giving it a real Workshop, I use `Mockito.mock(ThingINeed.class)` (in fact, Workshop is an Interface). Mockito.mock() will hand back a mocked Workshop; if I call anything on the fake workshop, it will silently succeed and return `null`.

Then, I use another Mockito call, `verify`. Verify checks wether or not a method was called on a mock. By default, verify will pass the test if the method was called once and only once, but you can pass this method other conditions if you need.

The point of this test is that I don't need to know or care what a workshop actually does; for all I know it's actually quite dangerous! But I know my TurnTaker needs to get every workshop to take its turn, so I verify that here and let another test worry about the Workshop's behavior. If this test were to start failing, I'd know I had a problem in my TurnTaker and not a cascading failure from a Workshop.

You can read more about JUnit [here](http://junit.org/) and more about Mockito [here](http://mockito.org).

## Mocking out Gdx.app's application

There's a few cases when this simple approach isn't quite enough. The most common one I've ran in to is trying to test a method that has `Gdx.app.log` statements in it; these throw a NullPointerException, as Gdx.app was never set up. That usually happens as a part of your Launcher class, which our tests skip. For most things, we still want to skip that, so here's a little work-around:

{% gist icbat/c3003d77c83e63792f64 %}

Simple enough, right? We're giving a fake Application to Gdx so it can call log like it wants. You can extend this to any of the Gdx static pieces; just find the one you're using and mock it out!

If you really do need a full Launcher to run first, might I suggest the [Headless backend](https://github.com/libgdx/libgdx/tree/master/backends/gdx-backend-headless)? I've never needed to use it, but this sort of thing sounds perfect for more involved testing.


# What next?

Go, and test all the things. Have some confidence that you're not  breaking things and that you'll never re-release an old bug again.

A quick sanity check before distributing your game can now be to run tests right before you build, using `gradle test`. This doesn't give much feedback by default, so it might be worth purposefully (and temporarily) breaking a test to see it fail.

If you're using something like Github to host your code, you can hook up [TravisCI](https://travis-ci.org/) to your repo to have all your tests run on every pull request and build of your main branch.
