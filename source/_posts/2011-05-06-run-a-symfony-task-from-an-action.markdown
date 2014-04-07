---
layout: post
title: Run a symfony Task from an Action
comments: true
categories: [php, symfony, dev]
tags:
- sfAction
- sfTask
status: publish
type: post
published: true
meta: 
  _edit_last: "1"
  _edit_lock: "1306863909"
  _syntaxhighlighter_encoded: "1"
  _aioseop_title: Run a symfony task from an action
  _aioseop_description: Run a symfony task from an action to take advantage of either built-in symfony task or your own custom tasks. Tasks are not bound to the command line.
  _aioseop_keywords: symfony, task, action, framework, php
---
It is sometimes useful and necessary to run a symfony task from within an action. I have found that it is a good practice to keep the amount of code in a task to a minimum by putting all your logic into a class and then simply instantiating an object of that class in a task and then calling a method to which you pass the task arguments and options to get your work done.
<!--more-->
So instead of 
``` php
<?php

//myTask.class.php
protected function execute($arguments = array(), $options = array())
{
  // initialize the database connection
  $databaseManager = new sfDatabaseManager($this->configuration);
  $connection = $databaseManager->getDatabase($options['connection'])->getConnection();

  // all code to perform task goes here
}
```

do this
``` php
<?php

//myTask.class.php
protected function execute($arguments = array(), $options = array())
{
  // initialize the database connection
  $databaseManager = new sfDatabaseManager($this->configuration);
  $connection = $databaseManager->getDatabase($options['connection'])->getConnection();

  $myObj = new myClass();
  $myObj->someMethod($arguments['arg'], $options['opt'], ...);
}
```

If you have a situation as just explained then it makes more sense to simply do the same in your action - instantiate an instance of the class - instead of executing a task in the action.

However, I have run into situation where the task I want to run is actually executing in a different context (different symfony application within the same project). Then you really have to run the task from your action.
There are a few things to consider when doing this and I have not found any good information on this. I do remember reading about this subject matter in the symfony docs, but I was not able to find it when I needed it.
There are two things that need to happen to make this work:

* Before instantiating the task you wish to execute you must change the current working directory to the project root.
* After the task has completed you have to switch back to your current context otherwise bad things will happen.

``` php
<?php

// in one of your methods in actions.class.php
:
// Remember the current dir and change it to sf root
$currentDir = getcwd();
chdir(sfConfig::get('sf_root_dir'));

// Instantiate the task and run it
$task = new myTask($this->dispatcher, new sfFormatter());
$rc = $task->run(array('arg1' => $argVal1), array('opt1' => $optVal1));

// Restore original environment, change back to original directory
// Switch the context back to where it was
chdir($currentDir);
sfContext::switchTo('AppName');
:
```

In the above code sample *AppName* is the name of the context/sf application in which the action lives.
So if for example your frontend application is called *frontend* and the task you are running needs to have *--application=backend*, then you need to switch it back to *frontend* once the task is done otherwise your application will most likely encounter a fatal error such as 

>The template "myTemplate.php" does not exist or is unreadable in ""

