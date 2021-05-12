# Es crack
## Task
```
> .\run.exe
Program 'run.exe' failed to run: The specified executable is not a valid application for this OS platform.
```
Mặc dù file extension là `.exe` nhưng khi thực thi trên Windows 10 thì lại bị lỗi như trên. Em chuyển sang thử chạy trên Linux.
```
└─$ ./run.exe
Segmentation fault
```
Hmmm, mặc dù extension là `.exe` nhưng lại chạy trên Linux, có vẻ như file này đã bị đổi extension. Nhưng không sao, vì câu hỏi bây giờ là tại sao lại bị segmentation fault. Nhờ sự trợ giúp của bạn bè thì em biết được là chương trình này là file lấy input từ tham số truyền vào từ command-line.
```
└─$ ./run.exe 1
Wrong!
```
> Việc cần làm có vẻ lại là tìm ra password bí mật và input đầu vào được truyền từ command-line.

## Solution
Xem kiến trúc của file.
```
└─$ file run.exe
run.exe: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, with debug_info, not stripped
```
Mở IDA Pro 32bit.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/Es%20crack/es_crack_ida.png)  

Đập vào mắt chính là biến `password`, em liền tiến hành xem chuỗi giá trị của biến này.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/Es%20crack/es_crack_password.png)  
> `P455W0rd` có thể chính là password mà mình cần tìm ?  

Em tiến hành test thử chuỗi này.  
```
└─$ ./run.exe P455W0rd
You Got This!
```
> Vậy password cần tìm là `P455W0rd`.
