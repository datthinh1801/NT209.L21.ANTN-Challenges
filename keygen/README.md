# keygen
## Task
```bash
└─$ ./Keygenme#1.exe
*******************************************
* Keygenme 1.0 Created by BlZbB           *
*******************************************


Enter Username : thinh
Enter Serial : 123435

Incorrect Serial !!!
```
> Tìm username và serial.

## Solution
Kiểm tra kiến trúc của file.  

```bash
└─$ file Keygenme#1.exe
Keygenme#1.exe: PE32 executable (console) Intel 80386, for MS Windows
```  

Mở IDA Pro 32bit và xem pseudocode.  
```c
void __noreturn start()
{
  int v0; // eax

  printf("*******************************************\n");
  printf("* Keygenme 1.0 Created by BlZbB           *\n");
  printf("*******************************************\n\n\n");
  v0 = sub_4010F9();
  ExitProcess(v0);
}
```  

Đầu tiên, chương trình sẽ in ra 3 dòng trên và sau đó sẽ gọi hàm `sub_4010F9()`.  

Xem pseudocode của `sub_4010F9()`.  

```c
int sub_4010F9()
{
  int v0; // ecx

  printf("Enter Username : ");
  if ( *(_BYTE *)gets(&username) )
  {
    printf("Enter Serial : ");
    if ( *(_BYTE *)gets(&serial) )
    {
      sub_4010C0();
      if ( v0 == 3 )
      {
        sub_401049();
        sub_401000();
      }
      else
      {
        printf("\nIncorrect Serial !!!\n");
      }
    }
    else
    {
      printf("\nYou didn't enter serial!!!\n");
    }
  }
  else
  {
    printf("\nYou didn't enter username!!!\n");
  }
  getc((FILE *)iob[0]._ptr);
  return 0;
}
```

Trước tiên, chương trình sẽ cho chúng ta nhập vào `username` sau đó kiểm tra xem chúng ta có nhập `username` hay không. Nếu có thì tiếp tục nhập `serial` và kiểm tra xem chúng ta có nhập `serial` hay không.  

Ở đây, ta có thể thấy được là khi `v0 == 3`, chương trình sẽ thực thi 2 hàm `sub_401049()` và `sub_401000()`, và có thể thấy được 2 hàm này sẽ thực hiện việc gì đó và sẽ in ra câu chúc mừng.  
Vậy để chương trình có thể thực thi 2 hàm này thì hàm `sub_4010C0()` phải trả về `3`.  

Tiến hành phân tích hàm `sub_4010C0()`.  

```c
int sub_4010C0()
{
  int *str_serial; // esi
  __int16 v1; // cx
  unsigned int v2; // ebx
  int result; // eax

  str_serial = (int *)&serial;
  v1 = 0;
  v2 = -16777216;
  while ( 1 )
  {
    result = *str_serial;
    if ( !(unsigned __int8)*str_serial )
      break;
    if ( (result & 0xFF000000) == 0 )
    {
      do
      {
        ++HIBYTE(v1);
        v2 >>= 8;
      }
      while ( (result & v2) == 0 );
      return result;
    }
    LOBYTE(v1) = v1 + 1;
    ++str_serial;
  }
  return result;
}
```
