# crackme_by_chrisK_v02
## Task
```
└─$ ./crackme_by_chrisK_v02.exe
Enter Password: 1234
```  
> Tìm password.

## Solution
Xem kiến trúc file.  
```
└─$ file crackme_by_chrisK_v02.exe
crackme_by_chrisK_v02.exe: PE32 executable (console) Intel 80386, for MS Windows
```  

Mở IDA Pro 32bit và xem pseudocode.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int result; // eax
  int v4; // eax
  char *v5; // ebx
  char password_key[50]; // [esp+10h] [ebp-74h] BYREF
  char password[50]; // [esp+42h] [ebp-42h] BYREF
  char *v8; // [esp+74h] [ebp-10h]
  signed int v9; // [esp+78h] [ebp-Ch]
  int i; // [esp+7Ch] [ebp-8h]

  __main();
  i = 0;
  v9 = 0;
  printf("Enter Password: ");
  scanf("%s", password);
  if ( strlen(password) > 9 && password[5] == 46 )
  {
    printf("Enter password key: ");
    scanf("%s", password_key);
    while ( 1 )
    {
      v4 = i++;
      if ( !password[v4] )
        break;
      ++v9;
    }
    v8 = (char *)malloc(4 * v9);
    srand(v9);
    for ( i = 0; i < v9; ++i )
    {
      v5 = &v8[4 * i];
      *(_DWORD *)v5 = rand() % v9 + v9 / -2;
    }
    magic(password, v8);
    if ( !strcmp(mstring, password_key) )
    {
      printf("Nice!!");
      result = 0;
    }
    else
    {
      printf("Wrong");
      result = 1;
    }
  }
  else
  {
    printf("Wrong");
    result = 1;
  }
  return result;
}
```  

Việc ta cần làm là khiến cho chương trình in ra dòng chữ `Nice!!`. Vậy đầu tiên, ta cần pass được block lệnh if sau:  
```c
  if ( strlen(password) > 9 && password[5] == '.' )
  {
    // something
  }
```  

Ở block lệnh này, `password` của chúng ta phải dài hơn 9 ký tự, đồng thời ký tự thứ `6` phải là dấu chấm `.`.



