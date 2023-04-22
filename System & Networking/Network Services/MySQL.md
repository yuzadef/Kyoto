# Mysql

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Connecting to the server](#connecting-to-the-server)
  - [Useful commands](#useful-commands)
- [Methodologies](#methodologies)
  - [Get Mysql version](#get-mysql-version)
  - [Dump tables & columns](#dump-tables---columns)
  - [Extract credentials](#extract-credentials)
  - [Enumerate server with Nmap script](#enumerate-server-with-nmap-script)

## Theory
MySQL is a database management system. It may be anything from a simple shopping list to a picture gallery or the vast amounts of information in a corporate network. To add, access, and process data stored in a computer database, you need a database management system such as MySQL Server.

## Basic usage
### Connecting to the server
`mysql -h 10.0.0.1 -u admin -p`

### Useful commands
| Commands | Description |
| -------- | ----------- |
| show databases; | List all databases |
| use db-name; | Use a database |
| show tables; | List tables in database |
| describe tb-name; | See the rows and columns of the table |
| select * from tb-name; | Retrieve data from a table |
| update tb-name set col-name = new-value; | Update a column in a table with a new value |

## Methodologies
### Get Mysql version
`use auxiliary/admin/mysql/mysql_sql`

### Dump tables & columns
`use auxiliary/admin/mysql/mysql_sql`

### Extract credentials
`use auxiliary/scanner/mysql/mysql_hashdump`

### Enumerate server with Nmap script
`nmap -p3306 --script mysql-enum 10.0.0.1`
