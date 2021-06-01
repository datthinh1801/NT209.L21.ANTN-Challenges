# Find password
## Task
```
└─$ ./password.exe
inserisci la password per accedere al programma 12345
password errata.
inserisci la password per accedere al programma 324251
password errata.
```
> Tìm password.  

## Solution
Xem kiến trúc file.  
```
└─$ file password.exe
password.exe: PE32 executable (console) Intel 80386 (stripped to external PDB), for MS Windows
```

Mở IDA Pro 32bit.  
Khi xem cửa sổ string thì chúng ta phát hiện những thứ sau:  
![image](https://user-images.githubusercontent.com/44528004/120324201-8ef13e80-c310-11eb-9faa-36aa8fa160a0.png)  

![image](https://user-images.githubusercontent.com/44528004/120324246-9ca6c400-c310-11eb-9f01-62c6264e7010.png)

Vậy mục tiêu của chúng ta là in ra được chuỗi `benvenuto`.  

Xem pseudocode của hàm có sử dụng chuỗi trên.  

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v4; // [esp+0h] [ebp-88h] BYREF
  char v5[4]; // [esp+1Ch] [ebp-6Ch] BYREF
  int v6; // [esp+20h] [ebp-68h]
  int (__cdecl *v7)(int, int, int, int, int, int); // [esp+34h] [ebp-54h]
  int *v8; // [esp+38h] [ebp-50h]
  char *v9; // [esp+3Ch] [ebp-4Ch]
  void (__noreturn *v10)(); // [esp+40h] [ebp-48h]
  int *v11; // [esp+44h] [ebp-44h]
  void *Buf2; // [esp+50h] [ebp-38h]
  int v13; // [esp+54h] [ebp-34h]
  char v14[16]; // [esp+58h] [ebp-30h] BYREF
  void *Buf1; // [esp+68h] [ebp-20h] BYREF
  size_t Size; // [esp+6Ch] [ebp-1Ch]
  char v17[16]; // [esp+70h] [ebp-18h] BYREF
  char v18; // [esp+80h] [ebp-8h] BYREF

  v7 = sub_4017D0;
  v8 = dword_4B5DE0;
  v9 = &v18;
  v11 = &v4;
  v10 = sub_4B5965;
  sub_427910(v5);
  sub_4270A0();
  Buf2 = v14;
  v6 = -1;
  sub_401360("djejie", (int)"");
  Buf1 = v17;
  v6 = 1;
  sub_401360("ggkfjgjfrg", (int)"");
  v6 = 2;
  sub_4B2DC0((int)&dword_4C6860, "inserisci la password per accedere al programma ");
  sub_403800(&dword_4C6900, &Buf1);
  do
  {
    do
    {
      v6 = 2;
      sub_4B0060(&dword_4C6860, "password errata.\n", 17);
      sub_4B0060(&dword_4C6860, "inserisci la password per accedere al programma ", 48);
      sub_403800(&dword_4C6900, &Buf1);
    }
    while ( Size != v13 );
  }
  while ( memcmp(Buf1, Buf2, Size) );
  sub_4B0060(&dword_4C6860, "benvenuto", 9);
  sub_4B0D70(&dword_4C6860);
  if ( Buf1 != v17 )
    j_free(Buf1);
  if ( Buf2 != v14 )
    j_free(Buf2);
  sub_427A70(v5);
  return 0;
}
```  

Vì chương trình sử dụng hàm `do ... while ...` nên chương trình sẽ luôn in ra `password errata.` sau lần nhần đầu tiên. Vậy phần nhập chính của chúng ta sẽ là lần thứ 2 trở đi.  

Bên cạnh đó, chúng ta có 2 chuỗi khá nghi vấn chính là `djejie` và `c`. Chúng ta liền thử 2 chuỗi này.  

1. Chuỗi `ggkfjgjfrg`
```
└─$ ./password.exe
inserisci la password per accedere al programma ggkfjgjfrg
password errata.
inserisci la password per accedere al programma ggkfjgjfrg
```  
> Sai  

2. Chuỗi `djejie`
```
└─$ ./password.exe
inserisci la password per accedere al programma ggkfjgjfrg
password errata.
inserisci la password per accedere al programma ggkfjgjfrg
password errata.
inserisci la password per accedere al programma djejie
benvenuto
```
> Thành công.
