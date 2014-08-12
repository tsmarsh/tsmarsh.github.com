---
layout: post
title: "Breaking down the 10x developer myth"
date: 2014-06-25 19:33
comments: true
categories:
---

I took some time earlier this year to write the first [PESTful](http://tailoredshapes.com/blog/2013/04/21/pest/) service: [Inventory Server](https://bitbucket.org/tsmarsh/inventoryserver). This project was weird for me, predominantly because I finished it. 

<!-- more -->

Here are a few stats that I think are interesting about this:

Stat                            | Value
:------------------------------ | -----:
Lines of Code                   | 7000 
Number of Days                  |  75  
Number of commits               | 140  
Total number of lines commited  | 19000
Average hours per Day           | 2    
Number of developers            | 1    
Unit tests                      | 50   
Class coverage                  | 99%  
Method coverage                 | 96%  
Line coverage                   | 95%  

7000 lines of code is a pretty decent sized project, most of that was figuring out a strategy to allow an RDBMS to act like a [persistant data structure](http://en.wikipedia.org/wiki/Persistent_data_structure) and the layers of abstraction required to make it as generic as I could make it.

What is interesting to me is that I shouldn't be able to complete a 7000 line project as a solo developer. The [Mythical Man Month](http://en.wikipedia.org/wiki/The_Mythical_Man-Month) tells me that I should be grateful if I can get 10 lines a day out of an average developer. My industrial experience tells me that is probably close to the truth. That means it should take ~5 man years to complete. As is, I got it done in a little over 3.5 months (20 days per month), at 2 hours a day, 5 days a week.

Lets do some stupid math, to bump my ego, before I tear it to pieces.

    lines_of_code_per_day = 7000 / 75
    >> 93
    # w00t I'm a 9.3x developer!
    
    # except I only worked 2 hours a day
    work_day_adjustment = 8/2
    actual_work_days = 75 * work_day_adjustment 
    >> 19
    lines_of_code_per_day = 7000 / actual_work_days
    >> 368
    # w00t woo I'm a 36.8x developer

    #But I actually commited 19k lines
    lines_of_code_per_day = 19000 / actual_work_days
    >> 1000
    # AMAZEBALLZ l337s it hurtz

This is clearly bullshit. I'm good. I work hard. I spend a lot of personal time working on my trade, but I am not 9 times more effective than my peers, never mind 100 times. That is clearly ridiculous. 

## So what happend?


### Did I use some super terse language to enhance my productivity?

No. I used [Java 8](http://www.oracle.com/technetwork/java/javase/overview/java8-2100321.html), with most of the development done in Java 7 as [Jetty](http://www.eclipse.org/jetty/) has an issue with Java 8 annotations, until I figured it out. Whilst Java is definiely terse compared to COBOL, its still quite verbose. That said I did use IntelliJ. Which means that many of the lines of code I 'wrote', were really IntelliJ refactoring huge swathes of my code base. 

But that is interesting in itself. I chose a language which I knew had the worlds best refactoring tooling, and it worked, renames, signature changes, pull-ups were all handled automatically. IntelliJ also had the nerve to show me potential bugs via static analysis and fix them!

I also did the unthinkable and used [Maven](http://maven.apache.org/), at least I created a pom. Unit Tests were still run via IntelliJ, Maven just meant that I didn't have to configure my IDE for silly things like resources or test locations. The real benefit was that I already know Maven and its limitations. So almost no time was spent trying to bend it to my will.

I used technologies I was somewhat familiar with. I used Hibernate instead of... you know, I have no idea. Seriously, what other ORM technology gets out of the way so quickly, whilst providing you with a scalable NoSQL layer if you want it? I used Guice instead of Spring because... fuck Spring. Also, all I wanted was a DI container. The real choice was between Guice or Pico, I just knew a little more Guice, also fuck Spring.

You would have thought that would have reduced the learing curve... and it did, but there were still many things that I learned on this project:

* How Hibernate handles objects when the primary key changes on update (not very well, you have to clone, but not too deep, otherwise bad things happen)
* That Guice is great, but it is much happier when you do things its way, rather than your own. It forced:
  * A switch to Servlets from HTTPServer
  * A switch to JPA from pure Hibernate. (both of these changes turned out to be excellent)
* How Guice handles Robot Legs, which I just assumed all DI containers did, turns out its hard and I got lucky picking Guice because its solution was closest to my assumptions.

So even on a project where I had complete control I still 'lost' time learning how other peoples code worked.

The crazy thing is that this is where most of the time really went.

Sure, I only spent 2 hours infront of a computer, but I was thinking about these problems constantly. Not every waking moment, constantly. I would hit a problem on my commute. Close my laptop, complete a full day of work, and unconsciously solve the problem. The same thing happened during sleep.

I also spent 0 hours in meetings and no time explaining my design with other developers. This actually really worries me as I'm not sure if my design is good. The only validation I have is my own and that of my tests. 

So lets look at the stats again:

    lines_of_code_per_day = 7000 / 75
    >> 93
    # w00t I'm a 9.3x developer!
    
    # except I thought about this every moment
    work_day_adjustment = 24 / 8
    actual_work_days = 75 * work_day_adjustment 
    >> 300
    lines_of_code_per_day = 7000 / actual_work_days
    >> 23
    # I'm a 2x developler

    #But I actually commited 19k lines
    lines_of_code_per_day = 19000 / actual_work_days
    >> 64
    # Meh, I'm ok

That's still a 6x developer. What do I attribute that too?

### SOLID Design Principles

This is the first project where I tried as hard as I could to be as SOLID as possible. I have 100s of tiny classes that do one thing and one thing well. I found this to be a mixed bag experience. I never hit a wall, I didn't build a giant ball of mud, but almost every check-in affected multiple files.

This is fine in a single developer world, but if I had been working in a team it would would have been merge hell from day one. Most of the reason for this was merciless refactoring of interfaces caused by doing that simplest thing that would work followed by the right thing: red - green - refactor, splitting green and refactor into seperate commits. 

This is fine. I also probably wasn't SOLID enough. I didn't create abstractions around my interactions with libraries, partly because I'm not sure how I would with regards to DI and JPA. But I'm not sure I would have been faster in a team. Once I had a design in my head, the time to execute that design was definitely faster than the time it would have taken to explain my desing and then evaluate my co-workers implementation.

### Solo Working

I was the customer, architect, dba, software developer, developer in test and release engineer on this project. I am happy to report that we were all on the same page from day one. The customer understood when we went over the the original 4 week estimate. Release engineering got in early and created a vagrant environment which really helped. When it was realized that the project was larger than originially estimated the developer in test stepped in and wrote tests, then we worked together on TDD for the remaineder of the project.

I could do this because of a decade of experience working with some of best developers in the world (thank you, [ThoughtWorks](http://www.thoughtworks.com)) who were kind enough to besto on me a fraction of their talent. But this wasn't the optimal team configuration.

If I could change one thing about my team it would have been the introduction of a pair. One constant meeting is so much better than 20% of your time in break-out spaces. I can't fall back in to [flow](http://en.wikipedia.org/wiki/Flow_(psychology)) at the drop of a hat. Even 15 minute meetings can cost me hours of productivity. 

I suspect if I was pairing I would have more unit tests from the start and that my overall design would have been better. At the very least we would know that our code was readable by at least one other human.

### Automatic Testing

I started the project knowing that I wanted automatic acceptance testing, I wasn't sure I wanted unit tests. 

I think I hit a good balance with this project. The automatic acceptence testing was great. Fast tests that hit the API directly and exercised the full stack caught the majority of my bugs. Eventually I introduced more unit tests that helped me work out my design at a micro level. For the first half of the project I was happy with 85% coverage, but as I kept introducing more unit tests and getting closer to 100% I kept finding more really gnarly bugs in the edge cases.

Introducing fast, acceptance tests and then back filling with unit tests for edge cases, design and documentation is now my prefered system for automated testing. I distinguish this from BDD because my acceptance tests were mostly happy path and large, not one test per story. My entire suite (unit and acceptance) runs in 8 seconds, this meant I ran it habitually at almost every save point, not every commit.  


#### Dependencies

I started mocking dependencies, but pulled out quickly. As soon as I introduced [Mockito](https://code.google.com/p/mockito/) my test times rocketed (10-100x slower). As I was using Java, top-down design was pretty much demanded, so I needed something to test with, so I used stubs. I then replaced those with test objects when I had a couple of tests that used similar stubs, then ultimately with dependency injected (DI) controlled objects. 

That final migration to DI controlled objects was new for me. I'd always been told that unit tests should just test the object under test. That is definitely true, but unit tests are also the least interesting of all the tests. I am plagued by many years of writing unit tests that are green because of a poorly understood mock/stub/test object not because they actually work. It is fair to say that using DI means that you are only testing a single configuration, well, thats only true if you have a single test configuration. 

I used one configuration for my 'unit' tests, then three configurations for my integration tests: in memory; JPA; cassandra (which was eventually aborted). The all caught different flavour of bugs.

The greatest reason for adopting DI Controlled Objects was pragmatism. As the project matured and there became a greater shift towards refactoring rather than feature addition, it was the tests that didn't use DI that had to be changed the most.

Testing dependencies is a cross cutting concern for unit tests. It makes sense to me that it should therefor be handled in a cross-cutting way, much like DI.

#### Multiple Backends

For some crazy reason I got it into my head that the best way to make sure that my design was flexible was to implement two back-ends from the start: in-memory and rdbms. This was a stroke of genious that I plan on replicating on other projects. I could get something working really quickly in memory, test out the core logic, then port to JPA where all I had to worry about were issues configuring JPA and transactions, not business logic. 

It wasn't perfect. But it was really cool having my 'integration' tests work with multiple backends without issue. I really don't know if this made me go faster. There were times when I was asking myself, "What is the point of maintaining the memory backend", then it would catch a bug that the other configuration didn't. 

## Conclusions

This project was a lot of fun. Doing a project for the sake of doing the project not because I wanted to learn a new langauge / framework, meant that I finished the project when it was feature complete, not when I had finished evaluating the new technology. 

Revisiting the practices that I sell: SOLID, automated testing, DevOps was my biggest take-away. These practices work really well for me. Whenever I deviated from them I started to see the problems I see in my clients. 

Java is still a great tool, especially when used in combination with IntelliJ. It has its faults. I spent alot of time working with edge cases in generics (var args and generics do not get along), there was a lot of boiler plate, but I don't feel like it ever really slowed me down. I'll take tooling over verbosity any day of the week.

2 hours of coding a day is plenty. I don't have to be on task and focused for 8 hours a day in order to be more productive than a team of developers. Size is the enemy. Most dev shops would have made this a 4 dev project, with a qa and pm, and quoted 3 months. The overhead in maintaining that team, might be the majority of the time on that project. All that is really needed is one dev, and a flexible client who understands that coding isn't about sitting in front of a computer for 12 hours a day.


