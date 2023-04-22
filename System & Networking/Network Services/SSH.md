# Secure Shell (SSH)

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Connecting to the server](#connecting-to-the-server)
- [Methodologies](#methodologies)
  - [Generate a SSH key](#generate-a-ssh-key)
  - [Port forwarding](#port-forwarding)

## Theory
The SSH protocol (Secure Shell) is used to login from one machine to another securely. It offers several options for strong authentication, as it protects the connections and communications security and integrity with strong encryption. This connection can be used for terminal access, file transfers, and for tunneling other applications.

## Basic usage
### Connecting to the server
```
ssh root@10.0.0.1 -p 22
ssh -i id_rsa root@10.0.0.1
```
Getting a stabilised shell

`ssh anonymous@10.0.0.1 -t 'bash --noprofile' -enables pseudo-tty allocation`

## Methodologies
### Generate a SSH key
`sudo ssh-keygen -f id_rsa`

### Port forwarding
`ssh -L 8080:10.0.0.1:8080 root@10.0.0.1 -p 22 -i id_rsa`
