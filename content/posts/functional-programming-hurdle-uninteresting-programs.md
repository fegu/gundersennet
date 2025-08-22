---
_edit_last: "1"
author: wpgundersen
categories:
  - haskell
date: "2013-10-22T11:25:04+00:00"
guid: http://www.gundersen.net/?p=164
parent_post_id: null
post_id: "164"
tags:
  - fp
  - functional-programming
title: 'Functional Programming Hurdle: Uninteresting programs'
url: /functional-programming-hurdle-uninteresting-programs/

---
Functional Programming, as a paradigm, does not have users, it has devotees. I am myself one of those, spending time in Haskell as often as I can.

_Why then, if it is so great, has FP not taken over the software development world?_

There are many answers to this question, ranging from the condescending ("is hard", "has no practical use") to the hopeful ("just haven't reached critical mass"). This post is about another, often overlooked, fundamental reason:

_Most programs are mostly about I/O_

Corporate programmers, in flavours of start-up to enterprise, mainly spend their days designing software that takes user input, moves it over the network, stores it in a database and even has the ability to fetch it back again. The furthest you get into processing data on a typical day is perhaps report generation. Then, outside of your main project, your build, deployment and test scripts are all about I/O as well.

The interesting tasks, such as rendering, query optimization or data aggregation is all handled by platforms or tools. Which is efficient, since those things can be time-consuming to develop, and you are paid by time units.

While the notion that most programs are mostly about I/O seems obvious to the legions of corporate programmers using Java, PHP and C#, it is not to everyone. [Runar Bjarnason](https://plus.google.com/106416252443767384318/posts) gave [a talk](http://www.infoq.com/presentations/io-functional-side-effects) in New York last week on I/O in FP. The talk itself, while great, is not what caught my interest.

{{< figure align="alignnone" width=357 src="//gundersen.net/wp-content/uploads/2013/10/FunctionalSideEffects.jpg" alt=" Runar Bjarnason - I want one of those stickers!" caption=" Runar Bjarnason - I want one of those stickers!" >}}

Inadvertently, 8 minutes in, he brilliantly sums up the chasm between FP and the real world (for mundane, but very common, variants of the real world) in 32 seconds:

> You have some code that interfaces with the outside world, and then you have, like, what your code actually _does_.
>
> And those things are kind of intermingled.
>
> But in reality, your code is not about I/O. I/O is just the code that retrieves the input to your program, and then takes the output of your program and delivers it somewhere.
>
> And those parts are not really that interesting. Your code is not about that. Your code is about what happens in between.

I have spent years in the corporate world on a multitude of different projects, and quite often not a lot happens in between. While there is a "pure" core of business logic in there somewhere, it is dwarfed by the amount of I/O-related code. Look at a typical mobile app, how many lines of code should have been separated into pure modules?

For this reason, if we want to convey the usefulness of FP to the imperatives amongst us, we need to focus on elegant solutions to real world inputs and outputs. Most software needs to integrate with other software. We need to show how our I/O-libraries, often overlooked or thought of as mere "helpers", are superior in efficiency and leads to cleaner and more maintainable code. While a pure HashMap is a beautiful thing, let's focus more of our time on Haskell packages such as [postgresql-simple](http://hackage.haskell.org/package/postgresql-simple "postgresql-simple"), [amqp](http://hackage.haskell.org/package/amqp "amqp") (of which I am a minor contributor), [aeson](https://www.fpcomplete.com/school/text-manipulation/json "aeson") and the fact that there is no native MSSQL library, just ODBC. Let's focus on the plumbing, so to say. Great packages for integrating with the outside world is what will bring FP to the masses. When that prerequisite is there, only then are they ready for all the other stuff we have.

And please do not go on and on about the beauty of [lens](http://fvisser.nl/post/2013/okt/11/why-i-dont-like-the-lens-library.html), to the casual passer-by it seems of no other use than to demonstrate that basic functionality they take for granted is somehow missing. If you want to wow someone, instead demonstrate how one line of code adds [ekg](http://ocharles.org.uk/blog/posts/2012-12-11-24-day-of-hackage-ekg.html "ekg") to your long-running process.

We could be doing our day jobs in Haskell. But most of the FP literature (and a lot of the packages) is focused elsewhere. As it is, one has to excpect or anticipate doing som library work when starting a project in Haskell, I really hope we will get past that soon.
