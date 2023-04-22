# Server Message Block (SMB)

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Access shares](#access-shares)
  - [Useful commands](#useful-commands)
- [Methodologies](#methodologies)
  - [Scan for outdated version](#scan-for-outdated-version)
  - [Gather useful informations from the server](#gather-useful-informations-from-the-server)
  - [Listing shares](#listing-shares)

## Theory
SMB protocol is a network file sharing protocol that allows applications on a computer to read and write to files and to request services from server programs in a computer network.

## Basic usage
### Access shares
`smbclient //10.0.0.1/share_name -U username`

### Useful commands
| Command | Description |
| ------- | ----------- |
| ls | List files and directories |
| cd | Change directories |
| get | Download files |
| put | Upload files |
| mkdir | Create directory |
| rmdir | Remove directory |
| rm | Delete file |
| help | Get list of help commands |

## Methodologies
### Scan for outdated version
Scan SMB server for outdated version.

`nmap -p139,445 --script=smb-vuln* 10.0.0.1 --min-rate=1500`

### Gather useful informations from the server
`enum4linux -a 10.0.0.1`

### Listing shares
```
smbclient -L 10.0.0.1
crackmapexec smb 10.0.0.1 -u username -p password --shares
```
