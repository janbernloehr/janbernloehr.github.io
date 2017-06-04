---
layout: post
title : "How to create a local copy of our wordpress site for testing"
date : 25.02.2016 23:17:00
tags: [wordpress]
---
{% include JB/setup %}

In this post I will briefly describe how to install wordpress on your windows machine and what to consider, when restoring the data of your live wordpress site to your local installation. This procedure can be useful to test wordpress (plugin/theme) updates locally before applying them to the life site.

Note: I assume that you are running wordpress against a MySql database and use a Windows machine for local testing.

## 0. Create a backup of your wordpress site

There are plenty of ways to create backups. The most convenient one is probably to use a plugin like [one of these](https://wordpress.org/plugins/tags/backup) which takes care of everything. If you have direct access to the server, you may also just create a copy of the wp-content folder and your wp-config.php. You also have to copy the mysql database - refer to this [Wordpress Wiki](https://codex.wordpress.org/WordPress_Backups) for detailed instructions.

## 1. Install Wordpress on your machine

*   Go ahead to the [Microsoft Web Platform Site for Wordpress](https://www.microsoft.com/web/wordpress) and hit the button **Install Wordpress**
*   The site will prompt you to download a tool called **Web Platfrom Installer**, or WebPI for short, which will take care of all the technical things like installing IIS, installing [PHP](http://windows.php.net/download/), installing [MySql](https://dev.mysql.com/downloads/windows/), installing [wordpress](https://wordpress.org/download/), and setting everything up.
*   You can find step by step instructions for the installation with WebPI [here](https://codex.wordpress.org/Installing_on_Microsoft_IIS).
*   When your setup is complete, write down the precise url under which your local wordpress installation is available (e.g. [http://localhost/wordpress/](http://localhost/wordpress/))

## 2. Restore your live site to your local machine

*   Restore the mysql database copied from your live site to your local mysql server. My recommendation is to leave the wordpress database created by WebPI as is and to restore your backup to a database with a different name, say *wordpress-restored*.
*   Overwrite the contents of your local *wp-content* folder with the ones of your live site.
*   Edit the local *wp-config.php* from your live site and enter the login data of your local mysql server and point it to your restored mysql database *wordpress-restored*.

```
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress-restored');

/** MySQL database username */
define('DB_USER', 'wp-restored-user');

/** MySQL database password */
define('DB_PASSWORD', 'wp-restored-pw');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');
```

*   Find the section of your live site's *wp-config.php* containing the authorization keys and salts and insert the ones of your live site.

```
define('AUTH_KEY',         'put your unique phrase here');
define('SECURE_AUTH_KEY',  'put your unique phrase here');
define('LOGGED_IN_KEY',    'put your unique phrase here');
define('NONCE_KEY',        'put your unique phrase here');
define('AUTH_SALT',        'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT',   'put your unique phrase here');
define('NONCE_SALT',       'put your unique phrase here');
```

*   Open the *wp-login.php* and find this line:

```
require( dirname(__FILE__) . '/wp-load.php' );
```

insert the following lines below

```
//FIXME: do comment/remove these hack lines. (once the database is updated)
update_option('siteurl', 'http://localhost/wordpress' );
update_option('home', 'http://localhost/wordpress' );
```

*   If you installed a newer wordpress version locally than the one running on your live site, head to the admin portal to initiate a database update.
*   That's it!
