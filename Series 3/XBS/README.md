# XBS
## Task
```
└─$ ./a.out
awefaw
Try again!
```
> Tìm chuỗi nhập.  

## Solution
Xem kiến trúc file.  
```
└─$ file a.out
a.out: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=e645bcadcc14569d7dda684c9fe11b20b1a40bff, for GNU/Linux 3.2.0, not stripped
```  

Mở IDA Pro 64bit và xem cửa sổ string để tìm chuỗi chúc mừng.  
![image](https://user-images.githubusercontent.com/44528004/123735007-992f3a00-d8c8-11eb-8cc1-f897de936df5.png)  

Nhảy đến hàm có tham chiếu tới chuỗi chúc mừng và xem pseudocode.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int iteration; // ebx
  int v4; // eax
  __int64 v5; // rcx
  char v6; // si
  int v7; // edx
  int v8; // er9
  bool v10; // cc
  _DWORD *v11; // rax
  int v12; // edx
  const char *v13; // rdi
  int v15; // [rsp+4h] [rbp-14h] BYREF
  unsigned __int64 v16; // [rsp+8h] [rbp-10h]

  iteration = 0;
  v16 = __readfsqword(0x28u);
  do
  {
    __isoc99_scanf(&unk_55D8A6AF0004, &v15, envp);
    v4 = v15;
    v5 = 30LL;
    v6 = 0;
    v7 = 0;
    while ( ((v4 >> v5) & 1) == 0 )
    {
LABEL_9:
      if ( v5-- == 0 )
      {
        if ( v6 )
          v15 = v4;
        goto LABEL_12;
      }
    }
    v8 = a[v5];
    if ( v8 )
    {
      ++v7;
      v4 ^= v8;
      v6 = 1;
      goto LABEL_9;
    }
    if ( v6 )
      v15 = v4;
    a[(int)v5] = v4;
LABEL_12:
    v10 = v7 <= 1;
    envp = (const char **)(unsigned int)(v7 - 1);
    if ( v10 && iteration > 1 )
    {
LABEL_18:
      v13 = "Try again!";
      goto LABEL_19;
    }
    ++iteration;
  }
  while ( iteration != 5 );
  v11 = a;
  v12 = 0;
  do
    v12 += *v11++;
  while ( v11 != &a[31] );
  v13 = "Congrats!";
  if ( v12 != 1073840184 )
    goto LABEL_18;
LABEL_19:
  puts(v13);
  return 0;
}
```  

Từ đoạn pseudocode trên, để chương trình in ra chuỗi `Congrats!`, `v12` phải bằng `1073840184` để chương trình không nhảy tới `LABEL_18` và in ra chuỗi `Try again!`.  

Để `v12 == 1073840184` thì tổng của các phần tử trong mảng `a` phải bằng `1073840184`.  

Bên cạnh đó, để chương trình không nhảy vào `LABEL_18` thì điều kiện `v10 && iteration > 1` phải sai. Vì là biến vòng lặp nên chúng ta không thể kiểm soát biến `iteration`. Và với điều kiện `iteration > 1` như trên thì chương trình sẽ không bao giờ in ra chuỗi `Try again!` ở 2 lần lặp đầu tiên.   

#### Tóm lược các đoạn pseudocode phía trên đoạn xét điều kiện quyết định
Sau quá trình debug, thì chúng ta biết được flow của chương trình như sau:  
- Với đầu vào là `v4`, chương trình sẽ tìm số lần shift phải sao cho lấy được bit `1` cao nhất của `v4`. Ví dụ, `v4 == 2` thì cần shift phải 1 lần để lấy bit `1` cao nhất của số `2`.  
- Với giá trị shift phải `v5`, một vài bước kiểm tra và tính toán sẽ diễn ra và sau đó, phần tử tại vị trí `v5` của mảng `a` sẽ được gán giá trị bằng `v4`.  

#### Tóm lược các bước kiểm tra và tính toán vừa được đề cập ở trên
- Với số lần shift phải vừa tìm được, là biến `v5` trong pseudocode, truy suất đến phần tử thứ `v5` của mảng `a` và kiểm tra xem giá trị của phần tử đó có khác 0 hay không. Nếu khác 0 thì sẽ tăng `v7` lên 1 đơn vị, tính lại `v4` bằng `v4 ^ v8` (với `v8` được khởi tạo bằng `0`, ở các lần lặp i > 0 thì `v8` sẽ bằng giá trị nhập của lần lặp trước. Tiếp theo là gán `v6 = 1` và nhảy đến `LABEL_9`.  
- Ở `LABEL_9`, nếu số lần shift phải, `v5`, bằng `0`, thì sẽ nhảy đến `LABEL_12` để kiểm tra điều kiện.  
- Ở `LABEL_12`, nếu lần lặp hiện tại `< 2`, nghĩa là 2 lần lặp đầu tiên, thì chương trình sẽ không in `Try again!` ra màn hình. Nếu `iteration > 1` thì chương trình sẽ kiểm tra biến `v10`. Nếu `v10` khác 0 thì chương trình sẽ in `Try again!` ra màn hình. Và biến `v10` này bằng `0` khi `v7 > 1`.  
