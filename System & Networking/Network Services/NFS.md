# Network File System (NFS)

## Summary
- [Theory](#theory)
- [Exporting shares on a server](#exporting-shares-on-a-server)

## Theory
The NFS is a mechanism for storing files on a network. It is a distributed file system that allows users to access files and directories located on remote computers and treat those files and directories as if they were local.

## Exporting shares on a server
*Create a directory for the exported files.*

`mkdir /tmp/mount`

*List the shares on the server.*

`/usr/sbin/showmount -e 10.0.0.1`

*Connect share to mount point.*

`mount -t nfs 10.0.0.1:share-name /tmp/mount -nolock`
