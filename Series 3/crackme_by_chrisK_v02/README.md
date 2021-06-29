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
  signed int password_len; // [esp+78h] [ebp-Ch]
  int i; // [esp+7Ch] [ebp-8h]

  __main();
  i = 0;
  password_len = 0;
  printf("Enter Password: ");
  scanf("%s", password);
  if ( strlen(password) > 9 && password[5] == '.' )
  {
    printf("Enter password key: ");
    scanf("%s", password_key);
    while ( 1 )
    {
      v4 = i++;
      if ( !password[v4] )
        break;
      ++password_len;
    }
    v8 = (char *)malloc(4 * password_len);
    srand(password_len);
    for ( i = 0; i < password_len; ++i )
    {
      v5 = &v8[4 * i];
      *(_DWORD *)v5 = rand() % password_len + password_len / -2;
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

Ở block lệnh này, `password` của chúng ta phải dài hơn 9 ký tự, đồng thời ký tự thứ `6` phải là dấu chấm `.`. Vậy ta chọn `password` là `aaaaa.aaaa`, đồng thời `password_key` sẽ là 1 chuỗi bất kỳ.  

Tiếp theo, để in ra được `Nice!!`, chúng ta cần pass block lệnh:  
```c
if ( !strcmp(mstring, password_key) )
    {
      printf("Nice!!");
      result = 0;
    }
```  

Vậy điều kiện cần là tìm `password_key` bằng với `mstring`.  

Khi trace ngược lên, `password` sẽ được đưa vào hàm `magic()`. Xem pseudocode hàm `magic()`.  
```c
char *__cdecl magic(int a1, int a2)
{
  int v2; // eax
  char *result; // eax
  int i; // [esp+8h] [ebp-Ch]
  int v5; // [esp+Ch] [ebp-8h]
  int j; // [esp+Ch] [ebp-8h]

  v5 = 0;
  for ( i = 0; ; ++i )
  {
    v2 = v5++;
    if ( !*(_BYTE *)(v2 + a1) )
      break;
  }
  for ( j = 0; j < i; ++j )
    mstring[j] += *(_BYTE *)(4 * j + a2) + *(_BYTE *)(j + a1);
  result = &mstring[j];
  mstring[j] = 0;
  return result;
}
```  

Khi debug thì ta sẽ thấy được rằng, với `password == aaaaa.aaaa` thì sau các bước biến đổi, `mstring` sẽ có giá trị sau:  
![image](https://user-images.githubusercontent.com/44528004/123722471-a2151100-d8b2-11eb-8592-74e6072fd06c.png)  

Vậy ta thử ngay `password_key` bằng chuỗi trên.  

### Kiểm tra kết quả
```
└─$ ./crackme_by_chrisK_v02.exe
Enter Password: aaaaa.aaaa
Enter password key: ]e^`c/^^be
Nice!!
```
> Thành công.




