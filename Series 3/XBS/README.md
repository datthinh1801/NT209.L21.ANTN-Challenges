# XBS
## Task
```
└─$ ./a.out
awefaw
Try again!
```
> Tìm chuỗi nhập.  

## Solution
Xem kiến trúc file.  
```
└─$ file a.out
a.out: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=e645bcadcc14569d7dda684c9fe11b20b1a40bff, for GNU/Linux 3.2.0, not stripped
```  

Mở IDA Pro 64bit và xem cửa sổ string để tìm chuỗi chúc mừng.  
![image](https://user-images.githubusercontent.com/44528004/123735007-992f3a00-d8c8-11eb-8cc1-f897de936df5.png)  

Nhảy đến hàm có tham chiếu tới chuỗi chúc mừng và xem pseudocode.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v3; // ebx
  int v4; // eax
  __int64 v5; // rcx
  char v6; // si
  int v7; // edx
  int v8; // er9
  bool v10; // cc
  char *v11; // rax
  int v12; // edx
  const char *v13; // rdi
  int v15; // [rsp+4h] [rbp-14h] BYREF
  unsigned __int64 v16; // [rsp+8h] [rbp-10h]

  v3 = 0;
  v16 = __readfsqword(0x28u);
  do
  {
    __isoc99_scanf(&unk_2004, &v15, envp);
    v4 = v15;
    v5 = 30LL;
    v6 = 0;
    v7 = 0;
    while ( ((v4 >> v5) & 1) == 0 )
    {
LABEL_9:
      if ( v5-- == 0 )
      {
        if ( v6 )
          v15 = v4;
        goto LABEL_12;
      }
    }
    v8 = a[v5];
    if ( v8 )
    {
      ++v7;
      v4 ^= v8;
      v6 = 1;
      goto LABEL_9;
    }
    if ( v6 )
      v15 = v4;
    a[(int)v5] = v4;
LABEL_12:
    v10 = v7 <= 1;
    envp = (const char **)(unsigned int)(v7 - 1);
    if ( v10 && v3 > 1 )
    {
LABEL_18:
      v13 = "Try again!";
      goto LABEL_19;
    }
    ++v3;
  }
  while ( v3 != 5 );
  v11 = (char *)a;
  v12 = 0;
  do
  {
    v12 += *(_DWORD *)v11;
    v11 += 4;
  }
  while ( v11 != (char *)&a[31] );
  v13 = "Congrats!";
  if ( v12 != 1073840184 )
    goto LABEL_18;
LABEL_19:
  puts(v13);
  return 0;
}
```  

Từ đoạn pseudocode trên, để chương trình in ra chuỗi `Congrats!`, `v12` phải bằng `1073840184` để chương trình không nhảy tới `LABEL_18` và in ra chuỗi `Try again!`.  

Bên cạnh đó, để chương trình không nhảy vào `LABEL_18` thì điều kiện `v10 && v3 > 1` phải sai. Nói cách khác, `v10` phải bằng `0` hoặc `v3 <= 1`.  

#### Trường hợp 1: `v10 == 0`
Để `v10 == 0` thì `v7` phải `> 1`.  
Để `v7 > 1` thì block lệnh sau phải được thực thi ít nhất 2 lần:  
```c
if ( v8 )
    {
      ++v7;
      v4 ^= v8;
      v6 = 1;
      goto LABEL_9;
    }
```  

Để block lệnh trên được thực thi ít nhất 2 lần thì mảng `a` phải có ít nhất 2 phần tử khác `0`.






