# POP3

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Connecting to the server](#connecting-to-the-server)
  - [Useful commands](#useful-commands)
- [Methodologies](#methodologies)
  - [Enumerate usernames](#enumerate-usernames)
  - [Enumerate passwords](#enumerate-passwords)

## Theory
POP3 is a commonly used protocol for receiving email over the internet. It often works together with SMTP for the sending and receiving emails activities.

## Basic usage
### Connecting to the server
```
nc 10.0.0.1 110
> USER username
> PASS password
```

### Useful commands
| Commands | Description |
| -------- | ----------- |
| stat | Returns total number of messages and total size |
| list | List all messages |
| retr message-name | Retrieves a message |
| dele message-name | Delete a message |
| top message-name | Returns the headers and number of lines from the message. |
| rset | Undelete a message |
| quit | Logs out and save changes |

## Methodologies
### Enumerate usernames
```
use auxiliary/scanner/pop3/pop3_login
set RHOSTS 10.0.0.1
set RPORT 110
set USER_FILE username.txt
set PASSWORD password
set STOP_ON_SUCCESS true
```

### Enumerate passwords
```
use auxiliary/scanner/pop3/pop3_login
set RHOSTS 10.0.0.1
set RPORT 110
set USERNAME username
set PASS_FILE password.txt
set STOP_ON_SUCCESS true
```
