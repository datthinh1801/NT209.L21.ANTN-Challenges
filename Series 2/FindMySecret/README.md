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

Ở đây, thread này sẽ thực thi hàm tại địa chỉ `StartAddress` và chứa địa chỉ bắt đầu của 1 hàm nào đó. Bên cạnh đó, việc `ArgList` được truyền vào ô nhớ tại địa chỉ `esp + 8` đồng nghĩ với việc `ArgList` chính là tham số thứ nhất của hàm mà `StartAddress` đang trỏ tới.  
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

- Khi giá trị truyền vào bằng `2`: chương trình sẽ thực hiện việc tính toán với 1 số lần nhất định.  
  > Để biết được hàm `mul_time_3_8()` làm gì thông qua pseudocode thì rất khó, do đó chúng ta sẽ xem assembly của đoạn này.  
  > ![image](https://user-images.githubusercontent.com/44528004/121792438-d46e0f80-cc1e-11eb-84c0-9962853605c3.png)  
  > ![image](https://user-images.githubusercontent.com/44528004/121792450-fb2c4600-cc1e-11eb-8199-d0b6d1f79c87.png)  
  > Ở đoạn code này, nếu giá trị truyền vào (`ArgList`) bằng `2` thì chương trình sẽ tiếp tục kiểm tra xem giá trị tại ô nhớ `ebp + ArgList + 2` có bằng `0` hay không. Nếu không thì sẽ thực thi hàm `mul_time_3_8()` sau đó giảm giá trị đó đi `1`.  
  > 
  > Khi chúng ta xem lại phần khai báo ở hàm `main` thì thấy được rằng, giá trị tại ô nhớ `ebp + ArgList + 2` chính là hằng số `5` vì hằng số được lưu tại địa chỉ cao hơn `ArgList` 2 bytes.  
  > ```
  > ArgList= word ptr -10h
  > const_5= word ptr -0Eh
  > ```  
  > Bên canh đó, sau khi thực thi `thread` thì `const_5` sẽ được gán bằng `5`, và `ArgList` sẽ được gán bằng `2`, vì vậy, số lần lặp của chúng ta khi thực thi hàm `mul_time_3_8()` sẽ là `5`.  
  > ![image](https://user-images.githubusercontent.com/44528004/121792513-fddb6b00-cc1f-11eb-87e1-923810254d5e.png)  
  > Hàm `mul_time_3_8()` sẽ thực thi câu lệnh sau:  
  > ```c
  > void mul_time_3_8()
  > {
  > calculated_from_time = double_3_8 * calculated_from_time * (1.0 - calculated_from_time);
  > }
  > ```


Quay lại với hàm `main()`.  

![image](https://user-images.githubusercontent.com/44528004/121792527-324f2700-cc20-11eb-8e48-e87a57dce3f2.png)  

Ở block lệnh trước khi gán `const_5` bằng `5` và `ArgList` bằng `2`, chương trình sẽ lấy thời gian hiện tại, chia lấy phần dư cho 50, sau đó chi cho 50 và lấy thương.  
```c
int cal_time_mod_50()
{
  int result; // eax

  do
    result = time(0) % 50;
  while ( 0.0 == (double)((long double)result / 50.0) );
  calculated_from_time = (long double)result / 50.0;
  return result;
}
```  

Sau đó, giá trị `calculated_from_time` này sẽ được tính lại ngay sau khi `const_5 = 5` và `ArgList = 2` thông qua hàm `mul_time_3_8()`.  

Kế đến chương trình sẽ yêu cầu chúng ta nhập vào 1 con số nếu bằng `0` thì sẽ in ra console `Invalid Range...`. Còn nếu lớn hơn `0` thì sẽ tiếp tục.  

![image](https://user-images.githubusercontent.com/44528004/121792569-fd8f9f80-cc20-11eb-9341-9359fbc1ba71.png)  

Ở block lệnh này, chương trình sẽ gọi hàm `sub_401708()` trước khi gọi hàm `success()`.  

![image](https://user-images.githubusercontent.com/44528004/121792578-17c97d80-cc21-11eb-9ab7-0d9d91fd29f8.png)

Xem mã giả của hàm `sub_401708()`.  
```c
__int16 *__cdecl sub_401708(__int16 *a1)
{
  __int16 *result; // eax
  double v2; // [esp+10h] [ebp-8h]

  if ( !byte_406034 )
  {
    calculated_from_time = calculated_from_time * 10000.0;
    byte_406034 = 1;
  }
  v2 = (double)*a1;
  if ( calculated_from_time <= (long double)v2 || v2 <= calculated_from_time - 1.0 )
  {
    result = a1;
    *a1 = 0;
  }
  else
  {
    result = a1;
    *a1 = 1;
  }
  return result;
}
```  

Hàm này sẽ nhân `calculated_time` cho `10000` sau đó kiểm tra các điều kiện.  

Ở hàm `success`.  
```
.data:00403038 ref_to_success  dd offset success
```  

Chương trình sẽ kiểm tra xem `a1` có khác `0` hay không, nếu có thì chúng ta thành công. Vậy mục tiêu là làm cho giá trị `a1` bằng `1` ở hàm `sub_401708()` phía trên.  
```c
int __cdecl success(int a1)
{
  int result; // eax

  if ( a1 )
    result = puts("Success! You have completely reverse engineered and found the secret number!");
  else
    result = puts("Nope, you have not yet found the secret number.");
  return result;
}
```


