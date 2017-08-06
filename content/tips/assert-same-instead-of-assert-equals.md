---
title: "Assert Same Instead of Assert Equals"
date: 2017-08-14T01:14:53+01:00
draft: true
categories: ["unit testing"]
tags: ["phpunit", "php"]
---

Quick and simple one this one - I had [a tweet](https://twitter.com/Brunty/status/850322699842973696) that was kinda popular a few months ago:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Remember: in PHPUnit use assertSame instead of assertEquals where possible:<br><br>assertEquals(5, true); // pass<br>assertSame(5, true); // fail</p>&mdash; Matt Brunt 🐘 (@Brunty) <a href="https://twitter.com/Brunty/status/850322699842973696">April 7, 2017</a></blockquote> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

This is because `assertEquals` in [PHPUnit](http://phpunit.de) checks if they are equal after type juggling using `==` and `assertSame` checks they are identicle with the same value and type using `===`.


