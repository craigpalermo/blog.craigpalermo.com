---
layout: post
title:  "Learning to Love Testing"
date:   2015-06-05
categories: [javascript, testing, test driven development]
---

I've never been a big fan of testing. As we grow as developers, we follow one or two paths relating to the subject. We either love and embrace it as a part of our workflow, or we hate it and hope that we find someone that will do it for us. For as long as I can remember, I've been the latter. Sometimes, I even say to myself "Today's the day, you're going to learn how to set up and run tests!", but that's [the day that never comes](https://youtu.be/O4rJUtJM3aM?t=1m54s).

## What can test-driven development do for you?

Anyway, here's me doing my part to make it easier for you to get started in the world of JavaScript testing. We're going to take a look at test-driven development (TDD) as a means of building on top of assumptions that you've made about what you want the end result to actually do. Let me illustrate the difference between this and your traditional unit testing.

*Developing with unit testing* - "Finally finished writing that sick function, and it totally looks like it should work because the output looks good for the three input sets I ran through it. Ugh, I should probably write unit tests for it. Nah, I've done enough today."

Needless to say, that dude's tests are never actually written, not on that day, or any that follow.

*Test-driven development* - "Now that I've defined exactly what I want my application to do, I can just write formal test cases that give me a readable path to follow as I'm building it! Plus, the tests help me verify what's working and what's not as I go along. This is great!"

He sounds a bit more enthused about his tests, and I'd bet that his program was written eventually and ended up with far fewer bugs.

## Figuring out where to start

This was my biggest question starting out. How am I supposed to write tests? What should I do to run them? The answer to both of these questions is to find a suitable testing framework. I'm focusing on JavaScript here, but the same idea applies to whatever language you prefer.

Here are a few popular options for testing pure JavaScript that doesn't require use of the DOM. These work great when developing JS or NPM modules.

* [Jasmine](https://github.com/jasmine/jasmine)
* [Mocha](http://mochajs.org/)
* [QUnit](http://qunitjs.com/)

I'm going to focus on using [Jasmine as an NPM module](https://github.com/jasmine/jasmine-npm) to test the contents of a single JavaScript file.

First things first, we'll want to install Jasmine:

```
npm install -g jasmine
```

Then initialize it inside our project folder:

```
jasmine init
```

This leaves us with something like the following directory structure, where we'll create a new file to contain our test suite. Let's call it `myTestSpec.js`, and we'll call our actual script mymodule.js:

<pre>
/project
  /spec
    /support
      jasmine.json
    myTestSpec.js
  mymodule.js
</pre>

### Write the tests

After we've done that, we can finally start to write our tests. Drawing from an example in [Jasmine's intro docs](http://jasmine.github.io/2.0/introduction.html):

{% highlight javascript %}
/*
* Using require.js is an easy way to bring the code we want
* to test into the testing environment
*/
var myModule = require('mymodule');

describe("A suite", function() {
  it("contains spec with an expectation", function() {
    expect(true).toBe(myModule !== null);
  });
});
{% endhighlight %}

Let's walk through the different parts of this:

__Suites__

Suites `describe` function our tests. The first argument is a string that describes what is being tested.

__Specs__

So far, our suite contains one spec, which we defined by calling `it`, another of Jasmine's global functions. The first argument given to `it` should be a short statement that defines what that specific test checks for.

__Expectations__

At the heart of our test lies `except`, which determines if our test will pass or not. You can call `except` with may different functions (toBe, toEqual, toMatch, etc.) depending on what you want to check for.

### Run the tests
Now that we've written some tests (and hopefully taken the time to write some functions in `mymodule` that are worth testing), we can go ahead and run this command, which will run all of the spec files specified in `jasmine.json`.

```
jasmine
```

As we continue to run and pass tests, we can keep going by writing more tests that explain the additional functionality that we desire, and then go on to implement more and more features.

## In conclusion

That's all there is to it! Now you should have a better idea of how to get started experimenting with TDD, which allows you to define the behavior of your program, write tests to verify that behavior, and then write the program to pass the tests. TDD is a great way to force yourself to really think about what you want to do before going in keys blazing and writing without giving testing a second thought.
