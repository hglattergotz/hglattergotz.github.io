---
layout: post
title: "Git merge specific files from one branch into another"
date: 2014-04-08 10:10:41 +0200
comments: true
categories: [git]
---
I have started to use [git branching](http://nvie.com/posts/a-successful-git-branching-model/)
more heavily in recent months and also make it a point to have small and well
documented commits. This sometimes leads to the need to merge specific files
from one branch into another (good or bad, it just happens). Git facilitates
this with ```checkout```:

```bash
$ git checkout source-branch-name/commit-sha path/tofile/of/interest
```

If you do not provide the commit sha it will simply checkout the file at the
HEAD of the source branch.
