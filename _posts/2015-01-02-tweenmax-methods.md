---
layout: post
title: "5 - [GSAP] TweenMax Methods (1)"
categories: lessons
---
We've talked about Tweens and Pins so far, and discovered a lot about ScrollMagic, but we haven't talked so much about what can be done with GSAP, the library that powers animations built with ScrollMagic, such as the TweenMax Object.

The only way we've been animating things so far is using the `TweenMax.to()` method, wich is one of the many methods of the TweenMax Object. We'll see 2 more this time and also 2 attributes we can use for even more animation awesomeness. Let's go !

# Setup

Here's the code I'll be starting with, pretty basic :

	<html>
		<head>
			<title>Learn ScrollMagic - 5 - TweenMax Methods</title>

			<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
			<script src="http://cdnjs.cloudflare.com/ajax/libs/gsap/1.14.2/TweenMax.min.js"></script>
			<script src="https://rawgit.com/janpaepke/ScrollMagic/master/js/jquery.scrollmagic.js"></script>
			<script src="https://rawgit.com/janpaepke/ScrollMagic/master/js/jquery.scrollmagic.debug.js"></script>

			<style>
				html, body {
					height: 100%;
					margin: 0;
					padding: 0;

					font-family: Helvetica, sans-serif;
				}

				.scene {
					margin: 200px 0;
					padding: 50px;

					outline: 1px dashed red;
				}

				.tween {
					padding: 20px;

					background: blue;
					color: white;
				}
			</style>
		</head>

		<body>
			<div style="height: 51vh">&nbsp;</div>

			<div class="scene" id="scene1">
				<div class="tween" id="tween1">Example 1</div>
			</div>

			<div class="scene" id="scene2">
				<div class="tween" id="tween2">Example 2</div>
			</div>

			<div class="scene" id="scene3">
				<div class="tween" id="tween3">Example 3</div>
			</div>

			<div class="scene" id="scene4">
				<div class="tween" id="tween4">Example 4</div>
			</div>

			<div class="scene" id="scene5">
				<div class="tween" id="tween5">Example 5</div>
			</div>

			<div style="height: 100vh">&nbsp;</div>

			<script>
				var controller;

				$(function() {
					controller = new ScrollMagic();
				});
			</script>
		</body>
	</html>

Nothing special, as always, if anything looks unusual or weird to you, go back to the previous lessons, it should all make sense.

# Brief recap first

First, to refresh our memory, let's start with a `TweenMax.to()` which we've been using so far on our first example. Let's make its background red.

	var tween1 = TweenMax.to('#tween1', 0.5, {
		backgroundColor: 'red'
	});

	var scene1 = new ScrollScene({
		triggerElement: '#scene1'
	})
	.setTween(tween1)
	.addTo(controller);

In this first example, we're doing an animation that changes the background color from its initial value (blue) **to** red. That's why we're using `TweenMax.to()` : to define the final state of our animation.

# TweenMax.from()

For our second example, the code will be almost identical to the first one, except we'll use `TweenMax.from()` instead of `TweenMax.to()` here's what the code looks like :

	var tween2 = TweenMax.from('#tween2', 0.5, {
		backgroundColor: 'red'
	});

	var scene2 = new ScrollScene({
		triggerElement: '#scene2'
	})
	.setTween(tween2)
	.addTo(controller);

If you refresh the page, you should see that our block *Example 2* is red when the page loads. Once you scroll through it, its background becomes blue.

The `TweenMax.from()` methods defines the initial state of the animation, so that when the animation ends, it has the default values, the ones set in the CSS code.

This is very useful, especially when the end state of the animation is what you want your page to look like if JavaScript is disabled, something that should be kept in mind as much as possible.

# TweenMax.fromTo()

For the third example in this lesson, we are going to mix `TweenMax.from()` and `TweenMax.to()` which enables us to define the starting and ending points of our animation. Here's what it looks like :

	var tween3 = TweenMax.fromTo('#tween3', 0.5, {
		backgroundColor: 'red'
	}, {
		backgroundColor: 'green'
	});

	var scene3 = new ScrollScene({
		triggerElement: '#scene3'
	})
	.setTween(tween3)
	.addTo(controller);

As you've probably noticed, we're passing one more object to this fonction, the end states, so the parameters are as follow :

* The Selector of the element
* The duration of the animation
* The starting state like a `TweenMax.from()`
* The ending state like a `Tween.to()`

Go back to the page, here you should see that the *Example 3* block starts with a red background and ends with a green one, and never becomes blue, as defined in our CSS.

Just like the `TweenMax.from()` method, this is useful when you want to ensure your content will be shown properly if JavaScript is disabled or not running for some reason, but the ideal styles for that are neither the initial or ending ones.

# The repeat attribute

In this new example, we're going to see a new technique, that's not really a method of TweenMax, but an attribute that can be given to a Tween, to make it repeat the animation over and over :

	// TweenMax.to() & repeat
	var tween4 = TweenMax.to('#tween4', 1, {
		backgroundColor: 'red',
		repeat: -1
	});

	var scene4 = new ScrollScene({
		triggerElement: '#scene4'
	})
	.setTween(tween4)
	.addTo(controller);

Now if you refresh and scroll, you'll see that once you go below the start of the Scene, the background transitions from blue to red, then goes back to blue *instantly* and starts over.

One thing that may be an issue here is that this animation never stops, unless you scroll back up above the "start" of the Scene. To fix that, you can add a duration to the Scene, in the [demo page]({{site.baseurl}}/demos/5-tweenmax-methods.html) I've set it to `duration: $('#scene4').outerHeight()` but you can put whatever floats your boat in there.

It's important to note that in this example, the value we've used is **-1** which means that the animation repeats itself indefinitely. It's possible to use another integer, such as 4 for example. If we change to that value, you'd notice that the animation repeats itself 4 times, based on the scroll, so it will play once during the 25% of the Scene's duration, go back to the initial state and play again from 25 to 50% of the duration, again from 50 to 75% and one last time from 75% to the end.

# The yoyo attribute

Last example of this demo, the `yoyo` attribute. It's a really useful attribute when used in conjonction with the `repeat` attribute. What it adds to the `repeat` attribute is that upon reaching the end state of the animation, it will then play backward toward the initial state, instead of *jumping* to it, before the animation plays again. Here's what the code looks like :

	var tween5 = TweenMax.to('#tween5', 1, {
		backgroundColor: 'red',
		repeat: -1,
		yoyo: true
	});

	var scene5 = new ScrollScene({
		triggerElement: '#scene5',
		duration: $('#scene5').outerHeight()
	})
	.setTween(tween5)
	.addTo(controller);

Refresh and enjoy : the *Example 5* block goes smoothly from red to blue and then blue to red.

It's also possible to use another value than -1 for the `repeat` attribute when using the `yoyo` attribute. When doing so, just like it would without it, the animation will repeat itself based on the scroll position during the duration, but it will play backward smoothly instead of jumping back to the initial state when reaching the end of the animation.

### Expected result

As we've seen in this lesson, GSAP's TweenMax provides many useful ways to create animations. You can see the recap of what we've done so far on the [demo page]({{site.baseurl}}/demos/5-tweenmax-methods.html), as always.

### Going further

GSAP's documentation is pretty nice and will help build better animations and show you many tricks, you can start by taking a look at the [TweenMax doc](http://greensock.com/docs/#/HTML5/GSAP/TweenMax/) for instance, to start wrapping your head around things.

Keep in mind though that we're using GSAP as a building block of our animation and not everything will be useful for us when building scroll-based animation using ScrollMagic.