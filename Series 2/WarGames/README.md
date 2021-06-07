# WarGames
## Task
```
└─$ ./WarGames
Use ./WarGames pass
└─$ ./WarGames 123415
Wrong Password !!!
```
> Tìm password  

## Solution
Xem kiến trúc của file.  
```
└─$ file WarGames
WarGames: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, BuildID[sha1]=9a0e6dfa0e34cb42a1d5524f94d26424fff8625e, for GNU/Linux 3.2.0, not stripped
```  

Mở IDA Pro 64bit và xem pseudocode.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int result; // eax
  int v4; // [rsp+18h] [rbp-28h]
  unsigned __int64 i; // [rsp+20h] [rbp-20h]
  _BYTE v6[9]; // [rsp+2Fh] [rbp-11h] BYREF
  unsigned __int64 v7; // [rsp+38h] [rbp-8h]

  v7 = __readfsqword(0x28u);
  if ( argc == 2 )
  {
    if ( j_strlen_ifunc(argv[1], argv, envp) == 9 )
    {
      qmemcpy(v6, "gssw#tpcz", sizeof(v6));
      v4 = 0;
      srandom(1983LL);
      for ( i = 0LL; i <= 8; ++i )
      {
        v6[i] -= (int)rand() % 5 + 1;
        if ( v6[i] != argv[1][i] )
        {
          v4 = 1;
          break;
        }
      }
      if ( v4 )
        puts("Wrong Password !!!");
      else
        puts("Congratulation !!!");
      result = 0;
    }
    else
    {
      puts("Wrong Password !!!");
      result = 0;
    }
  }
  else
  {
    puts("Use ./WarGames pass");
    result = 0;
  }
  return result;
}
```  

Từ đoạn pseudocode trên, để chuỗi `Congratulation !!!` được in ra màn hình thì biến `v4` phải bằng `0`.  

Để `v4 == 0` thì ở vòng lặp `for` ngay bên trên, các giá trị tại vị trí `i` của `v6` phải bằng các giá trị tại vị trí `i` của `password` để `v4` không được gán bằng `1`.  
Chuỗi `v6` này là chuỗi `gssw#tpcz` đã được thay đổi với 1 giá trị random. Tuy nhiên, hàm random này được seed 1 giá trị cố định `1983` nên giá trị được random ở các lần chạy khác nhau đều bằng nhau.  

Vậy việc chúng ta cần làm là debug chương trình để xem từng ký tự này là gì thì sẽ có được `password` tương ứng.  

---
Sau quá trình debug thì chuỗi `password` có được là `dont play`. Vì chuỗi này có dấu cách nhưng nếu chúng ta nhập `dont play` thì chương trình sẽ nhận đây là 2 tham số `dont` và `play`. Vậy để nhập được chuỗi này trên linux thì chúng ta phải dùng `\` operator để nối chuỗi có khoảng trắng.
> Vậy chuỗi cần nhập là `dont\ play`.  

### Kiểm tra kết quả
```
└─$ ./WarGames dont\ play
Congratulation !!!
```
> Thành công!
