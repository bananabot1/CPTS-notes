**Overview:**
- MySQL is an open source RDBMS that uses SQL to create and manage databases. As a relational database, MySQL stores data in tables of rows and columns organized into schemas.
- SQL, which stands for Structured Query Language is a programming language that’s used to retrieve, update, delete, and otherwise manipulate data in relational databases.


`MySQL` is an open-source SQL relational database management system developed and supported by Oracle. A database is simply a structured collection of data organized for easy use and retrieval. The database system can quickly process large amounts of data with high performance. Within the database, data storage is done in a manner to take up as little space as possible. The database is controlled using the [SQL database language](https://www.w3schools.com/sql/sql_intro.asp). MySQL works according to the `client-server principle` and consists of a MySQL server and one or more MySQL clients. The MySQL server is the actual database management system. It takes care of data storage and distribution. The data is stored in tables with different columns, rows, and data types. These databases are often stored in a single file with the file extension `.sql`, for example, like `wordpress.sql

The MySQL clients can retrieve and edit the data using structured queries to the database engine. Inserting, deleting, modifying, and retrieving data, is done using the SQL database language. Therefore, MySQL is suitable for managing many different databases to which clients can send multiple queries simultaneously. Depending on the use of the database, access is possible via an internal network or the public Internet.

MySQL is ideally suited for applications such as `dynamic websites`, where efficient syntax and high response speed are essential. It is often combined with a Linux OS, PHP, and an Apache web server and is also known in this combination as [LAMP](https://en.wikipedia.org/wiki/LAMP_\(software_bundle\)) (Linux, Apache, MySQL, PHP), or when using Nginx, as [LEMP](https://lemp.io/).

A MySQL database translates the commands internally into executable code and performs the requested actions. The web application informs the user if an error occurs during processing, which various `SQL injections` can provoke. Often, these error descriptions contain important information and confirm, among other things, that the web application interacts with the database in a different way than the developers intended.

---

## Dangerous settings

|   |   |
|---|---|
|`user`|Sets which user the MySQL service will run as.|
|`password`|Sets the password for the MySQL user.|
|`admin_address`|The IP address on which to listen for TCP/IP connections on the administrative network interface.|
|`debug`|This variable indicates the current debugging settings|
|`sql_warnings`|This variable controls whether single-row INSERT statements produce an information string if warnings occur.|
|`secure_file_priv`|This variable is used to limit the effect of data import and export operations.|

##  MySQL Enumeration

```
sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*
```



## Connection

```
mysql -u <user> -h <host> -P <port> -p
```

Connects to MySQL database. `-p` prompts for password.

---
## Commands Reference

> [!info]- MySQL Commands
> 
> |Command|Description|
> |---|---|
> |`SHOW DATABASES;`|Lists all available databases|
> |`USE <database>;`|Switches to a specific database|
> |`SHOW TABLES;`|Lists all tables in the current database|
> |`DESCRIBE <table>;`|Shows table structure (columns, data types)|
> |`SELECT * FROM <table>;`|Retrieves all columns and rows from a table|
> |`SELECT <column1>, <column2> FROM <table>;`|Retrieves specific columns|
> |`SELECT * FROM <table> WHERE <condition>;`|Retrieves rows matching a condition|
> |`SELECT * FROM <table> WHERE <column> LIKE '<pattern>%';`|Pattern matching (% is wildcard)|
> |`SELECT * FROM <table> ORDER BY <column> DESC;`|Sorts results (DESC/ASC)|
> |`SELECT * FROM <table> LIMIT <n>;`|Returns only first n results|
> |`INSERT INTO <table> VALUES (<value1>, <value2>, ...);`|Inserts a new row|
> |`UPDATE <table> SET <column>=<new-value> WHERE <condition>;`|Updates existing rows|
> |`DROP TABLE <table>;`|Deletes a table permanently|