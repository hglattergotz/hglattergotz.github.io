---
layout: post
title: "PHP Composer: Working with local repositories"
date: 2014-04-09 10:03:10 +0200
comments: true
categories: [ php, composer, git ]
---
When developing packages or Bundles for your Symfony2 project it is nice to be
able to manage them in their own repository and have Composer deal with installing
them in your test project. This is a bit slow and quite cumbersome if you are
using something like GitHub since you have to push changes to remote and then
Composer has to pull it all back down.
<!--more-->
To make this a bit more convenient it is possible to configure your project to
use a package from a local git repository:

In composer.json:
```json
    :
    "repositories": [
        {
            "type": "vcs",
            "url": "/path/to/the/local/git/repository"
        }
    ],
    "require": {
        :
        "username/package-name": "dev-master"
    },
    :
```
Here username/package-name is the value that is specified under the "name" key
in the composer.json of the package.

So the work flow would be as follows:

* Work on Bundle and commit to its repository locally
* In your Symfony2 project execute ```composer update username/package-name```

This also lets you use a git repo hosted on GitHub that is not registered on
packagist.org
```json
    :
    "repositories": [
        {
            "type": "vcs",
            "url": "http://github.com/username/reponame"
        }
    ],
    :
```
