# Postgresql

## Summary
- [Theory](#theory)
- [Methodologies](#methodologies)
  - [Enumerate login credentials](#enumerate-login-credentials)
  - [Dump user hash](#dump-user-hash)
  - [Execute commands on server](#execute-commands-on-server)
  - [Read files on server with valid credentials](#read-files-on-server-with-valid-credentials)
  - [Arbitrary command execution](#arbitrary-command-execution)


## Theory
# Postgresql

## Summary
- [Theory](#theory)
- [Methodologies](#methodologies)
  - [Enumerate login credentials](#enumerate-login-credentials)
  - [Dump user hash](#dump-user-hash)
  - [Execute commands on server](#execute-commands-on-server)
  - [Read files on server with valid credentials](#read-files-on-server-with-valid-credentials)
  - [Arbitrary command execution](#arbitrary-command-execution)


## Theory
PostgreSQL is a powerful, open source object-relational database system that uses and extends the SQL language combined with many features that safely store and scale the most complicated data workloads.

## Methodologies
### Enumerate login credentials
`use auxiliary/scanner/postgres/postgres_login`

### Dump user hash
`use auxiliary/scanner/postgres/postgres_hashdump`

### Execute commands on server
`use auxiliary/admin/postgres/postgres_sql`

### Read files on server with valid credentials
`use auxiliary/admin/postgres/postgres_readfile`

### Arbitrary command execution
`use exploit/multi/postgres/postgres_copy_from_program_cmd_exec`


## Methodologies
### Enumerate login credentials
`use auxiliary/scanner/postgres/postgres_login`

### Dump user hash
`use auxiliary/scanner/postgres/postgres_hashdump`

### Execute commands on server
`use auxiliary/admin/postgres/postgres_sql`

### Read files on server with valid credentials
`use auxiliary/admin/postgres/postgres_readfile`

### Arbitrary command execution
`use exploit/multi/postgres/postgres_copy_from_program_cmd_exec`
