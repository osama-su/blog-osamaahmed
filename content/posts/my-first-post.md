---
title: "How to Install and Configure LAMP stack on Arch Linux"
date: 2022-05-22T17:16:25+02:00
draft: false
---

# How to Install and Configure LAMP stack on Arch Linux

## LAMP => Linux — Apache —MySQL— PHP

Most of Web Applications runs on top of a stack consists of: an operating system (Linux) a web server, like Apache HTTP Server or nginx. a relational DBMS, like MySQL or PostgreSQL. a programming language, like PHP, Python or Perl

An example of such a stack is LAMP witch stand for four open source software: Linux, Apache, MySQL and PHP.

The article provides a step-by-step guide on how to install and configure the LAMP stack on Arch Linux.

# Before You Begin

Before getting started with the installation process,check and update the Arch package database.

    sudo pacman -Syu

# Apache :

install Apache with the following command.

    sudo pacman -S apache

![](https://miro.medium.com/max/1340/1*dI1dpx5tier0Sshx0OLzqA.png)

open the main configuration file `/etc/httpd/conf/**httpd.conf**`. with your favorit editor and comment out the line

`LoadModule unique_id_module modules/mod_unique_id.so`

Set Apache to start at boot:

    sudo systemctl enable httpd.service

start and check server process daemon.

    sudo systemctl start httpd.service

    sudo systemctl start httpd.service

![](https://miro.medium.com/max/1364/1*ajiDIe5h--8iBTCp_vpKBQ.png)

To check whether the apache server is running, open the browser and type localhost. The browser will display the following page:

![](https://miro.medium.com/max/1400/1*2BQl--fMetVuSMTa3OCTHg.png)

# MySQL Database (MariaDB)

Install MySQL as follows:

    sudo pacman -S mysql

choose mariadb repository.

![](https://miro.medium.com/max/1346/1*3qEgmJ3gPWnZsJJHo61Owg.png)

Install the MariaDB data directory:

    sudo mysql_install_db --user=mysql --basedir=/usr --datadir=/var/lib/mysql

![](https://miro.medium.com/max/1400/1*FY4eQnHZObDmnCyxdssPXg.png)

Start MariaDB and set it to run at boot:

    sudo systemctl start mysqld.service
    sudo systemctl enable mysqld.service

Run `mysql_secure_installation`, a program that helps secure MySQL and MariaDB. `mysql_secure_installation` gives you the option to set your root password, disable root logins from outside localhost, remove anonymous user accounts, remove the test database and then reload the privilege tables:

    sudo mysql_secure_installation

follow the process of the installation and if you get stuck enter Y at evrey option.

Verify MySQL database connectivity by running the following command then leave database shell with **quit** or **exit** statement.

    mysql -u root -p

`-u <user>` specifies the user, and `-p` will prompt you for the password.

![](https://miro.medium.com/max/1400/1*rfj0K5pjF3CdgCZb4wdW2Q.png)

# PHP

Install PHP:

    sudo pacman -Syu php php-apache

Enable the PHP module in the `/etc/httpd/conf/httpd.conf` file by adding the following lines to the end of the file.

    LoadModule php_module modules/libphp.so
    AddHandler php-script php
    Include conf/extra/php_module.conf

![](https://miro.medium.com/max/1400/1*BUtM0QXQzB7Jvy6Sd-H3Tw.png)

In the same file `/etc/httpd/conf/httpd.conf`, comment the line:

    #LoadModule mpm_event_module modules/mod_mpm_event.so

and uncomment the line:

    LoadModule mpm_prefork_module modules/mod_mpm_prefork.so

![](https://miro.medium.com/max/1400/1*ObEnT-hzbJMhzOo40WezYw.png)

Restart the Apache:

    sudo systemctl restart httpd.service

Test if PHP is working correctly, by creating a `index.php` file in `/srv/http` and add phpinfo():

    sudo echo "<?php \n phpinfo(); \n ?>" >> /srv/http/index.php

![](https://miro.medium.com/max/1216/1*6OIVVYGakftFemX2VM0piA.png)

hit back to the browser and type localhost to verify php setting.

![](https://miro.medium.com/max/1400/1*1Z7dwDJ-rIEtSBFd2JKGhw.png)

That’s it! If everything looks like image above, you now have PHP dynamic server-side scripting language enabled on Apache

If you want to verify Apache syntax configurations and see a list of loaded modules without restarting httpd daemon run the following commands.

    sudo apachectl configtest
    sudo apachectl -M

# Install and Configuring PhpMyAdmin

If you don’t master MySQL command line and want a simple remote access to MySQL database provided through web interface then you need PhpMyAdmin package installed on your Arch.

    sudo pacman -S phpmyadmin php-mcrypt

Open the file `/etc/php/php.ini` and uncomment the lines `extension=mysqli`, `extension=pdo_mysql` and `extension=iconv` by removing the semicolon `(;)`

![](https://miro.medium.com/max/1400/1*W9K_tik-mMCJhbHqvojxVw.png)

Now set up Apache to work with phpmyadmin by creating phpmyadmin’s main configuration file.

    sudo vim /etc/httpd/conf/extra/phpmyadmin.conf

![](https://miro.medium.com/max/1224/1*pUJTDF-jN9yFREc0OEM3fQ.png)

Next, include the path to this configuration file on Apache’s main configuration file. `/etc/httpd/conf/httpd.conf`

![](https://miro.medium.com/max/840/1*Fhv_vmCuWgJdTdqGdfjwfw.png)

restart the Apache web service.

    sudo systemctl restart httpd

Now access the PhpMyAdmin from the browser.

http://localhost/phpmyadmin

![](https://miro.medium.com/max/1400/1*Ee8JT1jeYP_LU5w_4JsvfQ.png)

![](https://miro.medium.com/max/1400/1*0-yakVSmJ9sTMk9TSfoEdA.png)

With this article guide, you can comfortably start testing the behavior of your web app in a production-like environment.
