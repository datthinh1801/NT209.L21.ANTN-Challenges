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
      alter_v0();
      if ( v0 == 3 )
      {
        process_username();
        last_check_and_print_success();
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

Ở đây, ta có thể thấy được là khi `v0 == 3`, chương trình sẽ thực thi 2 hàm `process_username()` và `last_check_and_print_success()`, và theo dự đoán thì 2 hàm này sẽ thực hiện việc gì đó và sẽ in ra câu chúc mừng.  
Vậy để chương trình có thể thực thi 2 hàm này thì hàm `alter_v0()` phải trả về `3`.  

Tiến hành phân tích hàm `alter_v0()`.  

```c
int alter_v0()
{
  int *arr_serial; // esi
  __int16 v1; // cx
  unsigned int v2; // ebx
  int group_of_4_letters_from_serial; // eax

  arr_serial = (int *)&serial;
  v1 = 0;
  v2 = -16777216;
  while ( 1 )
  {
    group_of_4_letters_from_serial = *arr_serial;
    if ( !(unsigned __int8)*arr_serial )
      break;
    if ( (group_of_4_letters_from_serial & 0xFF000000) == 0 )
    {
      do
      {
        ++HIBYTE(v1);
        v2 >>= 8;
      }
      while ( (group_of_4_letters_from_serial & v2) == 0 );
      return group_of_4_letters_from_serial;
    }
    LOBYTE(v1) = v1 + 1;
    ++arr_serial;
  }
  return group_of_4_letters_from_serial;
}
```  

Điều ta cần chú ý là biến `v1` vì biến này là 16 bits thấp của thanh ghi `ecx` (là `v0` ở hàm `sub_4010F9()`). Vậy chúng ta phải làm sao để `v1 = 3` khi hàm kết thúc.  

Ở đây, ta thấy được là `v1` sẽ được cộng 1 vào byte thấp khi `(group_of_4_letters_from_serial & 0xFF000000) != 0`. Sau khoảng 30p debug thì em biết được là chương trình này sẽ lưu theo dạng **little endian**, nghĩa là khi chúng ta nhập `abcd`, thì giá trị được lưu sẽ là `dcba`.  
Vậy để `(group_of_4_letters_from_serial & 0xFF000000) != 0` thì `serial` của chúng ta phải là 1 chuỗi 12 ký tự để khi việc so sánh này diễn ra thì chúng ta luôn có byte thứ 4 khác `00` và dẫn đến kết quả của phép `AND` khác 0.

Sau khi làm cho `v0 == 3`, chúng ta tiếp tục phân tích hàm `process_username()`.
```c
int process_username()
{
  _BYTE *v0; // edi
  int v1; // ecx
  bool v2; // zf
  unsigned __int8 v3; // cl
  int v4; // ebx
  unsigned __int8 *username_iterator; // esi
  unsigned __int8 cur_username_letter; // al
  int v7; // ebx
  int result; // eax
  int *v9; // esi
  int v10; // ecx

  v0 = &username;
  v1 = 255;
  do
  {
    if ( !v1 )
      break;
    v2 = *v0++ == 0;
    --v1;
  }
  while ( !v2 );


  v3 = -(char)v1 - 2;
  v4 = 0;
  username_iterator = (unsigned __int8 *)&username;
  do
  {
    cur_username_letter = *username_iterator++;
    v4 += (unsigned __int16)(v3 * cur_username_letter);
    ++v0;
    --v3;
  }
  while ( v3 );
  v7 = ~(v4 ^ 0x13131313) ^ 0x1234ABCD;
  result = v7 & 0xF0F0F0F;
  dword_4020A0 = v7 & 0xF0F0F0F;
  dword_4020A4 = (v7 & 0xF0F0F0F0) >> 4;
  v9 = &dword_4020A0;
  v10 = 8;
  do
  {
    if ( *(_BYTE *)v9 > 9u )
      *(_BYTE *)v9 += 55;
    else
      *(_BYTE *)v9 |= 0x30u;
    v9 = (int *)((char *)v9 + 1);
    --v10;
  }
  while ( v10 );
  return result;
}
```
