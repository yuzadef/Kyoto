# Rsync

## Summary
- [Theory](#theory)
- [Basic usage](#basic-usage)
  - [Listing files](#listing-files)
  - [Copy files onto local machine](#copy-files-onto-local-machine)
  - [Copy all files onto local machine](#copy-all-files-onto-local-machine)
  - [Sync local files with Rsync](#sync-local-files-with-rsync)

## Theory
Rsync is a command-line tool for copying files and directories between local and remote systems.

## Basic usage
### Listing files
`rsync --list-only rsync://rsync-connect@10.0.0.1`

### Copy files onto local machine
`rsync -zvh file-name /local-file-path/`

### Copy all files onto local machine
`rsync -avzh /remote/path/files /local-file-path/`

### Sync local files with Rsync
`rsync file-name rsync://rsync-connect@10.0.0.1/file-path`
