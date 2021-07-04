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
  unsigned int v9; // edx
  void **v10; // eax
  void *v11; // ecx
  unsigned int v12; // edx
  void **v13; // eax
  void *v14; // ecx
  unsigned int v15; // edx
  void **v16; // eax
  void *v17; // ecx
  unsigned int v18; // edx
  void **v19; // eax
  void *v20; // ecx
  unsigned int v21; // edx
  void **v22; // eax
  void *v23; // ecx
  unsigned int v24; // edx
  void **v25; // eax
  void *v26; // ecx
  unsigned int v27; // edx
  void **v28; // eax
  void *v29; // ecx
  unsigned int v30; // edx
  void **v31; // eax
  void *v32; // ecx
  unsigned int v33; // edx
  void **v34; // eax
  void *v35; // ecx
  unsigned int v36; // edx
  void **v37; // eax
  void *v38; // ecx
  unsigned int v39; // edx
  void **v40; // eax
  void *v41; // ecx
  unsigned int v42; // edx
  void **v43; // ecx
  void **v44; // edx
  unsigned int v45; // esi
  bool v46; // cf
  unsigned __int8 v47; // al
  unsigned __int8 v48; // al
  unsigned __int8 v49; // al
  int v50; // eax
  void *v51; // ecx
  unsigned int v52; // edx
  void *v53; // ecx
  unsigned int v54; // edx
  void *v55; // edx
  unsigned int v56; // ecx
  void *v57; // ecx
  unsigned int v58; // edx
  void *v59; // ecx
  unsigned int v60; // edx
  void *v61; // ecx
  unsigned int v62; // edx
  void *v63; // ecx
  unsigned int v64; // edx
  int v66; // [esp-18h] [ebp-CCh] BYREF
  unsigned int v67; // [esp-4h] [ebp-B8h]
  void *v68[5]; // [esp+14h] [ebp-A0h] BYREF
  unsigned int v69; // [esp+28h] [ebp-8Ch]
  void *v70[5]; // [esp+2Ch] [ebp-88h] BYREF
  unsigned int v71; // [esp+40h] [ebp-74h]
  void *v72[5]; // [esp+44h] [ebp-70h] BYREF
  unsigned int v73; // [esp+58h] [ebp-5Ch]
  void *v74[4]; // [esp+5Ch] [ebp-58h] BYREF
  int v75; // [esp+6Ch] [ebp-48h]
  unsigned int v76; // [esp+70h] [ebp-44h]
  void *Src[4]; // [esp+74h] [ebp-40h] BYREF
  unsigned int v78; // [esp+84h] [ebp-30h]
  unsigned int v79; // [esp+88h] [ebp-2Ch]
  void *Block[4]; // [esp+8Ch] [ebp-28h] BYREF
  int v81; // [esp+9Ch] [ebp-18h]
  unsigned int v82; // [esp+A0h] [ebp-14h]
  int v83; // [esp+B0h] [ebp-4h]

  v68[4] = 0;
  v69 = 15;
  LOBYTE(v68[0]) = 0;
  fake_print(v68, "Hello! Search the password? :D", 0x1Eu);
  v83 = 0;
  v70[4] = 0;
  v71 = 15;
  LOBYTE(v70[0]) = 0;
  fake_print(v70, "This not a pass", 0xFu);
  LOBYTE(v83) = 1;
  v72[4] = 0;
  v73 = 15;
  LOBYTE(v72[0]) = 0;
  fake_print(
    v72,
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
  LOBYTE(v83) = 2;
  sub_335A0();
  v78 = 0;
  v79 = 15;
  LOBYTE(Src[0]) = 0;
  LOBYTE(v83) = 3;
  v3 = std::ios::widen(std::cin + *(_DWORD *)(std::cin + 4), 10);
  sub_33B90(v3);
  v4 = v78;
  if ( !v78 )
    exit(1);
  v5 = v79;
  v6 = Src;
  v7 = (void **)Src[0];
  if ( v79 >= 0x10 )
    v6 = (void **)Src[0];
  if ( *(_BYTE *)v6 == '1' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v8 = Block[0];
      v9 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v8 = (void *)*((_DWORD *)Block[0] - 1);
        v9 = v82 + 36;
        if ( (unsigned int)(Block[0] - v8 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v9;
      sub_34015(v8);
    }
  }
  v10 = Src;
  if ( v5 >= 0x10 )
    v10 = v7;
  if ( *(_BYTE *)v10 == '1' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v11 = Block[0];
      v12 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v11 = (void *)*((_DWORD *)Block[0] - 1);
        v12 = v82 + 36;
        if ( (unsigned int)(Block[0] - v11 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v12;
      sub_34015(v11);
    }
  }
  v13 = Src;
  if ( v5 >= 0x10 )
    v13 = v7;
  if ( *(_BYTE *)v13 == '2' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v14 = Block[0];
      v15 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v14 = (void *)*((_DWORD *)Block[0] - 1);
        v15 = v82 + 36;
        if ( (unsigned int)(Block[0] - v14 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v15;
      sub_34015(v14);
    }
  }
  v16 = Src;
  if ( v5 >= 0x10 )
    v16 = v7;
  if ( *(_BYTE *)v16 == '3' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v17 = Block[0];
      v18 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v17 = (void *)*((_DWORD *)Block[0] - 1);
        v18 = v82 + 36;
        if ( (unsigned int)(Block[0] - v17 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v18;
      sub_34015(v17);
    }
  }
  v19 = Src;
  if ( v5 >= 0x10 )
    v19 = v7;
  if ( *(_BYTE *)v19 == '4' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v20 = Block[0];
      v21 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v20 = (void *)*((_DWORD *)Block[0] - 1);
        v21 = v82 + 36;
        if ( (unsigned int)(Block[0] - v20 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v21;
      sub_34015(v20);
    }
  }
  v22 = Src;
  if ( v5 >= 0x10 )
    v22 = v7;
  if ( *(_BYTE *)v22 == '5' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v23 = Block[0];
      v24 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v23 = (void *)*((_DWORD *)Block[0] - 1);
        v24 = v82 + 36;
        if ( (unsigned int)(Block[0] - v23 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v24;
      sub_34015(v23);
    }
  }
  v25 = Src;
  if ( v5 >= 0x10 )
    v25 = v7;
  if ( *(_BYTE *)v25 == '6' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v26 = Block[0];
      v27 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v26 = (void *)*((_DWORD *)Block[0] - 1);
        v27 = v82 + 36;
        if ( (unsigned int)(Block[0] - v26 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v27;
      sub_34015(v26);
    }
  }
  v28 = Src;
  if ( v5 >= 0x10 )
    v28 = v7;
  if ( *(_BYTE *)v28 == '7' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v29 = Block[0];
      v30 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v29 = (void *)*((_DWORD *)Block[0] - 1);
        v30 = v82 + 36;
        if ( (unsigned int)(Block[0] - v29 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v30;
      sub_34015(v29);
    }
  }
  v31 = Src;
  if ( v5 >= 0x10 )
    v31 = v7;
  if ( *(_BYTE *)v31 == '8' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v32 = Block[0];
      v33 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v32 = (void *)*((_DWORD *)Block[0] - 1);
        v33 = v82 + 36;
        if ( (unsigned int)(Block[0] - v32 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v33;
      sub_34015(v32);
    }
  }
  v34 = Src;
  if ( v5 >= 0x10 )
    v34 = v7;
  if ( *(_BYTE *)v34 == '9' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v35 = Block[0];
      v36 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v35 = (void *)*((_DWORD *)Block[0] - 1);
        v36 = v82 + 36;
        if ( (unsigned int)(Block[0] - v35 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v36;
      sub_34015(v35);
    }
  }
  v37 = Src;
  if ( v5 >= 0x10 )
    v37 = v7;
  if ( *(_BYTE *)v37 == 'w' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v38 = Block[0];
      v39 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v38 = (void *)*((_DWORD *)Block[0] - 1);
        v39 = v82 + 36;
        if ( (unsigned int)(Block[0] - v38 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v39;
      sub_34015(v38);
    }
  }
  v40 = Src;
  if ( v5 >= 0x10 )
    v40 = v7;
  if ( *(_BYTE *)v40 == 'f' )
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v41 = Block[0];
      v42 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v41 = (void *)*((_DWORD *)Block[0] - 1);
        v42 = v82 + 36;
        if ( (unsigned int)(Block[0] - v41 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v42;
      sub_34015(v41);
    }
  }
  v81 = 0;
  v82 = 15;
  LOBYTE(Block[0]) = 0;
  fake_print(Block, "crackmes100820201323", 0x14u);
  LOBYTE(v83) = 4;
  v43 = Block;
  v44 = Src;
  if ( v82 >= 0x10 )
    v43 = (void **)Block[0];
  if ( v5 >= 0x10 )
    v44 = v7;
  if ( v4 == v81 )
  {
    v46 = v4 < 4;
    v45 = v4 - 4;
    if ( v46 )
    {
LABEL_107:
      if ( v45 == -4 )
        goto LABEL_116;
    }
    else
    {
      while ( *v44 == *v43 )
      {
        ++v44;
        ++v43;
        v46 = v45 < 4;
        v45 -= 4;
        if ( v46 )
          goto LABEL_107;
      }
    }
    v46 = *(_BYTE *)v44 < *(_BYTE *)v43;
    if ( *(_BYTE *)v44 != *(_BYTE *)v43
      || v45 != -3
      && ((v47 = *((_BYTE *)v44 + 1), v46 = v47 < *((_BYTE *)v43 + 1), v47 != *((_BYTE *)v43 + 1))
       || v45 != -2
       && ((v48 = *((_BYTE *)v44 + 2), v46 = v48 < *((_BYTE *)v43 + 2), v48 != *((_BYTE *)v43 + 2))
        || v45 != -1 && (v49 = *((_BYTE *)v44 + 3), v46 = v49 < *((_BYTE *)v43 + 3), v49 != *((_BYTE *)v43 + 3)))) )
    {
      v50 = v46 ? -1 : 1;
LABEL_117:
      if ( !v50 )
      {
        v75 = 0;
        v76 = 15;
        LOBYTE(v74[0]) = 0;
        fake_print(v74, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
        if ( v76 >= 0x10 )
        {
          v51 = v74[0];
          v52 = v76 + 1;
          if ( v76 + 1 >= 0x1000 )
          {
            v51 = (void *)*((_DWORD *)v74[0] - 1);
            v52 = v76 + 36;
            if ( (unsigned int)(v74[0] - v51 - 4) > 0x1F )
              invalid_parameter_noinfo_noreturn();
          }
          v67 = v52;
          sub_34015(v51);
        }
      }
      goto LABEL_123;
    }
LABEL_116:
    v50 = 0;
    goto LABEL_117;
  }
LABEL_123:
  v75 = 0;
  v76 = 15;
  LOBYTE(v74[0]) = 0;
  fake_print(v74, "oh... sorry XD XD XD XD", 0x17u);
  LOBYTE(v83) = 5;
  copy(&v66, Src);
  true_print();
  if ( v76 >= 0x10 )
  {
    v53 = v74[0];
    v54 = v76 + 1;
    if ( v76 + 1 >= 0x1000 )
    {
      v53 = (void *)*((_DWORD *)v74[0] - 1);
      v54 = v76 + 36;
      if ( (unsigned int)(v74[0] - v53 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    v67 = v54;
    sub_34015(v53);
  }
  if ( v82 >= 0x10 )
  {
    v55 = Block[0];
    v56 = v82 + 1;
    if ( v82 + 1 >= 0x1000 )
    {
      v55 = (void *)*((_DWORD *)Block[0] - 1);
      v56 = v82 + 36;
      if ( (unsigned int)(Block[0] - v55 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    v67 = v56;
    sub_34015(v55);
  }
  if ( v79 >= 0x10 )
  {
    v57 = Src[0];
    v58 = v79 + 1;
    if ( v79 + 1 >= 0x1000 )
    {
      v57 = (void *)*((_DWORD *)Src[0] - 1);
      v58 = v79 + 36;
      if ( (unsigned int)(Src[0] - v57 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    v67 = v58;
    sub_34015(v57);
  }
  if ( v73 >= 0x10 )
  {
    v59 = v72[0];
    v60 = v73 + 1;
    if ( v73 + 1 >= 0x1000 )
    {
      v59 = (void *)*((_DWORD *)v72[0] - 1);
      v60 = v73 + 36;
      if ( (unsigned int)(v72[0] - v59 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    v67 = v60;
    sub_34015(v59);
  }
  if ( v71 >= 0x10 )
  {
    v61 = v70[0];
    v62 = v71 + 1;
    if ( v71 + 1 >= 0x1000 )
    {
      v61 = (void *)*((_DWORD *)v70[0] - 1);
      v62 = v71 + 36;
      if ( (unsigned int)(v70[0] - v61 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    v67 = v62;
    sub_34015(v61);
  }
  if ( v69 >= 0x10 )
  {
    v63 = v68[0];
    v64 = v69 + 1;
    if ( v69 + 1 >= 0x1000 )
    {
      v63 = (void *)*((_DWORD *)v68[0] - 1);
      v64 = v69 + 36;
      if ( (unsigned int)(v68[0] - v63 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    v67 = v64;
    sub_34015(v63);
  }
  return 0;
}
```  

Đầu tiên, ta có thể thấy được đoạn code đọc input sau:  
```c
v3 = std::ios::widen(std::cin + *(_DWORD *)(std::cin + 4), 10);
sub_33B90(v3);
```  

Sau đó, phần tử đầu tiên của input sẽ lần lượt được so sánh với các ký tự `'1', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'w', 'f'`. Và các block lệnh trong câu lệnh `if` của tất cả các trường hợp trên đều giống nhau. Ví dụ:  
```c
  {
    v81 = 0;
    v82 = 15;
    LOBYTE(Block[0]) = 0;
    fake_print(Block, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
    if ( v82 >= 0x10 )
    {
      v38 = Block[0];
      v39 = v82 + 1;
      if ( v82 + 1 >= 0x1000 )
      {
        v38 = (void *)*((_DWORD *)Block[0] - 1);
        v39 = v82 + 36;
        if ( (unsigned int)(Block[0] - v38 - 4) > 0x1F )
          invalid_parameter_noinfo_noreturn();
      }
      v67 = v39;
      sub_34015(v38);
    }
  }
```  

Tuy nhiên, khi ta nhập vào một chuỗi có ký tự đầu tiên là một trong các ký tự trên thì chẳng có gì xảy ra cả. Mặc dù trong block lệnh có chứa một câu lệnh có đối số truyền vào là `SUCCESS! tell us how this crackme was solved` nhưng thực tế thì không có chuỗi nào được in ra màn hình.  

Phân tích tiếp đoạn code phía sau thì ta thấy có thêm 3 lần hàm `fake_print()` được gọi.  

Đoạn đầu tiên:  
```c
v81 = 0;
  v82 = 15;
  LOBYTE(Block[0]) = 0;
  fake_print(Block, "crackmes100820201323", 0x14u);
  LOBYTE(v83) = 4;
  v43 = Block;
  v44 = Src;
  if ( v82 >= 0x10 )
    v43 = (void **)Block[0];
  if ( v5 >= 0x10 )
    v44 = v7;
  if ( v4 == v81 )
  {
    v46 = v4 < 4;
    v45 = v4 - 4;
    if ( v46 )
    {
LABEL_107:
      if ( v45 == -4 )
        goto LABEL_116;
    }
    else
    {
      while ( *v44 == *v43 )
      {
        ++v44;
        ++v43;
        v46 = v45 < 4;
        v45 -= 4;
        if ( v46 )
          goto LABEL_107;
      }
    }
    v46 = *(_BYTE *)v44 < *(_BYTE *)v43;
    if ( *(_BYTE *)v44 != *(_BYTE *)v43
      || v45 != -3
      && ((v47 = *((_BYTE *)v44 + 1), v46 = v47 < *((_BYTE *)v43 + 1), v47 != *((_BYTE *)v43 + 1))
       || v45 != -2
       && ((v48 = *((_BYTE *)v44 + 2), v46 = v48 < *((_BYTE *)v43 + 2), v48 != *((_BYTE *)v43 + 2))
        || v45 != -1 && (v49 = *((_BYTE *)v44 + 3), v46 = v49 < *((_BYTE *)v43 + 3), v49 != *((_BYTE *)v43 + 3)))) )
    {
      v50 = v46 ? -1 : 1;
LABEL_117:
      if ( !v50 )
      {
        v75 = 0;
        v76 = 15;
        LOBYTE(v74[0]) = 0;
        fake_print(v74, "SUCCESS! tell us how this crackme was solved", 0x2Cu);
        if ( v76 >= 0x10 )
        {
          v51 = v74[0];
          v52 = v76 + 1;
          if ( v76 + 1 >= 0x1000 )
          {
            v51 = (void *)*((_DWORD *)v74[0] - 1);
            v52 = v76 + 36;
            if ( (unsigned int)(v74[0] - v51 - 4) > 0x1F )
              invalid_parameter_noinfo_noreturn();
          }
          v67 = v52;
          sub_34015(v51);
        }
      }
      goto LABEL_123;
    }
LABEL_116:
    v50 = 0;
    goto LABEL_117;
  }
```  

Ở đoạn code này, ngoài 2 lần gọi hàm `fake_print()` ra thì không có hàm nào khác khả nghi được gọi nên ta bỏ qua.  

Ở đoạn code tiếp theo:  
```c
v75 = 0;
  v76 = 15;
  LOBYTE(v74[0]) = 0;
  fake_print(v74, "oh... sorry XD XD XD XD", 0x17u);
  LOBYTE(v83) = 5;
  copy(&v66, Src);
  true_print();
  if ( v76 >= 0x10 )
  {
    v53 = v74[0];
    v54 = v76 + 1;
    if ( v76 + 1 >= 0x1000 )
    {
      v53 = (void *)*((_DWORD *)v74[0] - 1);
      v54 = v76 + 36;
      if ( (unsigned int)(v74[0] - v53 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    v67 = v54;
    sub_34015(v53);
```  

Ở đây, mình phát hiện ra có gọi một hàm khác và mình đổi tên thành hàm `true_print()` vì khi xem pseudocode của hàm này, mình thấy có một số chuỗi được gán và so sánh. Bên cạnh đó, hàm này cũng gọi một hàm khác với đối số truyền vào là chuỗi `SUCCESS! ...`, tuy nhiên tên hàm này vẫn là tên hàm mặc định `sub_32510()`. Điều đó có nghĩa là hàm này khác với hàm `fake_print()`. Vì các lí do trên nên mình đoán kết quả cuối cùng sẽ được in ra tại đây.  
```c
void __cdecl sub_31300(void *Src, int a2, int a3, int a4, unsigned int a5, unsigned int a6)
{
  int v6; // esi
  int v7; // edi
  void **v8; // eax
  void *v9; // ecx
  void **v10; // eax
  void **v11; // eax
  void **v12; // eax
  void **v13; // eax
  void *v14; // ecx
  void **v15; // eax
  char *v16; // ecx
  unsigned int v17; // esi
  int v18; // eax
  char *v19; // edx
  bool v20; // cf
  unsigned __int8 v21; // al
  unsigned __int8 v22; // al
  unsigned __int8 v23; // al
  int v24; // eax
  char *v25; // ecx
  int v26; // eax
  char *v27; // edx
  bool v28; // cf
  unsigned __int8 v29; // al
  unsigned __int8 v30; // al
  unsigned __int8 v31; // al
  int v32; // eax
  char *v33; // ecx
  int v34; // eax
  char *v35; // edx
  bool v36; // cf
  unsigned __int8 v37; // al
  unsigned __int8 v38; // al
  unsigned __int8 v39; // al
  int v40; // eax
  char *v41; // ecx
  const char *v42; // edx
  unsigned int v43; // esi
  bool v44; // cf
  unsigned __int8 v45; // al
  unsigned __int8 v46; // al
  unsigned __int8 v47; // al
  int v48; // eax
  char *v49; // ecx
  const char *v50; // edx
  unsigned int v51; // esi
  bool v52; // cf
  unsigned __int8 v53; // al
  unsigned __int8 v54; // al
  unsigned __int8 v55; // al
  int v56; // eax
  void **v57; // eax
  void *v58; // ecx
  _BYTE *v59; // ecx
  int v60; // [esp-Ch] [ebp-110h]
  int v61; // [esp-8h] [ebp-10Ch]
  int v62; // [esp-4h] [ebp-108h]
  int v63; // [esp+0h] [ebp-104h]
  int v64; // [esp+4h] [ebp-100h]
  int v65; // [esp+10h] [ebp-F4h]
  int v66[44]; // [esp+14h] [ebp-F0h] BYREF
  char v67[24]; // [esp+C4h] [ebp-40h] BYREF
  void *Block[5]; // [esp+DCh] [ebp-28h] BYREF
  unsigned int v69; // [esp+F0h] [ebp-14h]
  int v70; // [esp+100h] [ebp-4h]

  v6 = 0;
  v70 = 0;
  v7 = 0;
  if ( a5 )
  {
    while ( v7 )
    {
      switch ( v7 )
      {
        case 1:
          copy(Block, &Src);
          v10 = Block;
          v9 = Block[0];
          if ( v69 >= 0x10 )
            v10 = (void **)Block[0];
          if ( *((_BYTE *)v10 + 1) != 116 )
          {
            if ( v69 < 0x10 )
              break;
            if ( v69 + 1 >= 0x1000 )
            {
              v9 = (void *)*((_DWORD *)Block[0] - 1);
              if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
                goto LABEL_153;
            }
            goto LABEL_76;
          }
          if ( v69 < 0x10 )
            goto LABEL_58;
          if ( v69 + 1 >= 0x1000 )
          {
            v9 = (void *)*((_DWORD *)Block[0] - 1);
            if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
              goto LABEL_153;
          }
          goto LABEL_45;
        case 2:
          copy(Block, &Src);
          v11 = Block;
          v9 = Block[0];
          if ( v69 >= 0x10 )
            v11 = (void **)Block[0];
          if ( *((_BYTE *)v11 + 2) != 111 )
          {
            if ( v69 < 0x10 )
              break;
            if ( v69 + 1 >= 0x1000 )
            {
              v9 = (void *)*((_DWORD *)Block[0] - 1);
              if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
                goto LABEL_153;
            }
            goto LABEL_76;
          }
          if ( v69 < 0x10 )
            goto LABEL_58;
          if ( v69 + 1 >= 0x1000 )
          {
            v9 = (void *)*((_DWORD *)Block[0] - 1);
            if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
              goto LABEL_153;
          }
          goto LABEL_45;
        case 3:
          copy(Block, &Src);
          v12 = Block;
          v9 = Block[0];
          if ( v69 >= 0x10 )
            v12 = (void **)Block[0];
          if ( *((_BYTE *)v12 + 3) != 112 )
          {
            if ( v69 < 0x10 )
              break;
            if ( v69 + 1 >= 0x1000 )
            {
              v9 = (void *)*((_DWORD *)Block[0] - 1);
              if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
                goto LABEL_153;
            }
            goto LABEL_76;
          }
          if ( v69 < 0x10 )
            goto LABEL_58;
          if ( v69 + 1 >= 0x1000 )
          {
            v9 = (void *)*((_DWORD *)Block[0] - 1);
            if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
              goto LABEL_153;
          }
          goto LABEL_45;
        case 4:
          copy(Block, &Src);
          v13 = Block;
          v14 = Block[0];
          if ( v69 >= 0x10 )
            v13 = (void **)Block[0];
          if ( *((_BYTE *)v13 + 4) == 105 )
          {
            if ( v69 >= 0x10 )
            {
              if ( v69 + 1 >= 0x1000 )
              {
                v14 = (void *)*((_DWORD *)Block[0] - 1);
                if ( (unsigned int)(Block[0] - v14 - 4) > 0x1F )
                  goto LABEL_153;
              }
              sub_34015(v14);
            }
LABEL_58:
            ++v6;
            break;
          }
          if ( v69 >= 0x10 )
          {
            if ( v69 + 1 >= 0x1000 )
            {
              v14 = (void *)*((_DWORD *)Block[0] - 1);
              if ( (unsigned int)(Block[0] - v14 - 4) > 0x1F )
                goto LABEL_153;
            }
            sub_34015(v14);
          }
          break;
        case 5:
          copy(Block, &Src);
          v15 = Block;
          v9 = Block[0];
          if ( v69 >= 0x10 )
            v15 = (void **)Block[0];
          if ( *((_BYTE *)v15 + 5) == 116 )
          {
            if ( v69 >= 0x10 )
            {
              if ( v69 + 1 >= 0x1000 )
              {
                v9 = (void *)*((_DWORD *)Block[0] - 1);
                if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
                  goto LABEL_153;
              }
              sub_34015(v9);
            }
            ++v6;
          }
          else if ( v69 >= 0x10 )
          {
            if ( v69 + 1 >= 0x1000 )
            {
              v9 = (void *)*((_DWORD *)Block[0] - 1);
              if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
                goto LABEL_153;
            }
LABEL_76:
            sub_34015(v9);
            break;
          }
          break;
      }
LABEL_77:
      if ( ++v7 >= a5 )
        goto LABEL_78;
    }
    copy(Block, &Src);
    v8 = Block;
    v9 = Block[0];
    if ( v69 >= 0x10 )
      v8 = (void **)Block[0];
    if ( *(_BYTE *)v8 != 115 )
    {
      if ( v69 < 0x10 )
        goto LABEL_77;
      if ( v69 + 1 >= 0x1000 )
      {
        v9 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
          goto LABEL_153;
      }
      goto LABEL_76;
    }
    if ( v69 < 0x10 )
      goto LABEL_58;
    if ( v69 + 1 >= 0x1000 )
    {
      v9 = (void *)*((_DWORD *)Block[0] - 1);
      if ( (unsigned int)(Block[0] - v9 - 4) > 0x1F )
        goto LABEL_153;
    }
LABEL_45:
    sub_34015(v9);
    ++v6;
    goto LABEL_77;
  }
LABEL_78:
  if ( v6 == 6 )
  {
    v16 = (char *)&Src;
    v17 = a5;
    if ( a6 >= 0x10 )
      v16 = (char *)Src;
    if ( a5 != 7 )
      goto LABEL_118;
    v18 = *(_DWORD *)v16;
    v19 = "stopped";
    v65 = 3;
    if ( v18 == *(_DWORD *)"stopped" )
    {
      v16 += 4;
      v65 = -1;
      v19 = "ped";
    }
    v20 = (unsigned __int8)*v16 < (unsigned __int8)*v19;
    if ( *v16 == *v19
      && (v21 = v16[1], v20 = v21 < (unsigned __int8)v19[1], v21 == v19[1])
      && (v22 = v16[2], v20 = v22 < (unsigned __int8)v19[2], v22 == v19[2])
      && (v65 == -1 || (v23 = v16[3], v20 = v23 < (unsigned __int8)v19[3], v23 == v19[3])) )
    {
      v24 = 0;
    }
    else
    {
      v24 = v20 ? -1 : 1;
    }
    if ( v24 )
    {
LABEL_118:
      v25 = (char *)&Src;
      if ( a6 >= 0x10 )
        v25 = (char *)Src;
      if ( a5 != 6 )
        goto LABEL_162;
      v26 = *(_DWORD *)v25;
      v27 = "stopin";
      v65 = 2;
      if ( v26 == *(_DWORD *)"stopin" )
      {
        v25 += 4;
        v65 = -2;
        v27 = "in";
      }
      v28 = (unsigned __int8)*v25 < (unsigned __int8)*v27;
      if ( *v25 == *v27
        && (v29 = v25[1], v28 = v29 < (unsigned __int8)v27[1], v29 == v27[1])
        && (v65 == -2
         || (v30 = v25[2], v28 = v30 < (unsigned __int8)v27[2], v30 == v27[2])
         && (v31 = v25[3], v28 = v31 < (unsigned __int8)v27[3], v31 == v27[3])) )
      {
        v32 = 0;
      }
      else
      {
        v32 = v28 ? -1 : 1;
      }
      if ( v32 )
      {
LABEL_162:
        v33 = (char *)&Src;
        if ( a6 >= 0x10 )
          v33 = (char *)Src;
        if ( a5 != 7 )
          goto LABEL_119;
        v34 = *(_DWORD *)v33;
        v35 = "shakeit";
        v65 = 3;
        if ( v34 == *(_DWORD *)"shakeit" )
        {
          v33 += 4;
          v65 = -1;
          v35 = "eit";
        }
        v36 = (unsigned __int8)*v33 < (unsigned __int8)*v35;
        if ( *v33 == *v35
          && (v37 = v33[1], v36 = v37 < (unsigned __int8)v35[1], v37 == v35[1])
          && (v38 = v33[2], v36 = v38 < (unsigned __int8)v35[2], v38 == v35[2])
          && (v65 == -1 || (v39 = v33[3], v36 = v39 < (unsigned __int8)v35[3], v39 == v35[3])) )
        {
          v40 = 0;
        }
        else
        {
          v40 = v36 ? -1 : 1;
        }
        if ( v40 )
        {
LABEL_119:
          v41 = (char *)&Src;
          if ( a6 >= 0x10 )
            v41 = (char *)Src;
          if ( a5 == 11 )
          {
            v42 = "MessageBoxA";
            v43 = 7;
            do
            {
              if ( *(_DWORD *)v41 != *(_DWORD *)v42 )
                break;
              v41 += 4;
              v42 += 4;
              v20 = v43 < 4;
              v43 -= 4;
            }
            while ( !v20 );
            v44 = (unsigned __int8)*v41 < (unsigned int)*v42;
            if ( *v41 == *v42
              && (v45 = v41[1], v44 = v45 < (unsigned int)v42[1], v45 == v42[1])
              && (v46 = v41[2], v44 = v46 < (unsigned int)v42[2], v46 == v42[2])
              && (v43 == -1 || (v47 = v41[3], v44 = v47 < (unsigned int)v42[3], v47 == v42[3])) )
            {
              v48 = 0;
            }
            else
            {
              v48 = v44 ? -1 : 1;
            }
            if ( !v48 )
              goto LABEL_92;
            v17 = a5;
          }
          v49 = (char *)&Src;
          if ( a6 >= 0x10 )
            v49 = (char *)Src;
          if ( v17 != 11 )
            goto LABEL_148;
          v50 = "ExitProcess";
          v51 = 7;
          do
          {
            if ( *(_DWORD *)v49 != *(_DWORD *)v50 )
              break;
            v49 += 4;
            v50 += 4;
            v20 = v51 < 4;
            v51 -= 4;
          }
          while ( !v20 );
          v52 = (unsigned __int8)*v49 < (unsigned int)*v50;
          if ( *v49 == *v50
            && (v53 = v49[1], v52 = v53 < (unsigned int)v50[1], v53 == v50[1])
            && (v54 = v49[2], v52 = v54 < (unsigned int)v50[2], v54 == v50[2])
            && (v51 == -1 || (v55 = v49[3], v52 = v55 < (unsigned int)v50[3], v55 == v50[3])) )
          {
            v56 = 0;
          }
          else
          {
            v56 = v52 ? -1 : 1;
          }
          if ( v56 )
          {
LABEL_148:
            sub_31120(Block, v63, v64);
            LOBYTE(v70) = 1;
            memset(v66, 0, sizeof(v66));
            v57 = Block;
            if ( v69 >= 0x10 )
              v57 = (void **)Block[0];
            sub_332D0(v57, v60, v61, v62);
            *(int *)((char *)v66 + *(_DWORD *)(v66[0] + 4)) = (int)&std::ofstream::`vftable';
            *(int *)((char *)&v65 + *(_DWORD *)(v66[0] + 4)) = *(_DWORD *)(v66[0] + 4) - 104;
            LOBYTE(v70) = 2;
            sub_335A0();
            std::ios::clear((char *)v66 + *(_DWORD *)(v66[0] + 4), 0, 0);
            sub_31A80(v66);
            if ( v69 >= 0x10 )
            {
              v58 = Block[0];
              if ( v69 + 1 >= 0x1000 )
              {
                v58 = (void *)*((_DWORD *)Block[0] - 1);
                if ( (unsigned int)(Block[0] - v58 - 4) > 0x1F )
LABEL_153:
                  invalid_parameter_noinfo_noreturn();
              }
              sub_34015(v58);
            }
            goto LABEL_155;
          }
        }
      }
    }
LABEL_92:
    sub_32510(v67, "SUCCESS! tell us how this crackme was solved");
    exit(0);
  }
LABEL_155:
  if ( a6 >= 0x10 )
  {
    v59 = Src;
    if ( a6 + 1 >= 0x1000 )
    {
      v59 = (_BYTE *)*((_DWORD *)Src - 1);
      if ( (unsigned int)((_BYTE *)Src - v59 - 4) > 0x1F )
        invalid_parameter_noinfo_noreturn();
    }
    sub_34015(v59);
  }
}
```  


