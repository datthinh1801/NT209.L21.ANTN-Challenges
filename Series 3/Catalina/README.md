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
![image](https://user-images.githubusercontent.com/44528004/123716502-ec8f9100-d8a4-11eb-8d17-b3070740d964.png)  

Xem cửa sổ string để tìm chuỗi chúc mừng.  
![image](https://user-images.githubusercontent.com/44528004/123716528-fe713400-d8a4-11eb-9b9d-6f71ab88e778.png)  

Xem pseudocode của hàm có chứa chuỗi chúc mừng.  
```c
__int64 __fastcall main(int a1, char **a2, char **a3)
{
  __int64 result; // rax
  int v4; // ebx
  const char *v5; // r12
  size_t v6; // rax
  size_t v7; // rax
  char v8[32]; // [rsp+10h] [rbp-100h] BYREF
  __int64 v9[4]; // [rsp+30h] [rbp-E0h] BYREF
  char v10; // [rsp+50h] [rbp-C0h]
  int v11[34]; // [rsp+60h] [rbp-B0h]
  const char *v12; // [rsp+E8h] [rbp-28h]
  int v13; // [rsp+F4h] [rbp-1Ch]
  int i; // [rsp+F8h] [rbp-18h]
  int v15; // [rsp+FCh] [rbp-14h]

  if ( a1 == 2 )
  {
    v12 = "JTQSRyZKSB05Dh9JgH6fQJIVjJ04UpA7ezxMIHcvpX6X70NJHW4xlxSHHMuLDjCJbzl9ITfgeLbTDLExZENyYrAzn7ehjAMuZf1siTB4HBLgyJ"
          "gpK38LHCq4UvpgqOxeoh72AVgDOYS8HU9xg";
    v11[0] = 4;
    v11[1] = 4;
    v11[2] = 5;
    v11[3] = 4;
    v11[4] = 2;
    v11[5] = 4;
    v11[6] = 3;
    v11[7] = 4;
    v11[8] = 2;
    v11[9] = 4;
    v11[10] = 6;
    v11[11] = 2;
    v11[12] = 4;
    v11[13] = 6;
    v11[14] = 2;
    v11[15] = 5;
    v11[16] = 5;
    v11[17] = 2;
    v11[18] = 3;
    v11[19] = 3;
    v11[20] = 5;
    v11[21] = 4;
    v11[22] = 2;
    v11[23] = 3;
    v11[24] = 4;
    v11[25] = 2;
    v11[26] = 2;
    v11[27] = 3;
    v11[28] = 3;
    v11[29] = 2;
    v11[30] = 4;
    v11[31] = 5;
    v9[0] = 0LL;
    v9[1] = 0LL;
    v9[2] = 0LL;
    v9[3] = 0LL;
    v10 = 0;
    v15 = 0;
    for ( i = 0; i <= 31; ++i )
    {
      *((_BYTE *)v9 + i) = v12[v15];
      v15 += v11[i] + 1;
    }
    sub_1482(v9, v8, 32LL);
    v15 = 0;
    v13 = 0;
    while ( v15 <= 23 )
    {
      v8[v15] ^= 0x41u;
      v4 = (unsigned __int8)v8[v15];
      v5 = a2[1];
      v6 = strlen(v5);
      if ( v6 >= v15 )
        v7 = v15;
      else
        v7 = strlen(a2[1]);
      v13 += v4 == v5[v7];
      ++v15;
    }
    if ( v13 == 24 )
      puts("Congratulations !! you solved the first challenge.");
    else
      puts("Invalid flag, try again");
    result = 0LL;
  }
  else
  {
    puts("Welcome to crackme N1");
    printf("Usage:\n%s <password>\n", *a2);
    result = 0xFFFFFFFFLL;
  }
  return result;
}
```

