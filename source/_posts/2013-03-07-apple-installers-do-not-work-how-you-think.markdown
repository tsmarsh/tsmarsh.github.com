---
layout: post
title: "What do Installers do?"
date: 2013-03-07 21:07
comments: true
categories: 
---
How do Apple's .pkg installation files work?

Today I learned the answer to a couple of questions I didn't realize I had:

Why are you prompted to install to a volume, not a directory?

Can I really keep applications outside of the /Applications directory?
<!-- more -->

The basic premise of an installer is simple. You build a file that contains enough information to put the right files in the right places and some smarts to register them with any part of the OS that demands to be told when these things happens. That is how it works on Linux, Windows and Java and that was my assumption for OS X.

The tools are set up to let you believe that is how it works too.

	productbuild --component path/to/Some.app /Applications Installer.pkg

This should read something like: build a product called Installer.pkg that places Some.app in /Applications. 

Simple. 

Except that isn't actually how it works at all. 

Despite being a long time Mac user and knowing that I could run applications from anywhere, I just assumed that was at the cost breaking future upgrades. It turns out that I didn't have to worry, you see the /Applications in that command is just where it will put files if all else fails and because of some sophisticated thinking on Apples part.

The .app contains a Info.plist, and that contains a unique string that identifies the application to... spotlight. 

So when the installer is picking a location each file, if that bundle contains an Info.plist it looks the file up in spotlight and installs it over the first bundle that matches that string, not the install location. If you are testing you freshly built package that is most likely the .app that you just built your package from, not /Applications. This isn't documented in the man pages for pkgutil, pkgbuild, productbuild or installer and I strongly suspect it is the source of a lot of the confusion around the 'bugs' in the much maligned PackageMake... well some of the reasons. 

So why can't you pick a more granular install location?

You are not picking an install location you are picking a search root for Spotlight. If a match is found for the bundles you are installing the will be installed at that location. 

Can you really keep Applications anywhere? 

Sort of. Apple appears to have gone above and beyond to make that truth, but its too easy for developers to make assumptions about the locations of files and hard code them in to the bundle. An inability of an app to exist outside of /Applications should be considered a bug.
