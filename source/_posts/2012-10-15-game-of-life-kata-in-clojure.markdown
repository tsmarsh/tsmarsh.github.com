---
layout: post
title: "Game of Life Kata in Clojure"
date: 2012-10-15 07:36
comments: true
categories:  Development, Clojure
---

Being an ex-Guardianista Conway's Game of Life has a special place in my heart. The problem is simple enough that you can express it in 6 or 7 cards and devious enough that two developers rarely come up with the same solution twice. It can be easily test driven and, in Java, the naive approach takes about and hour and a half to complete and usually spans three to five classes. 

It is a neat problem to put a language through whilst you're learning it. 

Things I really liked about Clojure:

* [Leiningen](http://example.com/ "Optional Title") : the best build tool I've ever user. Easy to configure. Easy to find help on.
* [Midje](https://github.com/marick/Midje): lovely little testing framework that doesn't feel like it is getting in your way
* Being able to make my solution parallel simply by making map -> pmap (not that it helped performance)

Things that I learned to like:

* Immutable objects: I thought I was pretty good at making my methods idempotent in OO code. I was surprised at how much I had to shift my thinking to get it to work for real. 
* Functional style: I really though this would be a breeze coming from Ruby / Python / Javascript, but functions without objects are completely different. What really surprised me is that loosing the structure and state embodied in objects actually made the solution feel lighter and me for more powerful.
* REPL: Immutable objects and strict scoping for values complicates the REPL experience. But it is still very nice to have a REPL, vs Java / C#.
* (): They really do melt away. 

Things that I didn't like:

* Clojure start-up time: it really is a second more than the usual JVM start up, enough to affect the frequency of running the tests compared to python or ruby.
* Debugging: I tried and failed to use trace and although you get some feed back from the exceptions, it was not as laser guided as Java or C#. 
* Learning curve: Learning new languages is fun for me. I've been programming since I was five years old in Logo and Basic. I learned Java and Haskell at University and I've been programming professionally in Java, C#, Python and Ruby for the last 8 years. Clojure was the toughest of all of them. It was like learning to code from scratch all over again. 
* Readability: Clojure lends itself to a terse style of programming and I can imagine finding it hard to read my own code in a couple of weeks. Python and Java have it beat. 
* IDE Support: Syntax highlighting and paren matching is about as good as it gets. That is plenty though. I've heard good things from Emacs users about Clojure support, but have no experience myself. 

I've come away from this experience a lot more positively than I expected too. Clojure is a language that has opened my eyes to complete different way of solving problems, and I liked it. There is a maturity in the ecosystem that is sorely missing from C#, Python and Scala in the form of leiningen which is comparable to Bundler and significantly better than Maven.

My biggest concern is that I have no idea how I would sell Clojure on a project. It is hard enough to find developers who can work in Java and C#, and harder still to find Ruby developers. I'm not sure I could apply the approach that the Guardian does for Scala and hire talented Java developers and then train them in Clojure. I relied far more on my Computer Science degree to get me through the Kata than my Java experience. Scala advantage here is being able to write Scala like Java with closures and type inference. If universities started to teach Lisps with the same passion as they do C style languages this would be less of a problem.

There are some problem spaces where Clojure looks really attractive. Expert systems written using core.logic look very interesting, but they are competing head on with Drools which has a significantly lower cognitive cost of entry even if Clojure definitely has a syntax advantage in that space. 

Concurrency is another area where Clojure makes excellent choices but my experience tells me that you really need close to a hundred cores to make concurrency pay because of synchronization costs. If you need concurrency to deal with IO delay then the Disruptor pattern or just an event loop provide much better bang for buck and even languages with global interpreter locks (GIL) like Ruby and Python are more than adequate for that. If you have an embarrassingly parallel computation problem Clojure may get you to a solution faster. Projects like [Parallella](http://www.kickstarter.com/projects/adapteva/parallella-a-supercomputer-for-everyone) might also help prove that multi-core is the near future. If you can genuinely out perform a $1000 server with $99 of arm chips with 16 co-processors that is a compelling reason to look at Clojure / Go / Scala. 

DSLs are another area where Clojure excels. Macros mean you can re-write the syntax in the language with no runtime penalty. I'm just not sure that DSLs are compelling on their own and I've seen enough bad DSLs in Ruby to know that they are no panacea.

So that is my view on Clojure. It is a fantastic language that is looking for a solution that can justify its learning curve. 

