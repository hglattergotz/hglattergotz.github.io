---
layout: post
title: "Secure File Transfers in WordPress with SFTP/SSH: Installing SSH2 for PHP"
comments: true
categories: [openssl, php, security, wordpress, ssh, sftp, dev]
status: publish
type: post
published: true
meta: 
  _edit_last: "1"
  _edit_lock: "1321461216"
  _syntaxhighlighter_encoded: "1"
  _wp_old_slug: ""
  _topsy_cache_timestamp: "1290467143"
  _aioseop_description: A tutorial that outlines how to install SSH2 for PHP and ultimately enable SFTP/SSH in WordPress in order to securely transfer data to your blog.
  _aioseop_title: "Secure WordPress with SFTP/SSH: Installing SSH2 for PHP"
  _aioseop_keywords: php,ssh,openssl,wordpress,security,encryption,pecl,ftp,sftp
---
>#### Update, 2011-11-16: Added some troubleshooting info

There are many reasons why you would want to install ssh2 for PHP on your server, mine was to allow me to use SFTP on a WordPress installation because my web host does not allow the use of FTP. Even if your host allows FTP I strongly urge you to use SFTP instead, since it uses SSH to encrypt all your data transfers. FTP sends your username and password as plain text!
<!--more-->
###Requirements for this tutorial
* PHP 5.2.x (I have used this on 5.2.9)
* Apache
* LINUX (probably works on Windows too, but no details about that are described here, sorry)
* WordPress 3.0.1 (somewhat older versions should work too)

A general overview of ssh2 for PHP can be found at <a title="PHP: Hypertext Processor" href="http://www.php.net/manual/en/book.ssh2.php">php.net</a>. If you look at the requirements section on <a title="PHP: Hypertext Processor" href="http://www.php.net/manual/en/ssh2.requirements.php">php.net</a> you will see that two other libraries are required for this to work. They are <a title="OpenSSL: Cryptography and SSL\TLS Toolkit" href="http://www.openssl.org/" target="_self">OpenSSL</a> and <a title="ibssh2 is a client-side C library implementing the SSH2 protocol" href="http://www.libssh2.org/" target="_self">libssh2</a>. I explain here how to install both of these as well.
###Installing OpenSSL
First log in as root on your server on the command line and find out if OpenSSL is already installed by running the following command:

``` bash
[root@host ~]$ rpm -qa | grep openssl
```

If you see something similar to the two lines below (might differ in version numbers) then OpenSSL is installed and you can skip to the next section.

``` bash
openssl-0.9.7a-43.17.2
openssl-devel-0.9.7a-43.17.2
```

If it is not installed, then you can do so by running yum or apt-get (depending on your system) as follows:

#### RedHat based systems

``` bash
[root@host ~]$ yum install openssl
```

#### Debian based systems

``` bash
[root@host ~]$ apt-get install openssl
```

###Installing libssh2
You can verify if it is already on your system by searching for libssh2.so like this

``` bash
[root@host ~]$ find / -name libssh2.so -print
```

If it is not installed then you will have to install it from source.

Create a temporary directory to download the source files to and then cd to the newly created temporary directory

``` bash
[root@server ~]$ wget http://sourceforge.net/projects/libssh2/files/old-libssh2-releases/1.1/libssh2-1.1.tar.gz
[root@server ~]$ tar -zxvf libssh2-1.1.tar.gz
[root@server ~]$ cd libssh2-1.1
[root@server ~]$ ./configure
[root@server ~]$ make
[root@server ~]$ make install
```

Once these steps have been successfully completed delete the previously created directory.

### Installing the ssh2 module for php

This is available via PECL and can be installed as follows

``` bash
[root@server ~]$ pecl install channel://pecl.php.net/ssh2-0.11.3
```

This should have placed the ssh2.so file in your php extensions directory. Now add

``` bash
extension=ssh2.so
```

to your php.ini or whatever file your extensions are specified in. Depending on your installation this might be in a separate file instead of your php.ini. Consult you php installation documentation for more information. The way this is done varies depending on your distribution.
Also worth mentioning is that you will need the zlib.so extension enabled as well. Make sure this is the case while you are editing the extension ini file and then restart apache.

