---
layout: post
title: "Desk Checking"
date: 2012-10-10 05:38
comments: true
categories: QA, Development
---

If there is one practice guaranteed to reduce the number of bugs in a system it is desk checking.

The process should look something like this:

1. Developer believes they have finished a story
2. Developer integrates with the latest version of the code
3. Developer runs automated tests
4. Developer asks the QA to 'Desk Check' their code
5. QA runs a quick, manual smoke test that puts the system through its paces
6. If any errors are found, Developer and QA discuss mechanisms for automating that testing at a unit or functional level.
7. Developer implements those tests, fixes problem and repeats.

Up until step four this process should be familiar to all developers. In practice, steps four through six rarely take more than five minutes, so if it catches just a single bug it can save the project an hour of developer context switching. There are other subtle benefits as well. Letting developers see how you test can help reduce the time it takes to test even further, it may even help them to identify other problems.

All parties involved in defect resolution agree that early diagnosis and resolution of bugs is good for everyone, but 'Desk Checking' is often met with resistance from both QA and Developers.

I've experienced complaints from QA that desk checking interferes with normal testing, but really the attitude should be that 'normal testing' is the filler between desk checks, they are that effective.

It is also a little confrontational. Developers can react badly to having their weaknesses exposed so publicly and personally. This is just an unhealthy attitude on the part of the developer and should be considered a smell by their management. Remember, it may not be productive in the long run, but making a developer cry is a QA rite of passage.
