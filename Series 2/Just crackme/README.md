# Just crackme
## Task
```
└─$ ./a.out
Enter your flag.
my flag
Try again
```
> Tìm flag.

## Solution
Xem kiến trúc file.  
```
└─$ file a.out
a.out: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=ed76b7e31e4e28ac60796e87722903fcffaf9af3, for GNU/Linux 3.2.0, not stripped
```

Mở IDA Pro 64bit và xem pseudocode.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  const char *src; // [rsp+8h] [rbp-E8h]
  char *s; // [rsp+10h] [rbp-E0h]
  _BYTE *timely_processed_base_string; // [rsp+18h] [rbp-D8h]
  char input_flag[200]; // [rsp+20h] [rbp-D0h] BYREF
  unsigned __int64 v8; // [rsp+E8h] [rbp-8h]

  v8 = __readfsqword(0x28u);
  if ( !malloc(0x46uLL) )
    exit(1);
  src = getenv("USER");
  s = (char *)malloc(0x12CuLL);
  if ( !s )
    exit(1);
  strcpy(&s[strlen(s)], "Wait, your name is");
  strcat(s, src);
  puts("Enter your flag.");
  __isoc99_scanf("%s", input_flag);
  timely_processed_base_string = (_BYTE *)compare((__int64)s);
  if ( (unsigned int)strcp(timely_processed_base_string, input_flag) )
    printf("Try again");
  else
    printf("Done.");
  return 0;
}
```

Sau khi debug thì chúng ta có được các thông tin sau:
1. Chương trình sẽ lấy tên user của máy sau đó sẽ nối vào chuỗi `Wait, your name is`.
    > Trong trường hợp này, user của mình là `datthinh` nên chuỗi sau khi nối sẽ là `Wait, your name isdatthinh` *(không có dấu cách giữa `is` và `datthinh`)*.
2. Tiếp theo, chuỗi này sẽ được đem đi xử lý phụ thuộc vào thời gian hiện tại.  
```c
__int64 __fastcall compare(__int64 a1)
{
  int i; // [rsp+18h] [rbp-28h]
  time_t timer; // [rsp+28h] [rbp-18h] BYREF
  struct tm *v4; // [rsp+30h] [rbp-10h]
  unsigned __int64 v5; // [rsp+38h] [rbp-8h]

  v5 = __readfsqword(0x28u);
  time(&timer);
  v4 = localtime(&timer);
  for ( i = 0; i < (int)stlen(a1); ++i )
    *(_BYTE *)(i + a1) ^= v4->tm_min >> v4->tm_mday;
  return a1;
}
```  
Xem đoạn code trên thì ta có thể thấy, từng ký tự của chuỗi sẽ được `XOR` với 1 giá trị được lấy từ phút hiện tại (`tm_min`) và theo mình đoán là ngày của tháng hiện tại (`tm_mday`).  

3. Sau đó, chuỗi vừa được thay đổi này sẽ được đem so sánh với chuỗi `flag` mà chúng ta nhập từ màn hình.  

Vậy ngay sau khi debug và có được chuỗi sau khi xử lý, chúng ta phải nhanh tay nhập chuỗi này vào chương trình thì mới thành công.  

## Kiểm tra kết quả
Debug để lấy chuỗi sau khi xử lý.  
![image](https://user-images.githubusercontent.com/44528004/120333862-52c2db80-c31a-11eb-8a97-411fcd218dd6.png)  

Nhập chuỗi để kiểm tra.  
```
└─$ ./a.out
Enter your flag.
Drzg?3j|fa3}r~v3z`wrgg{z}{
Done.
```
> Thành công!
