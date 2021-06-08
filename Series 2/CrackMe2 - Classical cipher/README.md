# CrackMe2
## Task
```
└─$ ./CrackMe2.exe
-----------------------------------
--           CrackMe 2           --
--      by github.com/dajoh      --
-----------------------------------

Enter password: 25151
Fail! You entered the wrong password.

Enter password: hello ?
Fail! You entered the wrong password.
```
> Tìm password.  

## Solution
Xem kiến trúc file.  
```
└─$ file CrackMe2.exe
CrackMe2.exe: PE32 executable (console) Intel 80386, for MS Windows
```

Mở IDA Pro 32 bit và xem pseudocode.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  unsigned __int8 v3; // al
  char *i; // ecx
  int v5; // eax
  char v7; // [esp-Ch] [ebp-110h]
  char v8; // [esp-8h] [ebp-10Ch]
  char v9; // [esp-8h] [ebp-10Ch]
  char v10; // [esp-4h] [ebp-108h]
  char Buffer[256]; // [esp+0h] [ebp-104h] BYREF

  sub_4010B0("-----------------------------------\n", Buffer[0]);
  sub_4010B0("--           CrackMe 2           --\n", v10);
  sub_4010B0("--      by github.com/dajoh      --\n", v8);
  sub_4010B0("-----------------------------------\n\n", v7);
  while ( 1 )
  {
    sub_401010((int)"Enter password: ", 0xFu, Buffer[0]);
    gets_s(Buffer, 0x100u);
    v3 = Buffer[0];
    for ( i = Buffer; *i; v3 = *i )
      *i++ = byte_4188F0[v3];
    v5 = strcmp(Buffer, "HfyhgrgfgrlmYlc579");
    if ( v5 )
      v5 = v5 < 0 ? -1 : 1;
    if ( !v5 )
      break;
    sub_401010((int)"Fail! You entered the wrong password.\n\n", 0xCu, Buffer[0]);
  }
  sub_401010((int)"Congratulations! You entered the correct password.\n\n", 0xAu, Buffer[0]);
  sub_401010((int)"Press enter to exit...", 8u, v9);
  sub_404D1E();
  return 0;
}
```
