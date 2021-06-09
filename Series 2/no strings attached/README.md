# no strings attached
## Task
```
└─$ ./crackme.exe
Enter password: password
WRONG PASSWORD
```
> Tìm password.  

## Solution
Xem kiến trúc của file.  
```
└─$ file crackme.exe
crackme.exe: PE32 executable (console) Intel 80386, for MS Windows
```  

Mở IDA Pro 32bit và xem pseudocode.  

```c
int __cdecl main_0(int argc, const char **argv, const char **envp)
{
  int v3; // eax
  char encrypted_c_string[108]; // [esp+14h] [ebp-E0h] BYREF
  char input[36]; // [esp+80h] [ebp-74h] BYREF
  char correct_string[16]; // [esp+A4h] [ebp-50h] BYREF
  char wrong_string[24]; // [esp+B4h] [ebp-40h] BYREF
  char Str[24]; // [esp+CCh] [ebp-28h] BYREF
  int v10; // [esp+F0h] [ebp-4h]

  strcpy(Str, "Enter password: ");
  strcpy(wrong_string, "WRONG PASSWORD");
  strcpy(correct_string, "CORRECT");
  sub_F712D0(std::cout, Str);
  sub_F710C8();
  v10 = 0;
  sub_F710AF(std::cin, (int)input);
  sub_F71352(encrypted_c_string);
  LOBYTE(v10) = 1;
  sub_F712C1((int)input);
  if ( (unsigned __int8)sub_F71438(encrypted_c_string) )
    v3 = sub_F712D0(std::cout, correct_string);
  else
    v3 = sub_F712D0(std::cout, wrong_string);
  std::ostream::operator<<(v3, sub_F713C5);
  while ( !std::ios_base::eof((std::ios_base *)(*(_DWORD *)(std::cin + 4) + std::cin)) )
    ;
  LOBYTE(v10) = 0;
  sub_F71177(encrypted_c_string);
  v10 = -1;
  sub_F71343(input);
  return 0;
}
```