.. author:: Frederik Sonnental <funke@frederik-sonnental.ch>
.. author:: Frederik Sonnental <https://frederik-sonnental.ch>

.. tag:: cms
.. tag:: web
.. tag:: php

.. sidebar:: Logo

.. image:: _static/images/craft-cms.svg
    :align: center


#####
Craft CMS
#####

.. tag_list::

`Craft-CMS<https://craftcms.com/>_` is a light and clean, though function-rich, PHP-based Content Management System (CMS).

Installation
=============

Which way
------------
Craft suggests multiple methods to install and deploy a project in it's `documentation<https://craftcms.com/docs/4.x/installation.html>`.

The DDEV-based installation will hardly work on uberspace, as it would need a running docker configuration.
This guide covers a manual way how to install Craft, adapted for all us using shared hosting on uberspace.

Initial Deployment: Step by step
------------

1) Create an empty database. 

If you are using mySQL & phpMyAdmin: 
Add a new DB in the `web-interface<https://mysql.uberspace.de/phpmyadmin>`. 
To know your credentials read :manual:`uberspaces mySQl manual<database-mysql>`.

2) Add your projects source-code
Assuming that you prepared your code (most likely) locally and that you use git to deploy and version-control:


Via SSH: Go with cd to the desired location on the file-system of your server:

.. code-block:: console

  [isabell@stardust ~/html/]$ git clone <project-clone-link>

You could also add the project via SFTP or use the deployment/file-transfer method of your choice.

3) Install Craft

.. code-block:: console

    [isabell@stardust ~/html]$ cd <your-project>
    [isabell@stardust ~/html/your-project]$ composer install
    [isabell@stardust ~/html/your-project]$ php craft setup/welcome

line three will create the .env file and install craft. 
Enter the correct information as guided in https://craftcms.com/knowledge-base/first-time-setup 

Run migrations and apply project config:

.. code-block:: console

    [isabell@stardust ~/html/your-project]$ php craft up --interactive=0

You might need or want to add configurations to the env file

4) Setup server-root via .htaccess

The craft routing engine expects every request to enter the /web folder.
Use this rule in the root-level-.htaccess-file to ensure requests are passed to the /web folder, 
where craft handle them

.. code-block:: console
    
    [isabell@stardust ~/html/your-project]$ nano .htaccess

.. code-block:: htaccess
RewriteEngine on
RewriteRule (.*) web/$1 [NC]

$1 is a variable and refers to the selected part (which is the whole url-slug)
Note that this rule will not double-append web/, in case web/ is already part of the url 

Maintenance & Updates
=============

Whenever an update is done please follow Crafts official instructions:
https://craftcms.com/docs/4.x/deployment.html#simple-git


Author's system information
=============
Operating System: CentOS Linux 7 (Core)
PHP 8.0.30
Composer version 2.6.5 2023-10-06 10:11:52
mysql  Ver 15.1 Distrib 10.6.16-MariaDB, for Linux (x86_64) using readline 5.1
