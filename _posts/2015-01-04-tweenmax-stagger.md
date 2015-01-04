---
layout: post
title: "6 - [GSAP] TweenMax Methods (2)"
categories: lessons
---
In the previous lesson we've seen few ways to create our animations, using `TweenMax.to()`, `TweenMax.from()` and `TweenMax.fromTo()`. These 3 methods of the TweenMax Object aren't the only ones available in our toolkit, in this new lesson we will see 3 more, which are used to **chain the same animation on multiple objects**. Pretty cool, right ?

Let's say you wanted 5 blocks , in a given Scene of duration 0, to turn red one after another. Using the techniques we've seen so far, you would have to define a ScrollScene object, a tween for the first block, make it go red, then do that again and again for the 5 blocks, with delays and stuff. Pretty painful if you ask me. Now we're going to discovers the Staggers methodes, there are 3 of them and you'll see how handy they come in, in this kind of scenarion.

# The setup

Here's the code we will start with :

	<html>
		<head>
			<title>Learn ScrollMagic - 6 - TweenMax Methods (2)</title>

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
				<div class="tween" id="tween2">Example 2</div>
				<div class="tween" id="tween3">Example 3</div>
				<div class="tween" id="tween4">Example 4</div>
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

As you can see, while we used to have one Scene per Tween in our previous examples (which is not necessary, just easier for the demos) here we have 5 `.tween` inside our Scene.

# TweenMax.staggerTo()

Ready ? Let's go :

	var stagger = TweenMax.staggerTo('#scene1 .tween', 1, {
		backgroundColor: 'red'
	},
	0.3);

	var scene1 = new ScrollScene({
		triggerElement: '#scene1'
	})
	.setTween(stagger)
	.addTo(controller)
	.addIndicators();

Couple of things to notice :

* The method is called `staggerTo`
* It looks like a `TweenMax.to()` except for the last `0.3` parameter
* The selector we use, `#scene1 .tween` returns an Array of 5 elements while we used to give 1 element (such as `#tween1) with previous methods

Now if you take a look at the result, you should see a really pretty animation, kind of a waterfall of animation. Cool isn't it ?

The `TweenMax.staggerTo()` method really is just the same as a `TweenMax.to()` method except we give it an Array of elements to animation, and the delay **between the start** of each animation.

In our example above, an animation starts every 0.3 second, each playing for 1 second.

# TweenMax.staggerFrom()

If `TweenMax.staggerTo()` is just like `TweenMax.to()` you can probably guess what `TweenMax.staggerFrom()` is going to do right ?

I'm sure you guessed right : it'll create a waterfall animation of multiple elements, going from the passed styles to the ones defined in the CSS code, just like `TweenMax.from()` does to a single element.

Update the code you've just written, to see it live in your browser. It's just one method to change, don't tell me you can't do that !

# TweenMax.staggerFromTo()

And lastly, still as obvious as the previous one, the waterfall version of the `TweenMax.fromTo()` method :

	var stagger = TweenMax.staggerFromTo('#scene1 .tween', 1, {
		backgroundColor: 'red'
	},
	{
		backgroundColor: 'green'
	},
	0.3);

	var scene1 = new ScrollScene({
		triggerElement: '#scene1'
	})
	.setTween(stagger)
	.addTo(controller)
	.addIndicators();

I don't need to explain that, it's so repetitive it hurts.

Let's leave this lesson here, this is all really simple to grasp if you understood everything so far.

### Expected result

You can see the result of the `TweenMax.staggerFromTo()` animation we created on the [demo page]({{site.baseurl}}/demos/6-tweenmax-stagger.html), I don't know about you but I find that really cool, considering how easy ScrollMagic and GSAP make it to create this kind of animations !

### Going further

I'm sure you're curious what it looks like if you set a duration on the Scene... No ? Well you should be, it's cool too !

