---
layout: post
title: "express-generator on OSX/homebrew node install"
date: 2014-04-24 23:01:47 +0200
comments: true
categories: [ node, npm, express, osx, homebrew ]
---
The [express](http://expressjs.com/guide.html#executable) docs claim that you
can simply install the express-generator globally and use it to generate
application skeletons.
<!--more-->

Well, I followed the instructions to install express-generator

```bash
$ npm install -g express-generator
```

and got 

```bash
$ express
-bash: express: command not found
```

I am working on OSX and installed node via homebrew, which is what is causing
this issue, at least for express. Because other globally installed packages
worked fine.

The output of the installation gives you a hint. The executable is installed in
```bash
/usr/local/share/npm/bin/
```

Add that directory to your path and it will solve the problem.

```bash
export PATH=/usr/local/share/npm/bin:$PATH
```

