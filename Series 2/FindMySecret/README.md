# FindMySecret
## Task
```
└─$ ./FindMySecret.exe
Enter the secret number9999
Nope, you have not yet found the secret number.

Enter the secret number1019
Nope, you have not yet found the secret number.
```
> Tìm secret number.  

## Solution
Xem kiến trúc file.  
```
└─$ file FindMySecret.exe
FindMySecret.exe: PE32 executable (console) Intel 80386 (stripped to external PDB), for MS Windows
```  

Mở IDA Pro 32bit.  

Challenge này là challenge có sử dụng thread vì chúng ta có thể dễ dàng nhìn thấy `beginthread` ở hình bên dưới.  

![image](https://user-images.githubusercontent.com/44528004/121792168-c79bec80-cc1b-11eb-9bc4-d2f864e54153.png)  

> Đa phần các tên biến và hàm đều đã được đổi tên để dễ theo dõi.  

Ở đây, thread này sẽ thực thi hàm tại địa chỉ `StartAddress` và chứa địa chỉ bắt đầu của 1 hàm nào đó.  
```
.data:00403028 StartAddress    dd offset sub_401626
```  

Xem pseudocode của hàm `sub_401626`.  
```c
void __cdecl __noreturn sub_401626(_WORD *a1)
{
  void (__cdecl *v1)(int (__cdecl *)(int, const char **, const char **)); // [esp+1Ch] [ebp-Ch]

  while ( 1 )
  {
    while ( !*a1 )
      ;
    if ( *a1 == 1 )
    {
      v1 = (void (__cdecl *)(int (__cdecl *)(int, const char **, const char **)))debug_checker;
      debug_checker(StartAddress);
      v1(off_40302C);
      v1((int (__cdecl *)(int, const char **, const char **))ref_to_mul_time_3_8);
      v1((int (__cdecl *)(int, const char **, const char **))ref_to_cal_time_mod_50);
      v1((int (__cdecl *)(int, const char **, const char **))ref_to_success);
      *a1 = 0;
    }
    else if ( *a1 == 2 )
    {
      while ( a1[1] )
      {
        ref_to_mul_time_3_8();
        --a1[1];
      }
      *a1 = 0;
    }
  }
}
```  

Ở hàm này, chương trình sẽ kiểm tra giá trị được truyền vào qua các điều kiện tương ứng:  
- Khi giá trì truyền vào bằng `0`: Chương trình sẽ đợi và không làm gì hết (infinite loop).  
  > ```c
  > while ( !*a1 )
  >   ;
  > ```  

- Khi giá trị truyền vào bằng `1`: Chương trình sẽ kiểm tra xem các hàm `mul_time_3_8`, `cal_time_mod_50`, và `success` có đang được debug hay không, nếu có thì sẽ thoát chương trình.  
  > ```
  > .data:00403024 debug_checker   dd offset sub_4015E8
  > ```  
  > Vì `debug_checker` trỏ tới hàm `sub_4015E8` và hàm này sẽ kiểm tra từng câu lệnh xem câu lệnh đó có đang được debug hay không (nếu có thì sẽ có byte biểu diễn là `0xCC`) cho tới khi gặp câu lệnh `ret` (có byte biểu diễn là `0xC3`).  
  > ```c
  > int __cdecl sub_4015E8(int a1)
  > {
  > int result; // eax
  > int v2; // [esp+1Ch] [ebp-Ch]
  > 
  > v2 = 0;
  > do
  > {
  > if ( *(_BYTE *)(v2 + a1) == 0xCC )
  >   exit(-20);
  > result = *(unsigned __int8 *)(++v2 + a1);
  > }
  > while ( (_BYTE)result != 0xC3 );
  > return result;
  > }
  > ```  
  > Từ [trang này](https://eli.thegreenplace.net/2011/01/27/how-debuggers-work-part-2-breakpoints), ta biết được là `0xCC` sẽ là byte encode cho debug exception handler nên chương trình sẽ tạm dừng và truyền control cho debugger.
  > ![image](https://user-images.githubusercontent.com/44528004/121792345-cec3fa00-cc1d-11eb-847d-847822d54981.png)  




