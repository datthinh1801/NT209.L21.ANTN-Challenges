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
0x00000000004010b6 <+182>:   mov    r15,rax
0x00000000004010b9 <+185>:   dec    r15
```  
sẽ thực hiện việc gán độ dài của `password` vừa nhập vào thanh ghi `r15` sau đó -1. Vì khi nhập vào 1 chuỗi, thì chuỗi sẽ chứa thêm 1 ký tự `NULL` (`\0`) ở cuối chuỗi nên độ dài sẽ dài hơn số ký tự thật sự được nhập là 1 ký tự.  

Khi sử dụng `pwndgb`, hầu hết thông tin cần thiết đều được hiện thị một cách đẹp đẽ.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/hello/hello_dec_r15.png)
> Hình trên được chụp sau khi chương trình thực thi xong câu lệnh `mov r15,rax`.
