# File Transfer Protocol (FTP)

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Connecting to the server](#connecting-to-the-server)
  - [Useful commands](#useful-commands)
- [Common attacks](#common-attacks)
  - [Brute-force credentials](#brute-force-credentials)
  - [FTP sniffing](#ftp-sniffing)

## Theory
The File Transfer Protocol (FTP) is a standard network protocol used for the transfer of files between a client and server. It usually runs on ports 21/tcp or 2121/tcp.

## Basic usage
### Connecting to the server
`ftp username@10.0.0.1 -p 21`
Some FTP servers are configured to let users connect anonymously without credentials.
`ftp anonymous@10.0.0.1 -p 21`

### Useful commands
| Commands | Description |
| -------- | ----------- |
| cd       | Change directory |
| ls       | List files |
| get      | Download files |
| put      | Upload files |
| help     | Display help informations |

***Hidden files can be listed with ls -la.***

## Common attacks
### Brute-force credentials
Using Metasploit-Framework module
```
msfconsole
use auxiliary/scanner/ftp/ftp_login
set RHOSTS 10.0.0.1
set RPORT 21
set USER_FILE /path/to/wordlists.txt
set PASS_FILE /path/to/wordlists.txt
```
Using Hydra
```
hydra -s 21 ssh://10.0.0.1 -l username -P passwords.txt
hydra -s 2121 ftp://10.0.0.1 -L usernames.txt -p password -t 64
```

### FTP sniffing
Requirements
- **FTP communications are not encrypted.**
- **Attacker is on the same network as client or server.**
- **Sniff data packet back & forth the client and server with Wireshark**
