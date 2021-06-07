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
Mở IDA Pro 64bit và xem pseudocode.  

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+14h] [rbp-9Ch]
  char v5[6]; // [rsp+1Ah] [rbp-96h] BYREF
  __int64 v6[4]; // [rsp+20h] [rbp-90h] BYREF
  char s2[32]; // [rsp+40h] [rbp-70h] BYREF
  char s1[32]; // [rsp+60h] [rbp-50h] BYREF
  char v9[40]; // [rsp+80h] [rbp-30h] BYREF
  unsigned __int64 v10; // [rsp+A8h] [rbp-8h]

  v10 = __readfsqword(0x28u);
  puts("***********************");
  puts("**      rules:       **");
  puts("***********************");
  putchar(10);
  puts("* do not bruteforce");
  puts("* do not patch, find instead the serial.");
  putchar(10);
  strcpy(v9, "This is a top secret text message!");
  __sidt(v5);
  if ( v5[5] == -1 )
  {
    puts("VMware detected");
    exit(1);
  }
  rot(13LL, v9);
  rot(13LL, v9);
  qmemcpy(v6, "AHi23DEADBEEFCOFFEE", 19);
  printf("enter the passphrase: ");
  __isoc99_scanf("%s", s2);
  if ( ptrace(PTRACE_TRACEME, 0LL) < 0 )
  {
    puts("This process is being debugged!!!");
    exit(1);
  }
  s1[0] = LOBYTE(v6[0]) ^ 2;
  s1[1] = BYTE3(v6[0]) - 10;
  s1[2] = BYTE2(v6[0]) + 12;
  s1[3] = BYTE2(v6[0]);
  s1[4] = BYTE1(v6[0]) + 1;
  for ( i = 5; i <= 18; ++i )
    s1[i] = *((_BYTE *)v6 + i) - 1;
  if ( !strcmp(s1, s2) )
    puts("you succeed!!");
  else
    puts("try again");
  return 0;
}
```  

Từ pseudocode trên thì ta có thể thấy, chuỗi `s1` sẽ được tạo ra từ chuỗi `v6` sau khi qua các bước biến đổi. Sau đó, chuỗi được nhập `s2` sẽ được so sánh với chuỗi `s1` này, nếu bằng nhau thì chương trình sẽ in ra chuỗi `you succeed!!`.  

Viết script python để exploit.  

```python
#! /bin/python3

v6 = "AHi23DEADBEEFCOFFEE"
s1 = []

s1.append(ord(v6[0]) ^ 2)
s1.append(ord(v6[3]) - 10)
s1.append(ord(v6[2]) + 12)
s1.append(ord(v6[2]))
s1.append(ord(v6[1]) + 1)

for i in range(5, 19):
    s1.append(ord(v6[i]) - 1)

print(''.join(map(chr, s1)))
```  

Chạy script:  
```
└─$ python3 solution.py
C(uiICD@CADDEBNEEDD
```

### Kiểm tra kết quả
```
└─$ ./ZED-Crackme-x64.bin
***********************
**      rules:       **
***********************

* do not bruteforce
* do not patch, find instead the serial.

enter the passphrase: C(uiICD@CADDEBNEEDD
you succeed!!
```
> Thành công!
