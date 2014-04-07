--- 
layout: post
title: Install Xdebug 2.1.0 on OSX 10.6
comments: true
categories: [dev, php, osx, xdebug]
tags: 
- Mac
- PHP
status: publish
type: post
published: true
meta: 
  _edit_last: "1"
  _edit_lock: "1307375251"
  _syntaxhighlighter_encoded: "1"
  _aioseop_title: Install Xdebug on OSX - wrong architecture PECL workaround
  _aioseop_description: A handy little workaround for when the PECL install on OSX does not work and generates an extension that has the wrong architecture (no suitable image found)
  _aioseop_keywords: xdebug, osx, wrong architecture, pecl, php, debugging
  _wp_old_slug: ""
---
I have had to install Xdebug on a few systems now and I always find myself spending time going down the wrong, or should I say the harder, path. Which is to try to install via PECL as recommended. For some reason the PECL install on OSX does not work as designed. The built extension ends up being of the wrong architecture. My guess is that it is built as a 32 bit extension instead of 64 bit.
<!--more-->
Anyway, if Xdebug will not load after you installed on OSX using PECL and you find something like this in your apache error log:

``` bash
Failed loading /Applications/XAMPP/xamppfiles/lib/php/php-
5.3.1/extensions/no-debug-non-zts-20090626/xdebug.so:  
dlopen(/Applications/XAMPP/xamppfiles/lib/php/php-5.3.1/extensions/no-
debug-non-zts-20090626/xdebug.so, 9): no suitable image found.  Did 
find: /Applications/XAMPP/xamppfiles/lib/php/php-5.3.1/extensions/no-
debug-non-zts-20090626/xdebug.so: mach-o, but wrong architecture
```

Go to <a href="http://code.activestate.com/komodo/remotedebugging/">http://code.activestate.com/komodo/remotedebugging/</a> and download the binary for your version of PHP and put it in the extension directory. Restart apache and you should be good to go.

If anyone knows how to coax PECL into building the correct version of the Xdebug extension on OSX please drop me a comment.
