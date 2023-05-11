# Cryptgonography

## Summary
- [Steganography](#steganography)
  - [Extract .png file](#extract-png-file)
  - [Extract .jpg file](#extract-jpg-file)
  - [Crack .jpg keyphrase](#crack-jpg-keyphrase)
- [Cryptography](#cryptography)
  - [Crack GPG keyphrase](#crack-gpg-keyphrase)
  - [Decrypt GPG message block](#decrypt-gpg-message-block)

## Steganography
### Extract .png file
```
binwalk [file]
binwalk [file] -e  {to extract the zip file if there is}
```

### Extract .jpg file
```
steghide info [file]
steghide --extract -sf [file]   {to extract data from the file}
stegseek --extract example.jpg 
```

### Crack .jpg keyphrase
```
stegseek example.jpg wordlists.txt
stegseek --seed example.jpg
```

## Cryptography
### Crack GPG keyphrase
```
gpg2john [private key file] > output
john --wordlist=rockyou.txt output
```

### Decrypt GPG message block
```
gpg --import [private gpg key]
gpg --decrypt [gpg message block]
gpg --decrypt-file [gpg message block]
```
