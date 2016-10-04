---
layout: post
title: Install Mysql on Windows
---

<h2>1. Unzip the uninstall file</h2>
  In this step the data folder is not exist for mysql-5.7.11-64.

<h2>2. Copy the ini file and rename it to my.ini</h2>
  In this step we will create the config file

<h2>3. Start mysql</h2>
  We can use the commend of "mysqld --console" to start the mysql and check the error.
  another command is "mysqld --defaults-file="**/my.ini" --init-file=D:\Tool\006.mysql\mysql-init.txt --console"
<h3>Note. get the error of "The innodb_system data file 'ibdata1' must be writable"</h3>
  1. End the process of mysqld in Windows Task Manager
  2. Delete the file of ib_logfile0 and ib_logfile1

<h2>4. Connect mysql</h2>
  The command is "mysql -u root -p", then it will remind you to enter the password, just enter.
