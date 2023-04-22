# Simple Transfer Mail Protocol (SMTP)

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Connecting to the server](#connecting-to-the-server)
  - [Useful commands](#useful-commands)
- [Methodologies](#methodologies)
  - [Enumerate valid SMTP users](#enumerate-valid-smtp-users)
  - [Get SMTP version](#get-smtp-version)

## Theory
An SMTP server is a computer or an app that is responsible for sending emails.

## Basic usage
### Connecting to the server
`nc 10.0.0.1 25`

### Useful commands
| Commands | Description |
| -------- | ----------- |
| VRFY username | Verify if the user exists |
| HELO or EHLO | Initiate a conversation |
| STARTTLS | Start a TLS session for encryption |
| RCPT | Recipient address |
| DATA | Message's content |
| RSET | Abort current message |
| MAIL | Sender address |
| AUTH | Authenticate client to server |
| QUIT | Close connection |
| HELP | Help screen |

## Methodologies
### Enumerate valid SMTP users
`smtp-user-enum -M VRFY -U /path/to/wordlists.txt -t 10.0.0.1`

Using Metasploit-Framework
```
msfconsole
use auxiliary/scanner/smtp/smtp_enum
set RHOSTS 10.0.0.1
set RPORT 25
set USER_FILE usernames.txt
```

### Get SMTP version
```
msfconsole
use auxiliary/scanner/smtp/smtp_version
set RHOSTS 10.0.0.1
set RPORT 25
```
