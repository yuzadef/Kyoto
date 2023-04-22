# Domain Name System

## Summary
- [Theory](#theory)
- [Types of DNS records](#types-of-dns-records)
- [Methodologies](#methodologies)
        - [Sending DNS queries with Nslookup](#sending-dns-queries-with-nslookup)
        - [Sending DNS queries with Dig](#sending-dns-queries-with-dig)
        - [Sending DNS queries with DNSRecon](#sending-dns-queries-with-dnsrecon)
        - [Perform reverse IP lookup](#perform-reverse-ip-lookup)
        - [DNS zone transfer](#dns-zone-transfer)

## Theory
The domain name system (DNS) is a naming database in which internet domain names are located and translated into Internet Protocol (IP) addresses. The domain name system maps the name people use to locate a website to the IP address that a computer uses to locate that website.

## Types of DNS records
| Record | Description |
| ------ | ----------- |
| A | Stores a hostname & its IPv4 address |
| AAAA | Stores a hostname & its IPv6 address |
| CNAME | Stores a hostname that points to another hostname |
| MX | Related to SMTP email server |

## Methodologies
### Sending DNS queries with Nslookup
`nslookup --type=A example.com 10.0.0.1`

### Sending DNS queries with Dig
`dig @10.0.0.1 example.com A`

### Sending DNS queries with DNSRecon
`dnsrecon -d example.com -n 10.0.0.1`

### Perform reverse IP lookup
```
dig -x 10.0.0.1
host 10.0.0.1
dig +noall +answer -x 10.0.0.1 --for cleaner output
nslookup 10.0.0.1
```

### DNS zone transfer
DNS servers host zones. A DNS zone is a portion of the domain name space that is served by a DNS server. For example, domain.com with all its subdomains may be a zone. However, second.example.com may also be a separate zone.

*Initiate a zone transfer*

`dig +short ns domain.com`

*Request for copy of the zone from primary server*

`dig axfr domain.com @zone.domain.com`
