# Mongodb

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Connecting to the server](#connecting-to-the-server)
  - [Useful commands](#useful-commands)

## Theory
MongoDB is a document database used to build highly available and scalable internet applications. With its flexible schema approach, it's popular with development teams using agile methodologies.

## Basic usage
### Connecting to the server
`mongo 10.0.0.1 -u username -p`

### Useful commands
| Commands | Description |
| -------- | ----------- |
| show dbs | List all databases |
| use db-name | Use a database to access its resources |
| show collections | List all collections in the database |
| db.collection-name.find() | Retrieve a collection |
