---
layout: post
title: "./symfony doctrine:build-schema for a Single Table"
date: 2012-07-16 06:52
comments: true
categories: [symfony, doctrine, dev, php]
tags:
published: true
---
The built-in command line tasks in symfony 1.4 (yes, I am still stuck on 1.4)
are very handy for quickly performing all sorts of project related tasks.
Doctrines own command line tasks are exposed through the symfony CLI in the
*doctrine* namespace.
I use the ```doctrine:build-schema``` task, which generates a yml schema for an
existing database, all the time. This allows one to very rapidly build models
from and for an existing database.
<!--more-->
But therein also lies a problem - at least sometimes. I am often working on
projects with multiple large databases that consist of dozens or hundreds of
tables. And most often I only need to have models for a very small subset of
the total list of tables.
I used to simply generate the yml schema with ```./symfony docrine:build-schema```
and then manually remove the tables I did not need. Horrible!! Slow, error
prone and mind numbing.

It would be great if there was a more granular version of this task.
For example
{% blockquote %}
Add the schema for table X on database Y to the existing schema file
{% endblockquote %}

Enter ```doctrine:build-table-schema```.
I wrapped the functionality in a class that is really nothing more than a slight
variation of part of the ```Doctrine_Import``` class. More specifically its
```importSchema``` method. But instead of building the schema for all tables of
the database it simply builds the ones that have been explicitly requested.
{% codeblock SchemaBuilder.class.php lang:php %}
<?php
/**
 * Build the schema for multiple connections and specific tables for those
 * connections.
 *
 * Example:
 *   $connections = array(
 *       'connection1' => array('table1', 'table2'),
 *       'connection2' => array('table1', 'table2')
 *   );
 *
 * @param type $directory
 * @param array $connections Array of connection names with their associated
 *                           tables
 * @param array $options
 * @return array
 */
protected function buildPHPModels($directory, array $connections = array(), array $options = array())
{
  $classes = array();

  $manager = Doctrine_Manager::getInstance();

  foreach ($manager as $name => $connection)
  {
    // Limit the databases to the ones specified by $connections.
    // Check only happens if array is not empty
    $connectionNames = array_keys($connections);

    if (!empty($connections) && !in_array($name, $connectionNames))
    {
      continue;
    }

    $builder = new Doctrine_Import_Builder();
    $builder->setTargetPath($directory);
    $builder->setOptions($options);

    $definitions = array();
    $currentConnName = $connection->getName();

    foreach ($connection->import->listTables() as $table)
    {
      if (!in_array($table, $connections[$currentConnName]))
      {
        continue;
      }

      $definition = array();
      $definition['tableName'] = $table;
      $definition['className'] = Doctrine_Inflector::classify(Doctrine_Inflector::tableize($table));
      $definition['columns'] = $connection->import->listTableColumns($table);
      $definition['connection'] = $connection->getName();
      $definition['connectionClassName'] = $definition['className'];

      try
      {
        $definition['relations'] = array();
        $relations = $connection->import->listTableRelations($table);
        $relClasses = array();
        foreach ($relations as $relation)
        {
          $table = $relation['table'];
          $class = Doctrine_Inflector::classify(Doctrine_Inflector::tableize($table));
          if (in_array($class, $relClasses))
          {
            $alias = $class . '_' . (count($relClasses) + 1);
          }
          else
          {
            $alias = $class;
          }
          $relClasses[] = $class;
          $definition['relations'][$alias] = array(
              'alias' => $alias,
              'class' => $class,
              'local' => $relation['local'],
              'foreign' => $relation['foreign']
          );
        }
      }
      catch (Exception $e)
      {

      }

      $definitions[strtolower($definition['className'])] = $definition;
      $classes[] = $definition['className'];
    }

    // Build opposite end of relationships
    foreach ($definitions as $definition)
    {
      $className = $definition['className'];
      $relClasses = array();
      foreach ($definition['relations'] as $alias => $relation)
      {
        if (in_array($relation['class'], $relClasses) || isset($definitions[$relation['class']]['relations'][$className]))
        {
          $alias = $className . '_' . (count($relClasses) + 1);
        }
        else
        {
          $alias = $className;
        }
        $relClasses[] = $relation['class'];
        $definitions[strtolower($relation['class'])]['relations'][$alias] = array(
            'type' => Doctrine_Relation::MANY,
            'alias' => $alias,
            'class' => $className,
            'local' => $relation['foreign'],
            'foreign' => $relation['local']
        );
      }
    }

    // Build records
    foreach ($definitions as $definition)
    {
      $builder->buildRecord($definition);
    }
  }

  return $classes;
}
{% endcodeblock %}

The entire code including a symfony task to call it is available as a symfony 1.4
plugin on github.com. It is part of a utilities plugin that is available at
[http://github.com/hglattergotz/uUtilitiesPlugin](http://github.com/hglattergotz/uUtilitiesPlugin).

The class can be found here ```uUtilitiesPlugin/lib/SchemaBuilder```

To add the schema for table *A* in database connection *Z* you would execute this from the
symfony project folder:
{% codeblock lang:bash %}
./symfony doctrine:build-schema-table --connection=Z --table=A --application=myapp
./symfony doctrine:build-model
{% endcodeblock %}

