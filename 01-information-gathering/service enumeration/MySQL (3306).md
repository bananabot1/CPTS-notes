**Overview:**
- MySQL is an open source RDBMS that uses SQL to create and manage databases. As a relational database, MySQL stores data in tables of rows and columns organized into schemas.
- SQL, which stands for Structured Query Language is a programming language that’s used to retrieve, update, delete, and otherwise manipulate data in relational databases.
---
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