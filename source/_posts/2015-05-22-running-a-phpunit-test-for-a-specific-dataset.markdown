---
layout: post
title: "Running a phpunit test for a specific dataset"
date: 2015-05-22 12:41:40 +0200
comments: true
categories: [ phpunit, php ]
---
When using data providers in your phpunit tests to run the same test for N different sets of input data it is
sometimes helpful if you can run the test for just a specific data set from the command line.
<!--more-->

Since I can never remember the syntax when I need it I decided to write it down (mostly for myself). Next up would be to create an alias or a helper script. We shall see.

So lets say I want to run the test method ```testTheThing``` with dataset #12.

```bash
$ phpunit -c app/ --filter '/::testTheThing .*#12$/'
```

Details:

```bash
-c app/  ....... The place where my phpunit config lives
--filter ....... pass in a filter that will limit the run to what you need
testTheThing ... The test method to call
12 ............. The number of the dataset to run
```

