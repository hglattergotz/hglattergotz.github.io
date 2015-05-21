---
layout: post
title: "Connecting to MSSQL from Symfony2 on Linux"
date: 2015-05-21 23:01:56 +0200
comments: true
categories: [ symfony, mssql, php, linux]
---
If you find yourself trying to connect to a MSSQL database in your Symfony 2 project that is running on Linux you will soon discover that it does not work.
Even if you have figured out how to connect to MSSQL with PHP itself as outlined [here](http://www.glatter-gotz.com/blog/2011/06/06/connecting-to-microsoft-sql-server-from-php-on-osx-and-xampp/).
<!--more-->

Doctrine 2 currently supports drivers called *sqlsrv*, which is Windows only, and *pdo_sqlsrv*.
The later creates a dsn in its driver class (Doctrine\DBAL\Driver\PDOSqlsrv\Driver) that looks like this:

```php
$dsn = sqlsrv:server=';
```

When it should actually be this:

```php
$dsn = 'dblib:host=';
```

So what we need is a custom driver class that creates the correct connection instance. Fortunately this already exists and can be installed via composer:

```bash
$ composer require leaseweb/doctrine-pdo-dblib
```

The docs for this bundle state that you need to install FreeTDS, configure a connection and use that connection name as the value for the ```host``` key in your ```config.yml```.
I have not found that to be necessary on Linux. And on OSX if you follow [this](http://www.glatter-gotz.com/blog/2011/06/06/connecting-to-microsoft-sql-server-from-php-on-osx-and-xampp/) I did not see the need for it either.

In your ```config.yml```

```yml
# Doctrine Configuration
doctrine:
    dbal:
        default_connection:   default
        connections:
            default:
                #driver:       "%default_database_driver%"
                driver_class: Lsw\DoctrinePdoDblib\Doctrine\DBAL\Driver\PDODblib\Driver
                host:         "%default_database_host%"
                port:         "%default_database_port%"
                dbname:       "%default_database_name%"
                user:         "%default_database_user%"
                password:     "%default_database_password%"
                charset:      UTF8
```

Notice that the ```driver``` parameter is commented out and a new parameter called ```driver_class``` is added, which tells doctrine to use the custom driver class that we installed.
