# ZED-Crackme
## Task
```
└─$ ./ZED-Crackme-x64.bin
Segmentation fault
```
> Not really sure!!??

Xem kiến trúc của file.  
```
└─$ file ZED-Crackme-x64.bin
ZED-Crackme-x64.bin: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), statically linked, no section header
```

Ở đây mình thấy được rằng, file này là file `static` nên có lẽ là sẽ không thể được thực thi. Sau một hồi hỏi han bạn bè thì mình được bảo là dùng tool `upx` để unpack file này là có thể thực thi bình thường.  
```
upx -d ZED-Crackme-x64.bin
                       Ultimate Packer for eXecutables
                          Copyright (C) 1996 - 2020
UPX 3.96        Markus Oberhumer, Laszlo Molnar & John Reiser   Jan 23rd 2020

        File size         Ratio      Format      Name
   --------------------   ------   -----------   -----------
     13184 <-      7656   58.07%   linux/amd64   ZED-Crackme-x64.bin

Unpacked 1 file.
```  
> Thật sự thì mình cũng không rõ lắm về pack và unpack nhưng thôi kệ, chạy được là được 😁.  

```
└─$ ./ZED-Crackme-x64.bin
***********************
**      rules:       **
***********************

* do not bruteforce
* do not patch, find instead the serial.

enter the passphrase: kytgytgk
try again
```

> OK, vậy task là tìm passphrase.  

## Solution
Mở IDA Pro 64bit.
