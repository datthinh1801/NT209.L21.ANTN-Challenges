# NASM
## Task
> Tìm passwod:
```
└─$ ./nasm
Enter your password: hello123
Wrong!
```

## Solution
Trước tiên, em dùng lệnh `file` trên Linux để xem file này là file thực thi 32bit hay 64bit.
```
└─$ file nasm
nasm: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, not stripped
```
Từ kết quả trên, em mở IDA Pro 64bit để phân tích chương trình.

Đầu tiên, em thử xem IDA có thể show cho chúng ta chuỗi nào khả nghi hay không.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/nasm/nasm_secret.png)  

`supersecret` ? Có vẻ đây là password mà ta cần tìm.  
Em tiến hành test thử chuỗi này.  
```
└─$ ./nasm
Enter your password: supersecret
Correct!
```
> Chính xác! Vậy password là `supersecret`.
