# Services Discovery

## Summary
- [Theory](#theory)
- [Common techniques](#common-techniques)

## Theory
By conducting an enumeration scan, you can see what ports are open on devices, which ones have access to specific services and what type of information is being transmitted.

## Common techniques
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
