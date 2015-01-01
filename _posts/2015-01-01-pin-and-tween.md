---
layout: post
title: "4 - Pin and Tween"
categories: lessons
---
This is our fourth lesson and things are getting more and more exciting now : we've seen how to animate things depending the the scroll position, how to have elements get fixed while the page scrolls, now **we're going to do both together !**

# A Pin and a Tween walk into a bar

For our first example, we'll have a Scene containing an element that gets pinned and another element that get tweened.

I'll let you do the preparation work, here's the markup I used :

	<div style="height: 51vh">&nbsp;</div>

	<div class="scene" id="scene1">
		<div class="pin" id="pin">
			Don't move !
		</div>

		<div class="tween" id="tween">
			I'm getting all red...
		</div>
	</div>

	<div style="height: 100vh">&nbsp;</div>

The css that goes with it :

	.scene {
		margin: 200px 0;
		padding: 50px;

		outline: 1px dashed orange;
	}

	.pin {
		padding: 20px;

		background: blue;

		color: white;
	}

	.tween {
		padding: 20px;

		outline: 1px dashed rgba(0, 0, 0, 0.2);
	}

The page should now contain one blue block and one with light grey outline, stacked one onto another.

### Now let's get to the real business !

First, we want to create a Pin of 200px of duration, so we'll create a new ScrollScene object using `#scene` as our trigger element and use the `setPin()` method to pin the `#pin` block.

I'm sure you can figure it out but just in case, here's what that looks like :

	var scene1 = new ScrollScene({
		triggerElement: '#scene1',
		duration: 200
	})
	.setPin('#pin')
	.addTo(controller)
	.addIndicators();

Hopefully you remembers to add the scene to the controller and maybe you added the indicators to see what is going on. If you now refresh, you should notice that :

* The element now gets pinned, yey !
* Space has been added between the `#pin` and the `#tween` element, so that they **end up stacked** by the end of the scene

### Let's add a tween now

First, we need to create a TweenMax animation before we create our scene, so that we can reference that Tween later and link it to the Scene. We did that 2 lessons ago, I'm sure you remember what that looks like. In my example, I change the background color and font color of the `#tween` div to respectively red and white.

	var tween1 = TweenMax.to('#tween', 0.5, {
		backgroundColor: 'red',
		color: 'white'
	});

We also need to use `.setTween(tween1)` to our Scene to link Tween and Scene together and now refresh again : A Pin and a Tween happening at the same time !

### Pin option `pushFollowers`

We've seen ScrollScene options so far, there are also options that can be passed to the `.setPin()` function, including one that's quite interesting : `pushFollowers`.

The space between our 2 elements is useful so they end up stacked by the end of the Scene. This is done by pushing the second block down. In some cases, you might not want that, how'd you deal with that ? Easy !

	.setPin('#pin', { pushFollowers: false })

If you now refresh the page, you see that the second element (and the rest of the page for that matters) doesn't get affected by the pin, elements aren't moved from their initial position. This is quite a useful feature that we'll be reminded of when going through the Pin Options in another lesson.

# Pinning and Tweening a single element

Let's copy our `#scene1` div and the `#pin` in it to make a new example :

	<div class="scene" id="scene2">
		<div class="pin" id="pin2">
			Don't move but do get red !
		</div>
	</div>

Exactly like we just did, we'll create a Scene on `#scene2`, set a Pin to it on `#pin2`, create a Tween changing the colors, apply it to `#pin2` as well and see how that looks. I'm sure you can do it all by yourself, don't look at the code just yet, give it a shot.

	var tween2 = TweenMax.to('#pin2', 0.5, {
		backgroundColor: 'red',
		color: 'white'
	});

	var scene2 = new ScrollScene({
		triggerElement: '#scene2',
		duration: 200
	})
	.setPin('#pin2')
	.setTween(tween2)
	.addTo(controller)
	.addIndicators();

Congratulations if you managed to do that by yourself, you've now mastered the two most useful features of ScrollMagic already and can start using it on many different ways to create cool animations !

### Expected Result

Pins and Tweens are the bread and butter of ScrollMagic animation, opening the door to many different kind of things. Here's what the [demo page]({{site.baseurl}}/demos/4-pin-and-tween.html) looks like by the end of this lesson, even though this looks very basic, using your creativity you can build many types of animations with those, in a few lessons we'll start deconstructing some pretty websites with great animations so we can really put the pieces together on real-life examples.

### Going Further

* Tween an element that's **inside** a pinned element