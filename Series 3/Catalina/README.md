# Catalina
## Task
```
└─$ ./crackme h3ll0
Invalid flag, try again
```
> Tìm password.  

## Solution
Xem kiến trúc của file.  
```
└─$ file crackme
crackme: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=4e67f827e2bb32da50b2ce98842c4a9e9fde0996, stripped
```  

Mở IDA Pro 64 bit.
