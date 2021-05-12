# hello
## Task
```
└─$ ./hello
Please enter your name: thinh
Hello thinh
Enter your Password: 12345
```
> Nhập tên và password thỏa một số yêu cầu gì đó do chương trình tính toán từ tên được nhập.  

## Solution
Kiểm tra kiến trúc của file.
```
└─$ file hello
hello: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
```
Sau khi mở IDA Pro 64bit, em đọc sơ code assembly thì thấy rằng sử dụng công cụ `pwndbg` sẽ dễ theo dõi flow của chương trình hơn, vì khi debug bằng `pwngdb` em sẽ đồng thời xem được code assembly, stack frame, giá trị các thanh ghi ngay sau khi câu lệnh được thực thi.  

Code assembly của block `_start`:  

![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/hello/hello_start.png)  
> Tại thời điểm này, các I/O operations đã hoàn tất.  
> `name` là `thinh` và `password` là `12345`.

Ngay sau khi nhập xong `password`,  
```
 ► 0x00000000004010b6 <+182>:   mov    r15,rax
   0x00000000004010b9 <+185>:   dec    r15
```  
sẽ thực hiện việc gán độ dài của `password` vừa nhập vào thanh ghi `r15` sau đó `- 1`. Vì khi nhập vào 1 chuỗi, thì chuỗi sẽ chứa thêm 1 ký tự `NULL` (`\0`) ở cuối chuỗi nên độ dài sẽ dài hơn số ký tự thật sự được nhập là 1 ký tự.  

Khi sử dụng `pwndgb`, hầu hết thông tin cần thiết đều được hiển thị một cách đẹp đẽ.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/hello/hello_dec_r15.png)
> Hình trên được chụp sau khi chương trình thực thi xong câu lệnh `mov r15,rax`.  

Có thể thấy rằng `r15 = 6` ngay sau khi `mov r15, rax` được thực thi.  

Ở 2 câu lệnh tiếp theo:  
```
   0x4010bc <_start.l1>       mov    r14, r15
 ► 0x4010bf <_start.l1+3>     add    r14, 5
```
Giá trị mới của `r15` (số lượng ký tự của `password`) sẽ được lưu vào `r14`. Sau đó `r14` được cộng thêm 5.  

Tiếp theo:
```
0x4010c3 <_start.l1+7>     mov    al, byte ptr [r14 + 0x402094]
```
Giá trị tại vùng nhớ `[r14 + 0x402094]` sẽ được gán vào `al`.  
Khi xem giá trị được lưu tại `0x402094`, ta có thể thấy được đây là địa chỉ của chuỗi `"Hello "` cộng với `name` mà ta đã nhập. Vậy giá trị được gán vào `al` sẽ là ký tự thứ `5 + len(password)` (trong trường hợp là ký tự `h` cuối cùng).
```
pwndbg> x/10sb 0x402094
0x402094:       "Hello thinh\n"
```

Tiếp theo.
```
0x4010ca <_start.l1+14>    add    al, 5
0x4010cc <_start.l1+16>    cmp    al, byte ptr [r15 + 0x402073]
0x4010d3 <_start.l1+23>    jne    _start.wrong <_start.wrong>
```

`al` được cộng thêm 5 trước khi so sánh với giá trị tại ô nhớ `[r15 + 0x402073]`.
```
pwndbg> x/10s 0x402073
0x402073:       ""
0x402074:       "12345\n"
```
Từ `pwndbg`, `0x402073` chính là địa chỉ của byte liền trước `password` và `[r15 + 0x402073]` chính là ký tự cuối cùng của `password` (`5`).  
> Vì `0x402073` là địa chỉ của byte liền trước `password` nên `0x402073 + 1` sẽ là `0x402074`, tương đương địa chỉ của ký tự đầu tiên của `password`.  

Vậy giả sử `welcome = "Hello " + name`, thì điều kiện đúng sẽ là `(welcome[len(password) + 5] + 5) == password[len(password)]`.  

Khi thỏa điều kiện trên, các câu lệnh tiếp theo sẽ được thực thi:  
```
0x4010d5 <_start.l1+25>    dec    r15
0x4010d8 <_start.l1+28>    jne    _start.l1 <_start.l1>
```
`r15` sẽ giảm đi 1, và được so sánh xem `r15` có bằng 0 hay không. Điều này tương đương với đoạn pseudocode sau:
```
for i from len(password) to 0:
    if welcome[i + 5] + 5 != pass[i]:
        goto wrong
```

Vậy để không phải tính toán quá nhiều, em sẽ chọn `password` có độ dài bằng 1. Lúc này, chương trình chỉ phải so sánh 1 ký tự duy nhất của `password` này với `welcome[6]` (tương đương ký tự đầu tiên của `name`.  

Sau khi biết được flow của chương trình, em chọn `name=t`. Khi đó `'t' + 5 = 'y'`. Vậy `password=y`.

## Kiểm tra kết quả
```
└─$ ./hello
Please enter your name: t
Hello t
Enter your Password: y
Great H4x0r Skillz!
```
