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
- Với số lần shift phải vừa tìm được, là biến `v5` trong pseudocode, truy suất đến phần tử thứ `v5` của mảng `a` và kiểm tra xem giá trị của phần tử đó có khác 0 hay không. Nếu khác 0 thì sẽ tăng `v7` lên 1 đơn vị, tính lại `v4` bằng `v4 ^ v8` (với `v8 = a[v5]`). Tiếp theo là gán `v6 = 1` và nhảy đến `LABEL_9`.  
- Ở `LABEL_9`, nếu số lần shift phải, `v5`, bằng `0`, thì sẽ nhảy đến `LABEL_12` để kiểm tra điều kiện.  
- Ở `LABEL_12`, nếu lần lặp hiện tại `< 2`, nghĩa là 2 lần lặp đầu tiên, thì chương trình sẽ không in `Try again!` ra màn hình. Nếu `iteration > 1` thì chương trình sẽ kiểm tra biến `v10`. Nếu `v10` khác 0 thì chương trình sẽ in `Try again!` ra màn hình. Và biến `v10` này bằng `0` khi `v7 > 1`.  

#### Khi `iteration > 1`
Khi `iteration > 1`, nếu chương trình đã tìm được số lần shift phải để lấy được bit `1` cao nhất của giá trị nhập `v4`, giá trị `a[v5] == 0` thì chương trình sẽ in ra `Try again!` vì giá trị `v7` không được tăng thêm `1` hai lần.  

Một trường hợp khác là khi `a[v5] != 0`, `v7` được tăng thêm 1 đơn vị và sau đó `v4` được tính lại bằng `v4 ^ v8`. Với giá vị `v4` mới này, chương trình sẽ tiếp tục tìm bit `1` ***cao nhất có thể*** của `v4`. Khi tìm được `v5` thỏa `v4` mới, chương trình sẽ tiếp tục kiểm tra `a[v5] == 5 ?`, lúc này, nếu `a[v5] == 0` thì `v7` cũng chỉ mang giá trị là `1` và chương trình vẫn sẽ in `Try again!` ra màn hình.  

#### Kết luận
Ở 2 lần lặp đầu tiên, chúng ta cần nhập 2 số sao cho tổng của chúng bằng `1073840184`. Ở các lần lặp sau, cần nhập một giá trị `x` sao cho số lần shift phải của `x` phải bằng vị trí mà 1 trong 2 giá trị trước đã được gán vào mảng `a`, và `x` `XOR` với giá trị đó phải có kết quả là một con số `y` sao cho số lần shift phải của `y` bằng vị trí của giá trị nhập còn lại.  
> Đọc có vẻ khó hiểu nên chúng ta sẽ phân tích với giá trị cụ thể.  

#### Giải thích với giá trị cụ thể
Do mình đã giải được bài này nên mình sẽ dùng các giá trị input hợp lệ để phân tích. Solution là:  
```
└─$ ./a.out
1073840128
56
1073840184
1073840184
1073840184
Congrats!
```  

##### Lần lặp 1 (`iteration == 0`)
| Biến | Giá trị |
|---|---|
| `v4` | `1073840128` |
| Biểu diễn nhị phân của `v4` | `1000000000000011000000000000000` |
| Vị trí bit `1` cao nhất | `31` |
| `v5` | `30` |
| `a[30]` ban đầu | `0` |
| `v8` | `0` |
| `v7` | `0` |
| `a[30]` được gán bằng `v4` | `1073840128` |  


##### Lần lặp 2 (`iteration == 1`)
| Biến | Giá trị |
|---|---|
| `v4` | `56` |
| Biểu diễn nhị phân của `v4` | `111000` |
| Vị trí bit `1` cao nhất | `6` |
| `v5` | `5` |
| `a[5]` ban đầu | `0` |
| `v8` | `0` |
| `v7` | `0` |
| `a[5]` được gán bằng `v4` | `56` |  

##### Lần lặp 3 (`iteration == 2`)
###### Khi `v4 == 1073840184`
| Biến | Giá trị | Ghi chú |
|---|---|---|
| `v4` | `1073840184` | Giá trị nhập |
| Biểu diễn nhị phân của `v4` | `1000000000000011000000000111000` | |
| Vị trí bit `1` cao nhất | `31` | |
| `v5` | `30` | |
| `a[30]` ban đầu | `1073840128` | `a[30] == 1073840128` vì giá trị này được gán ở lần lặp đầu tiên |
| `v8` | `1073840128` | `v8 = a[v5] = a[30]` |
| `v7` | `1` | Vì `a[30] != 0` nên `v8 != 0` nên `v7 += 1` |
| `v4` sau khi `XOR` | `56` | `v4 = v4 ^ v8 = 1073840184 ^ 1073840128 = 56` |
| `v6` | `1` |  

Lúc này chương trình sẽ thực thi câu lệnh `goto LABEL_9` và tiếp tục giảm dần `v5` cho đến khi `v5` bằng `0`.  

###### Khi `v4 == 56`
| Biến | Giá trị | Ghi chú |
|---|---|---|
| `v4` | `56` | `v4` sau khi được `XOR` với `v8` |
| Biểu diễn nhị phân của `v4` | `111000` | |
| Vị trí bit `1` cao nhất | `6` | |
| `v5` | `5` | |
| `a[5]` ban đầu | `56` | `a[5] == 56` vì giá trị này được gán ở lần lặp thứ 2 |
| `v8` | `56` | `v8 = a[v5] = a[5]` |
| `v7` | `2` | Vì `a[5] != 0` nên `v8 != 0` nên `v7 += 1` |
| `v4` sau khi `XOR` | `0` | `v4 = v4 ^ v8 = 56 ^ 56 = 0` |
| `v6` | `1` |  

Lúc này, chương trình sẽ nhảy đến `LABEL_9` và giảm dần `v5`. Khi `v5` bằng `0`, chương trình nhảy đến `LABEL_12` và kiểm tra điều kiện.  

Tại đây, `v10` sẽ bằng `0` vì `v7 == 2` nên `v7 <= 1` sai. Do đó câu lệnh `if (v10 && iteration > 1)` sẽ sai và vòng lặp sẽ tiếp tục mà không in ra màn hình `Try again!`.  
> Ở các lần lặp 4 và 5 (`iteration == 3` và `iteration == 4`), tất cả các bước và giá trị điều tương tự ở lần lặp 3 (`iteration == 2`). Bên cạnh đó, với các giá trị nhập như trên thì từ lần lặp 3 trở đi, mảng `a` sẽ không được gán thêm các phần từ nào vì ngay sau khi tăng `v7`, chương trình sẽ nhảy đến `LABEL_9` và sau đó là nhảy đến `LABEL_12` mà không thực thi câu lệnh `a[(int)v5] = v4` như ở 2 lần lặp đầu tiên.   

Lúc này, vòng lặp `do ... while (iteration != 5)` sẽ kết thúc.  

Sau đó, chương trình sẽ tính tổng tất cả các giá trị của các phần tử của `a`. Và với các giá trị nhập như trên thì mảng `a` lúc này có `a[5] == 56`,`a[30] == 1073840128` và các vị trí còn lại có giá trị bằng `0`. Do vậy, tổng của các phần tử của mảng `a` bằng `1073840128 + 56 = 1073840184` và thỏa điều kiện để in `Congrats!` ra màn hình.
