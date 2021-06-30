# recursive
## Task
```
└─$ ./CRACKME.exe
PASSWORD: h4ll0
```  
> Tìm `password`.  

## Solution
Xem kiến trúc file.  
```
└─$ file CRACKME.exe
CRACKME.exe: PE32 executable (console) Intel 80386, for MS Windows
```  

Mở IDA Pro 32 bit và xem pseudocode.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  unsigned __int8 v3; // al
  unsigned int v4; // esi
  unsigned int v5; // edi
  void **v6; // eax
  void **v7; // ebx
  void *v8; // ecx
  void **v9; // eax
  void *v10; // ecx
  void **v11; // eax
  void *v12; // ecx
  void **v13; // eax
  void *v14; // ecx
  void **v15; // eax
  void *v16; // ecx
  void **v17; // eax
  void *v18; // ecx
  void **v19; // eax
  void *v20; // ecx
  void **v21; // eax
  void *v22; // ecx
  void **v23; // eax
  void *v24; // ecx
  void **v25; // eax
  void *v26; // ecx
  void **v27; // eax
  void *v28; // ecx
  void **v29; // eax
  void *v30; // ecx
  void **v31; // ecx
  void **v32; // edx
  unsigned int v33; // esi
  bool v34; // cf
  unsigned __int8 v35; // al
  unsigned __int8 v36; // al
  unsigned __int8 v37; // al
  int v38; // eax
  void *v39; // ecx
  void *v40; // ecx
  void *v41; // edx
  void *v42; // ecx
  void *v43; // ecx
  void *v44; // ecx
  void *v45; // ecx
  void *v47[5]; // [esp+14h] [ebp-A0h] BYREF
  unsigned int v48; // [esp+28h] [ebp-8Ch]
  void *v49[5]; // [esp+2Ch] [ebp-88h] BYREF
  unsigned int v50; // [esp+40h] [ebp-74h]
  void *v51[5]; // [esp+44h] [ebp-70h] BYREF
  unsigned int v52; // [esp+58h] [ebp-5Ch]
  void *v53[4]; // [esp+5Ch] [ebp-58h] BYREF
  int v54; // [esp+6Ch] [ebp-48h]
  unsigned int v55; // [esp+70h] [ebp-44h]
  void *Src[4]; // [esp+74h] [ebp-40h] BYREF
  unsigned int v57; // [esp+84h] [ebp-30h]
  unsigned int v58; // [esp+88h] [ebp-2Ch]
  void *Block[4]; // [esp+8Ch] [ebp-28h] BYREF
  int v60; // [esp+9Ch] [ebp-18h]
  unsigned int v61; // [esp+A0h] [ebp-14h]
  int v62; // [esp+B0h] [ebp-4h]

  v47[4] = 0;
  v48 = 15;
  LOBYTE(v47[0]) = 0;
  sub_403190(v47, "Hello! Search the password? :D", 0x1Eu);
  v62 = 0;
  v49[4] = 0;
  v50 = 15;
  LOBYTE(v49[0]) = 0;
  sub_403190(v49, "This not a pass", 0xFu);
  LOBYTE(v62) = 1;
  v51[4] = 0;
  v52 = 15;
  LOBYTE(v51[0]) = 0;
  sub_403190(
    v51,
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "dddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd"
    "ddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddddd",
    0xF03u);
  LOBYTE(v62) = 2;
  sub_4035A0();
  v57 = 0;
  v58 = 15;
  LOBYTE(Src[0]) = 0;
  LOBYTE(v62) = 3;
  v3 = std::ios::widen(std::cin + *(_DWORD *)(std::cin + 4), 10);
  sub_403B90(v3);
  v4 = v57;
  if ( !v57 )
    exit(1);
  v5 = v58;
  v6 = Src;
  v7 = (void **)Src[0];
  if ( v58 >= 0x10 )
    v6 = (void **)Src[0];
  if ( *(_BYTE *)v6 == 49 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v8 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v8 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v8 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v8);
    }
  }
  v9 = Src;
  if ( v5 >= 0x10 )
    v9 = v7;
  if ( *(_BYTE *)v9 == 49 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v10 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v10 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v10 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v10);
    }
  }
  v11 = Src;
  if ( v5 >= 0x10 )
    v11 = v7;
  if ( *(_BYTE *)v11 == 50 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v12 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v12 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v12 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v12);
    }
  }
  v13 = Src;
  if ( v5 >= 0x10 )
    v13 = v7;
  if ( *(_BYTE *)v13 == 51 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v14 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v14 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v14 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v14);
    }
  }
  v15 = Src;
  if ( v5 >= 0x10 )
    v15 = v7;
  if ( *(_BYTE *)v15 == 52 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v16 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v16 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v16 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v16);
    }
  }
  v17 = Src;
  if ( v5 >= 0x10 )
    v17 = v7;
  if ( *(_BYTE *)v17 == 53 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v18 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v18 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v18 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v18);
    }
  }
  v19 = Src;
  if ( v5 >= 0x10 )
    v19 = v7;
  if ( *(_BYTE *)v19 == 54 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v20 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v20 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v20 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v20);
    }
  }
  v21 = Src;
  if ( v5 >= 0x10 )
    v21 = v7;
  if ( *(_BYTE *)v21 == 55 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v22 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v22 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v22 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v22);
    }
  }
  v23 = Src;
  if ( v5 >= 0x10 )
    v23 = v7;
  if ( *(_BYTE *)v23 == 56 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v24 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v24 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v24 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v24);
    }
  }
  v25 = Src;
  if ( v5 >= 0x10 )
    v25 = v7;
  if ( *(_BYTE *)v25 == 57 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v26 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v26 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v26 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v26);
    }
  }
  v27 = Src;
  if ( v5 >= 0x10 )
    v27 = v7;
  if ( *(_BYTE *)v27 == 119 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v28 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v28 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v28 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v28);
    }
  }
  v29 = Src;
  if ( v5 >= 0x10 )
    v29 = v7;
  if ( *(_BYTE *)v29 == 102 )
  {
    v60 = 0;
    v61 = 15;
    LOBYTE(Block[0]) = 0;
    sub_403190(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v61 >= 0x10 )
    {
      v30 = Block[0];
      if ( v61 + 1 >= 0x1000 )
      {
        v30 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v30 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      sub_404015(v30);
    }
  }
  v60 = 0;
  v61 = 15;
  LOBYTE(Block[0]) = 0;
  sub_403190(Block, "crackmes100820201323", 0x14u);
  LOBYTE(v62) = 4;
  v31 = Block;
  v32 = Src;
  if ( v61 >= 0x10 )
    v31 = (void **)Block[0];
  if ( v5 >= 0x10 )
    v32 = v7;
  if ( v4 == v60 )
  {
    v34 = v4 < 4;
    v33 = v4 - 4;
    if ( v34 )
    {
LABEL_107:
      if ( v33 == -4 )
        goto LABEL_116;
    }
    else
    {
      while ( *v32 == *v31 )
      {
        ++v32;
        ++v31;
        v34 = v33 < 4;
        v33 -= 4;
        if ( v34 )
          goto LABEL_107;
      }
    }
    v34 = *(_BYTE *)v32 < *(_BYTE *)v31;
    if ( *(_BYTE *)v32 != *(_BYTE *)v31
      || v33 != -3
      && ((v35 = *((_BYTE *)v32 + 1), v34 = v35 < *((_BYTE *)v31 + 1), v35 != *((_BYTE *)v31 + 1))
       || v33 != -2
       && ((v36 = *((_BYTE *)v32 + 2), v34 = v36 < *((_BYTE *)v31 + 2), v36 != *((_BYTE *)v31 + 2))
        || v33 != -1 && (v37 = *((_BYTE *)v32 + 3), v34 = v37 < *((_BYTE *)v31 + 3), v37 != *((_BYTE *)v31 + 3)))) )
    {
      v38 = v34 ? -1 : 1;
LABEL_117:
      if ( !v38 )
      {
        v54 = 0;
        v55 = 15;
        LOBYTE(v53[0]) = 0;
        sub_403190(v53, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
        if ( v55 >= 0x10 )
        {
          v39 = v53[0];
          if ( v55 + 1 >= 0x1000 )
          {
            v39 = (void *)*((_DWORD *)v53[0] - 1);
            if ( (unsigned int)(v53[0] - v39 - 4) > 0x1F )
              invalid_parameter_noinfo_noreturn();
          }
          sub_404015(v39);
        }
      }
      goto LABEL_123;
    }
LABEL_116:
    v38 = 0;
    goto LABEL_117;
  }
LABEL_123:
  v54 = 0;
  v55 = 15;
  LOBYTE(v53[0]) = 0;
  sub_403190(v53, "oh... sorry XD XD XD XD", 0x17u);
  LOBYTE(v62) = 5;
  sub_402550(Src);
  sub_401300();
  if ( v55 >= 0x10 )
  {
    v40 = v53[0];
    if ( v55 + 1 >= 0x1000 )
    {
      v40 = (void *)*((_DWORD *)v53[0] - 1);
      if ( (unsigned int)(v53[0] - v40 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    sub_404015(v40);
  }
  if ( v61 >= 0x10 )
  {
    v41 = Block[0];
    if ( v61 + 1 >= 0x1000 )
    {
      v41 = (void *)*((_DWORD *)Block[0] - 1);
      if ( (unsigned int)(Block[0] - v41 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    sub_404015(v41);
  }
  if ( v58 >= 0x10 )
  {
    v42 = Src[0];
    if ( v58 + 1 >= 0x1000 )
    {
      v42 = (void *)*((_DWORD *)Src[0] - 1);
      if ( (unsigned int)(Src[0] - v42 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    sub_404015(v42);
  }
  if ( v52 >= 0x10 )
  {
    v43 = v51[0];
    if ( v52 + 1 >= 0x1000 )
    {
      v43 = (void *)*((_DWORD *)v51[0] - 1);
      if ( (unsigned int)(v51[0] - v43 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    sub_404015(v43);
  }
  if ( v50 >= 0x10 )
  {
    v44 = v49[0];
    if ( v50 + 1 >= 0x1000 )
    {
      v44 = (void *)*((_DWORD *)v49[0] - 1);
      if ( (unsigned int)(v49[0] - v44 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    sub_404015(v44);
  }
  if ( v48 >= 0x10 )
  {
    v45 = v47[0];
    if ( v48 + 1 >= 0x1000 )
    {
      v45 = (void *)*((_DWORD *)v47[0] - 1);
      if ( (unsigned int)(v47[0] - v45 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    sub_404015(v45);
  }
  return 0;
}
```
