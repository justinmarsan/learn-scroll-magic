---
layout: post
title: "1 - First Tween"
categories: lessons
---
In this first lesson, now that's we have our page setup to start coding, we'll create our first tween !

*Wait, what's a tween*

The term is used pretty broadly, but you can think of it as a transition. In order to create an animation using ScrollMagic, we'll always need *at least* two things : a Tween (how things change) and a Scene (defining when that change happens).

For our first tween, we are going to do something very basic, you can check the end result on this lesson's [demo page]({{site.baseurl}}/demos/1-first-tween.html) : change the background and text colors of a block when scrolling on its parent.

*The "trigger" and "start" thick (and the "end" one we'll see in the next lesson) are created by ScrollMagic's debug feature, quite neat, right ?*

# The markup

First things first, we need to add some markup to our page to create the elements. We'll start with the HTML to give us an idea of the structure we'll be working with, it looks like this :

	<div id="container">
		<div id="block">
			Hi there !
		</div>
	</div>

This goes inside our `body` tag, that goes without saying !

The div `#container` shown with dashed orange outlines in the [demo page]({{site.baseurl}}/demos/1-first-tween.html) is the one we will be basing our Scene onto. It's the *context* of the animation.

The div `#block` is what we're going to animate, you can put any content in here of course, I decided to put some basic text, we'll take more time later to work on pretty examples, for now we're simply trying to figure things out !

## A `script` to hold our code

Next thing we'll need to add in our markup is a `script` tag to hold our own JavaScript code. You can put that in the `head` or, preferrably, at the end of your page, just before the closing `</body>` tag.

	<script></script>

## A `style` for our CSS

To make things easier to manage, I decided to put my styles in a `style` tag inside the `head` of my document, it's up to you to decide if you want to use an external file or a tag or anything else for now, either way we'll need to do some css so prepare that too.

# Adding some styles

In order to function properly, there are a few lines of CSS that we'll add to our page for ScrollMagic to function properly. Here's the code :

	html, body {
		height: 100%;
		margin: 0;
		padding: 0;
	}

Nothing too fancy here, we simply need to give `html` and `body`a height of `100%` so that ScrollMagic can calculate the viewport height properly. I'm not entirely sure why this is needed, I'll have to do some research, for this doesn't affect the way we'll code the rest of our page, so don't think about it to much, just make sure it's there.

**To Remember :** If at some point the position of your "trigger" indicator doesn't make any sense, make sure you have this CSS in your code, without it the trigger will be placed at the middle of the whole page instead of the middle of the viewport, which could give funky results !

## Additional CSS for our demo

Let's style our page just so we can see the 2 elements we'll be working on.

	#container {
		margin: 51vh 0;
		padding: 50px;

		outline: 1px dashed orange;
	}

If you're not familiar with it, the `vh` unit is a percentage of the *viewport's height* so we start off by adding 51vh of margin above and below our container, just so it start a little below the middle of our page and makes it taller than the viewport so we can actually scroll (or that example would be fairly lame, right !).

We add some padding for prettiness, and an outline to see the boundaries of our element and understand things better in the next stage.

	#block {
		padding: 10px;

		border: 1px solid black;

		font-family: Helvetica;
	}

Now we style the block we're about to animate. Nothing specific here, it's all very simple, right ?

# Getting ScrollMagic started !

Okay now the fun part, *let's make things move !*

First we need to **initialize ScrollMagic**, let's jusmp to our `script` tag and add the following code :

	var controller;

	$(function() {
		controller = new ScrollMagic();
	});

Assuming you're familiar with JavaScript and jQuery, this should'nt look too weird to you. We're creating a `controller` variable in our global scope (in case we want to use it later on, elsewhere) and then, when the page is ready, we create a new ScrollMagic in this `controller` variable.

This controller is what is going to hold our page and all its animations together, we'll barely use it ourselves but it'll be needed by all our animations to run properly.

## Finally, the Tween !

Let's add, after the controller is initialized, the code for our first tween, it looks like that :

	var changeToRed = TweenMax.to('#block', 0.5, {
		backgroundColor: 'red',
		color: 'white'
	});

Let's take a look at that in details :

First we save everything we just coded in a `changeToRed` variable. This is because the Tween is only half of what we need to do for our animation, remember, we also need a Scene, so we'll have to reference them later, so we store them in variables.

TweenMax is part of the GSAP we've included in the previous lesson, it's the library used to create the animation. Here we are doing a `TweenMax.to()` kind of animation, we'll be seeing more later.

The first parameter of the function is `'#block'` which is a selector that returns the block we want to animate.

The second parameter is the duration, in seconds of our animation.

The third parameter is where you can let your creativity live, it's an object holding the css proprieties and values you want to transition to when the animation is called. Here I'm changing the background color and the text color.

*Because the CSS proprieties and values are defined as a JavaScript Object, the proprieties containing hyphens are change so `background-color` becomes `backgroundColor`, but the values don't change but are surrounded by `'`*.

Once you get the hang of it it's really not that difficult, let's keep going, it'll all make sense when you see it live in your page !

## Defining our Scene

Now that our animation is prepared, we need to defined when it happens. This is done using a Scene, we'll start with a minimalistic example and look into many of the options available later on.

	var whenInContainer = new ScrollScene({
		triggerElement: '#container'
	});

As for the Tween, we're storing everything in a variable, `whenInContainer` for this one.

We're then creating a new ScrollScene Object, which is part of the ScrollMagic library.

It's only parameter is a JavaScript Object, where we'll put our options. Right now we'll only use one : `triggerElement` which is going to define what elements is used as a scene for our animation. We set it to `'#container'` which is a selector to our div element with the dashed orange borders.

Now we need to use 3 functions to glue everything together :

* Link the Scene and the Tween
* Link the Scene and the Controller
* Show the helpful Indicators to understand our animation

Here it goes :

	whenInContainer.setTween(changeToRed);
	whenInContainer.addTo(controller);
	whenInContainer.addIndicators();

A more consise Scene definition would look more like this, using chaining :

	var whenInContainer = new ScrollScene({
		triggerElement: '#container'
	})
	.setTween(changeToRed)
	.addTo(controller)
	.addIndicators();

# Testing it

Now if you go to your page, you should see that when the "trigger" thick reaches the "start" one, the box turns red with its text becoming white.

*Tada !*

Let's do a quick recap of the important things we did here :

* We added some required CSS for ScrollMagic to function properly (html & body height)
* We initialized ScrollMagic in a `controller` variable.
* We created our first Tween
* We created our first Scene
* We glued everything together link Scene and Tween, and Scene and Controller
* We added the ScrollMagic Indicators, to understand everything going on

### Expected result

Here's what your page is supposed to look like now that you've completed this lesson : [demo page]({{site.baseurl}}/demos/1-first-tween.html).

If it doesn't look or behave like that, re-start the tutorial, check out the source code of the [demo page]({{site.baseurl}}/demos/1-first-tween.html) and see where you went wrong.

### Going further

It's all fun and nice, we've animated our first element. In the next lesson we'll see how to take this a little further, if you want to keep playing, here are a bunch of ideas :

* Animating using different css proprieties
* Add more containers and blocks to the page for moar scrolling awesomeness
* Animate ALL THE THINGS !