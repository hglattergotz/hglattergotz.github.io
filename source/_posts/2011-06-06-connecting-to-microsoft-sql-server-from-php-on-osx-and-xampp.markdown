--- 
layout: post
title: Connecting to Microsoft SQL Server from PHP on OSX and XAMPP
comments: true
categories: [osx, mssql, php, xampp, dev]
tags: 
- Mac
- MSSQL
- OSX
- PHP
- XAMPP
status: publish
type: post
published: true
meta: 
  _edit_lock: "1332936196"
  _edit_last: "1"
  _syntaxhighlighter_encoded: "1"
  _aioseop_title: Connecting to Microsoft SQL Server from PHP on OSX and XAMPP
  _aioseop_description: Connecting to Microsoft SQL Server from PHP/OSX/XAMPP with FreeTDS driver. Turn on logging in the FreeTDS driver to find out what the problem is and fix it.
  _aioseop_keywords: mssql, microsoft sql server, osx, php, xampp, freetds, mssql_connect
---
>#### UPDATE (March 23rd 2012):
>I recently found a <a href="http://php-osx.liip.ch/">PHP binary </a>that has a one-line installer. It is actively maintained by Liip AG. I now use that in combination with <a href="http://mxcl.github.com/homebrew/">HomeBrew</a> for all the rest. The Liip package is compiled with pdo_dblib which makes the problem described in this post a non-issue.

I recently found myself in the position of having to connect to a remote Microsoft SQL Server from my OSX system using PHP. The production environment runs Ubuntu LINUX, where connecting via mssql_connect() was no problem, but I develop on OSX and I could not get this to work initially.
<!--more-->
mssql_connect() simply returns boolean FALSE on failure and PHP tells you little more than

``` bash
Warning: mssql_connect(): Unable to connect to server: YOURSERVERNAME
```

It took quite a few hours to finally figure out that I could turn on some additional logging on the driver level. I use XAMPP and its PHP distribution uses FreeTDS for mssql access.
There is a configuration file for this driver located at

``` bash
/Applications/XAMPP/xamppfiles/etc/freetds.conf
```


that lets you turn on debug logging to a file.

Uncomment the following lines

``` bash
;       dump file = /tmp/freetds.log
;       debug flags = 0xffff
```


And then run your connection attempt again. Take a look at the log for details on the failure.

In the end I added a new entry for an MS SQL server at the end of the freetds.conf file and used its name in the call to mssql_connect().

``` bash
[MyServerXYZ]
        host = the_host_name
        instance = the_instance_name
        port = 1433
        tds version = 9.0
```


In my case I had to deal with an instance name that would normally be appended to the host name in a connection string, but for this driver configuration you have to use a variable called <em>instance</em>.

In PHP I then use this code during development

``` bash
$conn = mssql_connect('MyServerXYZ', $username, $password);
```