#### Ubuntu/Debian

``` bash
[root@server ~]$ /etc/init.d/apache2 restart
```

#### Red Hat, CentOS, Fedora

``` bash
[root@server ~]$ /etc/init.d/httpd restart
```

Now make sure that ssh2.so is really loaded by executing the following command on the command line:

``` bash
[root@server ~]$ php -i | grep ssh2
```

You should see something like

``` bash
libssh2 version => 1.1
```

If you do not, then the ssh2.so file might have been placed in the wrong directory or you did not correctly configure your ini file to load it.

### Generating the ssh keys

Wordpress needs to know the location of your ssh public and private keys. This is how you generate them:
On the command line on your server login as the user that has access to the site. In my case I have the site installed as a virtual host and have a specific user that owns it.

``` bash
[user@server ~]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa):
Created directory '/home/user/.ssh'.
Enter passphrase (empty for no passphrase): (enter a passphrase and make note of it)
Enter same passphrase again: (enter the same passphrase)
Your identification has been saved in /home/bsf/.ssh/id_rsa.
Your public key has been saved in /home/bsf/.ssh/id_rsa.pub.
The key fingerprint is:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:XX:X:X:XX:XX user@server.something.com
The key's randomart image is:
+--[ RSA 2048]----+
(info will be displayed here)
+-----------------+
```

Create an "authorized_keys" file by copying the public key in place

``` bash
[user@server ~]$ cd .ssh
[user@server .ssh]$ cp id_rsa.pub authorized_keys
[user@server .ssh]$ cd ..
[user@server ~]$ chmod 755 .ssh
[user@server ~]$ chmod 644 .ssh/*
```

Now, when you log into the WordPress admin dashboard and perform an action that requires a file transfer operation, such as installing or updating a plugin, you should be presented with more options for the connection type. Aside from FTP, which was there before, you should also have SSH2.
Select SSH2 and it will add a few fields to the form and it should look something like the screenshot below.

#### Important:

The FTP/SSH Username here is the user name for SSH for which you created the ssh keys earlier, NOT your ftp user. And the password is the passphrase that you created during the key generation step earlier, not the users password.

{% img left /images/wordpress_connection_settings.png 602 452 'WordPress Connection Settings with SSH2 Enabled' 'WordPress Connection Settings when SSH2 is installed' %}

To avoid having to enter this information every time a file transfer is executed you can define a few constants in your WordPress configuration file wp-config.php:

``` php
define('FTP_PUBKEY','/home/user/.ssh/id_rsa.pub');
define('FTP_PRIKEY','/home/user/.ssh/id_rsa');
define('FTP_USER','user');
define('FTP_PASS','yourpassphrase');
define('FTP_HOST','server.something.com');
```

**Important:** Be sure to move your wp-config.php file out of the doc root for added security.

### Troubleshooting

##### Pecl installation of ssh2 fails

**Error**
``` bash
Cannot find autoconf. Please check your autoconf installation and the
$PHP_AUTOCONF environment variable. Then, rerun this script.
```

##### Solution

Check if autoconf is on your system but not in the path:

``` bash
which autoconf
```

If it is there then

``` bash
export path/to/autoconf
```

**Error**

Pecl does not have write permissions to the tmp directories and fails with

``` bash
/usr/local/bin/phpize: /tmp/pear/temp/...: /bin/sh: bad interpreter: Permission denied
```

##### Solution
Temporarily change permissions on the following folders.

``` bash
mount -o remount,exec,suid /tmp
mount -o remount,exec,suid /var/tmp
```

Run the install again and then change the permissions back

``` bash
mount -o remount,noexec,suid /tmp
mount -o remount,noexec,suid /var/tmp
```

#### For getting this to work the first time for myself I referred to the following two posts:
<a href="http://hostechs.com/2008/07/installing-ssh2-for-php-shell-connections-how-to/">http://hostechs.com/2008/07/installing-ssh2-for-php-shell-connections-how-to/</a>
<a href="http://www.firesidemedia.net/dev/wordpress-install-upgrade-ssh/">http://www.firesidemedia.net/dev/wordpress-install-upgrade-ssh/</a>
