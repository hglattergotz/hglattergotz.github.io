---
layout: post
title: "How to run cron jobs with an offset"
date: 2015-02-21 10:07:04 +0100
comments: true
categories: [ cron, php ]
---
I have been using the ```*/n``` syntax for the minute field for some time now to run a cron task every "n" minutes starting at the top of the hour.

What I did not realize is that

    */5
    
is actually equivalent to

    0-59/5

which basically means: run every 5 minutes starting at the top of the hour (0).

So if you would like to run every 5 minutes starting at 2 minutes after the hour you would simply specify

    2-59/5

Other examples:

    5-59/15 run at 5, 20, 35, 50 minutes

    1-59/2 run every 2 minutes starting at 1 minute after the hour
