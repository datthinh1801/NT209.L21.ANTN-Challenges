# racecars
## Task
```
└─$ ./racecars
Gimme what I want!
```
> Chưa rõ chúng ta sẽ phải làm gì!  

## Solution
Xem kiến trúc của file.  
```
└─$ file racecars
racecars: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=84eee6611847da3272d223e4129ccbc5febe4231, for GNU/Linux 3.2.0, with debug_info, not stripped
```  

Mở IDA Pro 64bit và xem pseudocode.  
```c
int __cdecl main(int argc, const char **filename, const char **envp)
{
  int start; // [rsp+18h] [rbp-8h]
  int end; // [rsp+1Ch] [rbp-4h]

  end = strlen(*filename) - 1;
  for ( start = end; (*filename)[start - 1] != 47; --start )
    ;
  while ( start < end )
  {
    if ( (*filename)[start] != (*filename)[end] )
    {
      puts("Gimme what I want!");
      exit(1);
    }
    --end;
    ++start;
  }
  puts("That's exactly what I wanted!");
  return 0;
}
```  

Sau quá trình debug thì ta biết được rằng giá trị truyền vào hàm main chính là `filename` (biến này đã được đổi tên để dễ theo dõi).  
![image](https://user-images.githubusercontent.com/44528004/120487109-e01c3380-c3df-11eb-843c-bb8a4e333752.png)  

Và hàm này thực hiện các việc sau:
- Trích xuất tên file thông qua việc lùi giá trị `start` cho đến khi ký tự phía trước `start` là dấu `/`.
- So sánh ký tự tại vị trí `start` và ký tự tại ví trí `end`, nếu khác nhau thì `exit(1)`; ngược lại, tăng biến `start` và giảm biến `end` để tiếp tục so sánh. Điều này tương đương với các coding problem về palindrome.

Vậy chúng ta chỉ cần đổi tên file thành 1 chuỗi có chẵn số ký tự (để khi còn 2 ký tự cuối cùng cần phải xét thì sau khi xét xong, `start` sẽ lớn hơn `end` và thoát khỏi vòng lặp `while`) và là 1 palindrome. Trong trường hợp này, chúng ta sẽ đổi tên file thành `rr`.  

### Kiểm tra kết quả
```
└─$ cp racecars ./rr
└─$ ./rr
That's exactly what I wanted!
```  
> Thành công!
