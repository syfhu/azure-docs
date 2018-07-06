---
title: MySQL drivers and management tools compatibility
description: This article describes the MySQL drivers and management tools that are compatible with Azure Database for MySQL. 
services: mysql
author: ajlam
ms.author: andrela
editor: jasonwhowell
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
---
# MySQL drivers and management tools compatible with Azure Database for MySQL
This article describes the drivers and management tools that are compatible with Azure Database for MySQL.

## MySQL Drivers
Azure Database for MySQL uses the world's most popular community edition of MySQL database. Therefore, it is compatible with a wide variety of programming languages and drivers. The goal is to support the three most recent versions MySQL drivers, and efforts with authors from the open source community to constantly improve the functionality and usability of MySQL drivers continue. A list of drivers that have been tested and found to be compatible with Azure Database for MySQL 5.6 and 5.7 is provided in the following table:

| **Driver** | **Links** | **Compatible Versions** | **Uncompatible Versions** | **Notes** |
| :-------- | :------------------------ | :----------- | :---------------------- | :--------------------------------------- |
| PHP | http://php.net/downloads.php | 5.5 5.6 7.x | 5.3 | For PHP 7.0 connection with SSL MySQLi, add MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT in the connection string. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO set: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` option to false.|
| .Net | [MySqlConnector on GitHub](https://github.com/mysql-net/MySqlConnector) <br> [Installation package from Nuget](https://www.nuget.org/packages/MySqlConnector/) | 0.27 and after | 0.26.5 and before | |
| Nodejs |  [MySQLjs on GitHub](https://github.com/mysqljs/mysql/releases) <br> Installation package from NPM:<br> Run `npm install mysql` from NPM | 2.15 | 2.14.1 and before | |
| GO | https://github.com/go-sql-driver/mysql/releases | 1.3 | 1.2 and before | Use allowNativePasswords=true in the connection string |
| Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 and before | |
| Java | https://downloads.mariadb.org/connector-java/ | 2.1 2.0 1.6 | 1.5.5 and before | |

## Management Tools
The compatibility advantage extends to database management tools as well. Your existing tools should continue to work with Azure Database for MySQL, as long as the database manipulation operates within the confines of user permissions. Three common database management tools that have been tested and found to be compatible with Azure Database for MySQL 5.6 and 5.7 are listed in the following table:

|                                     | **MySQL Workbench 6.x and up** | **Navicat 12** | **PHPMyAdmin 4.x and up** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Create, Update, Read, Write, Delete | X | X | X |
| SSL Connection | X | X | X |
| SQL Query Auto Completion | X | X |  |
| Import and Export Data | X | X | X |
| Export to Multiple Formats | X | X | X |
| Backup and Restore |  | X |  |
| Display Server Parameters | X | X | X |
| Display Client Connections | X | X | X |
