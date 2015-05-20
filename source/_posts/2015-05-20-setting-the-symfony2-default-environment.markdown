---
layout: post
title: "Setting the Symfony2 default environment"
date: 2015-05-20 12:13:22 +0200
comments: true
categories: [ php, symfony2, cli ]
---
In my quest to type less because I am getting lazy I am trying to automate as much as I can.
Something that is a bit tiresome in Symfony2 is to have to specify the ```--env``` flag for each run on the command-line if you do not wish to run in the default built-in ```dev``` environment.
<!--more-->

Turns out there is an extremely simple way to override this:

In your ```~/.profile``` (or ```~/.bash_profile```) simply add

```bash
export SYMFONY_ENV=prod
```

Any console command in your Symfony2 project will now run with --env=prod if you omit the flag.

Another useful thing to put in your ```~/.profile``` is an alias for the app/console portion of your console command:

```bash
# Symfony2 aliases
alias sf='app/console'
```

Now, instead of having to type

```bash
$ app/console namespace:command
```

you can simply type

```bash
$ sf namespace:command
```
