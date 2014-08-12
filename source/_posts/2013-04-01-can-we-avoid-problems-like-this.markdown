---
layout: post
title: "Can we avoid problems like this?"
date: 2013-04-01 14:35
comments: true
categories: 
---
Knockout is currently my favourite way of using ECMAScript at large. It essentially allows you to outsource event management and focus on business logic and views, but at a cost: events go in, and if they don't come out in the way you expected you are screwed. 

<!-- more -->

There are a couple of neat tricks I've picked up on the way:

	<p data-bind="text: console.log($data.someStateIWantToPoke)"></p>

This will always execute and gives you a value to play with in the console. 

But, one year into my experience with Knockout it continues to kick me at every given turn.

The difference between:

	<div id="user-details" data-bind="text: 'user'"></div> 

and 

	<div id="user-details" data-bind="text: user"></div>

is huge semantically, but syntactically they're almost identical. 


The same goes for:

	<div id="user-details" data-bind="text: users()"></div>

which should give you the result of calling the function users or if the variable is an observable, will give you its current value and an error if users is not a function or observable.

	<div id="user-details" data-bind="text: users"></div>

whose output will be the value of an observable, the value of a variable and weird error if users is a function.

There really isn't a good way of debugging these kinds of problems and knockout has no idea it is doing anything wrong, the syntax in all of these examples is perfectly valid, it all falls down to programmer error. 

But it feels like the frameworks fault. 

The flexibility of being able to use both a function and a variable in the same place definitely buys some brevity, but seems to introduce a class of bugs that I don't know how to prevent. I can't even unit test this code.

I have a feeling it might mean that I can't use Knockout anymore. I loose 7-8% of my time to errors like this in Knockout and I'm not sure what to do about it.

Anyone got any ideas?