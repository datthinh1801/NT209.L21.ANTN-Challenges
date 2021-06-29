# fence
## Task
```
└─$ cat readme.txt
the encrypted flag is: "arln_pra_dfgafcchsrb_l{ieeye_ea}"
```
```
└─$ ./encryptor hello
lhleo
└─$ ./encryptor arln_pra_dfgafcchsrb_l{ieeye_ea}
lp_gcs_iyeanrdacrleear_affhb{e_}
```
> Cần tìm plaintext sao cho khi mã hóa thì ciphertext có giá trị là `arln_pra_dfgafcchsrb_l{ieeye_ea}`.  

## Solution
Xem kiến trúc file.  
```
└─$ file encryptor
encryptor: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=b417774ddd1cfb3521bac910a9128bd397e2fe9c, not stripped
```

Mở IDA Pro 64bit và xem pseudocode.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  __int64 v3; // rax
  unsigned __int64 v4; // rax
  char *v5; // rax
  unsigned __int64 v6; // rax
  char *v7; // rax
  unsigned __int64 v8; // rax
  char *v9; // rax
  __int64 v10; // rax
  char v12; // [rsp+17h] [rbp-F9h] BYREF
  unsigned __int64 i; // [rsp+18h] [rbp-F8h]
  unsigned __int64 j; // [rsp+20h] [rbp-F0h]
  unsigned __int64 k; // [rsp+28h] [rbp-E8h]
  char v16[32]; // [rsp+30h] [rbp-E0h] BYREF
  char v17[32]; // [rsp+50h] [rbp-C0h] BYREF
  char v18[32]; // [rsp+70h] [rbp-A0h] BYREF
  char v19[32]; // [rsp+90h] [rbp-80h] BYREF
  char v20[32]; // [rsp+B0h] [rbp-60h] BYREF
  char v21[40]; // [rsp+D0h] [rbp-40h] BYREF
  unsigned __int64 v22; // [rsp+F8h] [rbp-18h]

  v22 = __readfsqword(0x28u);
  if ( argc != 2 )
  {
    v3 = std::operator<<<std::char_traits<char>>(&std::cout, "you must supply exactly one argument", envp);
    std::ostream::operator<<(v3, &std::endl<char,std::char_traits<char>>);
    exit(-1);
  }
  std::allocator<char>::allocator(&v12, argv, envp);
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::basic_string(v16, argv[1], &v12);
  std::allocator<char>::~allocator(&v12);
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::basic_string(v17);
  for ( i = 0LL; ; i += 3LL )
  {
    v4 = std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::length(v16);
    if ( i >= v4 )
      break;
    v5 = (char *)std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::operator[](v16, i);
    std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::operator+=(v17, (unsigned int)*v5);
  }
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::basic_string(v18);
  for ( j = 1LL; ; j += 3LL )
  {
    v6 = std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::length(v16);
    if ( j >= v6 )
      break;
    v7 = (char *)std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::operator[](v16, j);
    std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::operator+=(v18, (unsigned int)*v7);
  }
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::basic_string(v19);
  for ( k = 2LL; ; k += 3LL )
  {
    v8 = std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::length(v16);
    if ( k >= v8 )
      break;
    v9 = (char *)std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::operator[](v16, k);
    std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::operator+=(v19, (unsigned int)*v9);
  }
  std::operator+<char>(v20, v19, v17);
  std::operator+<char>(v21, v20, v18);
  v10 = std::operator<<<char>(&std::cout, v21);
  std::ostream::operator<<(v10, &std::endl<char,std::char_traits<char>>);
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::~basic_string((__int64)v21);
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::~basic_string((__int64)v20);
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::~basic_string((__int64)v19);
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::~basic_string((__int64)v18);
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::~basic_string((__int64)v17);
  std::__cxx11::basic_string<char,std::char_traits<char>,std::allocator<char>>::~basic_string((__int64)v16);
  return 0;
}
```


