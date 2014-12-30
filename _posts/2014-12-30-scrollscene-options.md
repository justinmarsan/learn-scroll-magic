---
layout: post
title: "2 - ScrollScene Options"
categories: lessons
---
Building up from the previous lesson, we're going to create a bunch of new animations to toy with the ScrollScene options a bit.

I recommend you take a look at ScrollMagic great [documentation](http://janpaepke.github.io/ScrollMagic/docs/ScrollScene.html#ScrollScene). If you click on the first item labeled *new ScrollScene(options)* you'll see the details of what options can be passed to a ScrollScene, like the `triggerHook` we used previously.

# Preparing for multiple animations

By the end of the previous lesson, we had only one animation using two divs : `#container` and `#block`. Because we're going to create more animations on the same page and to reuse the style, we'll simply update the css a bit to use class names instead of IDs, and modify the markup accordingly :

	.container {
		margin: 100px 0 100px;
		padding: 50px;

		outline: 1px dashed orange;
	}

	.block {
		padding: 10px;

		border: 1px solid black;

		font-family: Helvetica;
	}

**You should also notice that I've changed the margin value of the `.container` to `100px 0 100px` instead of `51vh`. Doing so, you'll see that once refreshing the page, the "trigger" thick is already below the "start" thick, so the animation is executed right as the page shows. We'll fix that in a second !**

And for the markup :

	<div id="container1" class="container">
		<div id="block1" class="block">
			Hi there !
		</div>
	</div>

	<div id="container2" class="container">
		<div id="block2" class="block">
			Hi there !
		</div>
	</div>

	<div id="container3" class="container">
		<div id="block3" class="block">
			Hi there !
		</div>
	</div>

	<div id="container4" class="container">
		<div id="block4" class="block">
			Hi there !
		</div>
	</div>

	<div style="height: 100vh">&nbsp;</div>

*I understand `#container1` and `#block1` aren't great semantic IDs but they'll make it clearer in our code later what elements we are targeting. Animations don't mean your code has to be unclear so make sure to be consistent and precise in your procjets, you'll see the code to move things around quickly piles up, better make it meaningfull in your projects !*

So, here we are, let's start using the options, as you've probably guessed by the markup above, we'll see 4 of them. Ready ? Great !

The last div is there for conveniance, to ensure we get enough space at the bottom of the page to be able to scroll as much as we want for our examples. It's re-using the `vh` unit we saw in the previous lesson. *I'm not adding padding-bottom to the body because that would change the viewport height, and therefor move the "trigger" thick to the bottom.*

# Scene offset

The first thing we're going to do is manipulate the position of the "start" thick, so it starts below the "trigger" part and actually runs the Tween when we scroll.

If you're starting from scratch, here's what your JS script should contain (with the updated selectors for the Tween and the Scene) :

	var controller;

	$(function() {
		controller = new ScrollMagic();

		// First Animation
		var changeToRed = TweenMax.to('#block1', 0.5, {
			backgroundColor: 'red',
			color: 'white'
		});

		var whenInContainer = new ScrollScene({
			triggerElement: '#container1'
		})
		.setTween(changeToRed)
		.addTo(controller)
		.addIndicators();
	});

Now we're going to add a new options to our ScrollScene : **offset** :

	var whenInContainer = new ScrollScene({
		triggerElement: '#container1',
		offset: 400
	})
	.setTween(changeToRed)
	.addTo(controller)
	.addIndicators();

If your refresh your page (and assuming you're viewport is less than 900px high, if not increase the offset value), the "start" thinck should be below the "trigger" one. It is ? Good, now scroll ! When the "trigger" reaches the "start", the animation should play.

### What happened there

**By default the offset value is 0**, meaning the "start" is at the very top of the trigger element. By using 400 (or something different depending on your viewport size), we're moving that "start" thick 400px toward the bottom of the page.

Of course, the offset value can also be negative, to push the "start" toward the top of the page too.

# Scene duration

For the second animation, let's copy the Tween and Scene declaration of our first animation, change the selectors first :

	// Second animation
	var changeToRed2 = TweenMax.to('#block2', 0.5, {
		backgroundColor: 'red',
		color: 'white'
	});

	var whenInContainer2 = new ScrollScene({
		triggerElement: '#container2',
		offset: 200
	})
	.setTween(changeToRed2)
	.addTo(controller)
	.addIndicators();

*I also changed the offset value so it looks better on my screen.*

If you refresh your page, you should see that we now have to "start" thick on our screen, and each animation, for our first and second blocks, starts when the "trigger" reaches one then the second "start" thicks. It does ? Good.

Now let's add a `duration` option to our Scene :

	var whenInContainer2 = new ScrollScene({
		triggerElement: '#container2',
		offset: 200,
		duration: 100
	})

Take a look at your page now. An "end" thick should have appeared. Now if you scroll and make the "trigger" go from the "start" to the "end", you should see that the animation is now progressive, so it finishes upon reaching the end of the Scene.

**Important :** Using the duration option on a Scene *overrides* the duration parameter of the Tween we had set to `0.5`.

### What happened this time

In our first animation, a 0.5s animation runs when the "trigger" reaches the "start", in this second example, the Tween is executed progressively through the duration of the Scene.

This duration, that we've set to 100 means the "end" thick is *100px* below the "start" thick.

# Reverse animations

In our first two examples, when scrolling up, the animation plays backward, either in a timed way for 0.5s for the first example, or progressively in the second from "end"  to "start". While this behaviour is cool in many cases, it might not be what you want all the time, good thing there's a way to prevent that, right ?

I'll let you figure out the code, it's mostly copy-paste from the previous example, here's what changes :

	var whenInContainer3 = new ScrollScene({
		triggerElement: '#container3',
		duration: $('#container3').outerHeight(),
		reverse: false
	})

I removed the offset value (because the `#container3`) is below the initial "trigger" so I don't really need it. It equals to òffset: 0`.

I used the outer height of the container as a duration. That's not really needed, I just want to point out that it doesn't have to be a fixed number, you can do calculations in there to get it, use a variable, or whatever.

`reverse: false` is the new part.

Test it out, it should be really straight-forward : refresh your page, scroll down, all 3 examples should turn red, scroll back up, the first two should go white again, and the third one remains red. Scroll back down, and it still stays red, the animation doesn't play again.

**Note :** If you refresh and scroll down to the middle of the duration, then scroll back up, the animation will freeze partially played out and stay this way until you scroll further than when you stopped and it will play out more. It's the "expected" behaviour but it's still good to take notice, just se you're not baffled later when you realize it.

# Logging

For our last example, I'll use the same code, applied to the last block, remove the reserve option (which defaults to true) and add a new `loglevel` parameter with a value of 3 :

	// Fourth animation
	var changeToRed4 = TweenMax.to('#block4', 0.5, {
		backgroundColor: 'red',
		color: 'white'
	});

	var whenInContainer4 = new ScrollScene({
		triggerElement: '#container4',
		duration: $('#container4').outerHeight(),
		loglevel: 3
	})
	.setTween(changeToRed4)
	.addTo(controller)
	.addIndicators();

Scroll through your page, you should see the same animation as for the third example, except it also plays in reverse when scrolling up. Not much has changed, unless you open your browser's console. Whenever you scroll, lines and lines of information should appear !

At first, they should look something like this :

	16:03:04:436 (ScrollScene) -> event fired: update -> Object {startPos: 385.5, endPos: 525.5, scrollPos: 137}

What this means is that the whenInContainer4 ScrollScene object fired an event, the `update` on, with the following parameters : 

* `startPos` which is the position of the "start" thick relative to the top of the page, in px
* `endPos` which is the position of the "end" thick
* `scrollPos` which is the position of the "trigger" thick when the event is fired.

You'll see that the `update` is fired every time your scroll position changes, whether the "trigger" is inside the Scene or not.

If you now scroll so that the "trigger" is inside the Scene, the log changes a little :

	16:07:01:116 (ScrollScene) -> event fired: progress -> Object {progress: 0.03214285714285714, state: "DURING", scrollDirection: "FORWARD"}

The event that gets fired is now called `progress` which makes sense since the animation is *in progress* !

The parameters change too :

* `progress` is how much of the animation has been played, 0.03214285714285714 equals to 3.21% of the animation
* `state` *I'm not sure what that one means, I'll have to do some more research and update this
* `scrollDirection` can either be `FORWARD` when scrolling down, or `REVERSE` when scrolling up.

You probably won't need to use this, but it's useful and gives you some ideas of what data is available in the ScrollScene events, that'll be the subject of another lesson though.

### The different values of the loglevel option

We've used the value of `3` but there are others, as stated in the [doc](http://janpaepke.github.io/ScrollMagic/docs/ScrollScene.html#ScrollScene), the accepted values are as follow :

* 0 : silent, no logs
* 1 : errors only (hopefully you won't see any)
* 2 : *default value* errors and warnings (again, I hope you don't see those)
* 3 : errors, warnings and debug informations (the one we just used that provides a looooooot of informations for debugging indeed)

Finally, to make the page look nicer, you should probably remove all the `addIndicators()` so the thicks disappear, or keep it that way, that's up to you.

### Expected result

As always, you can check out the [demo page]({{site.baseurl}}/demos/2-scrollscene-options.html) to see what things are supposed to look like. Be sure to also take a look at the console for the debug informations.

### Going further

* Try and synchronize the start of all animations
* Change the Tweens so they're not the same for all animations, get crazy, think CSS3 fun stuff !