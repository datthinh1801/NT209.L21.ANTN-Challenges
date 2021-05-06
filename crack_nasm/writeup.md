Đầu tiên, em chạy thử file thực thi để xem chương trình này làm gì.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/crack_nasm/crack_nasm_try.png)  

Thì ở đây, rõ ràng là chương trình sẽ yêu cầu user nhập vào một chuỗi gọi là `flag`, sau đó chuỗi vừa được nhập này sẽ được so sánh với một chuỗi nào đó trong chương trình (có thể là chuỗi hằng, hoặc được tính toán thông qua một số quy luật nào đó).  

Tiếp theo, để biết được các quy luật mà chương trình sử dụng để so sánh, em tiến hành mở file bằng công cụ **IDA Pro**. Tuy nhiên, trước tiên, em dùng câu lệnh `file` trên Linux để kiểm tra xem file thực thi này được compile bởi một máy có kiến trúc gì (32bit hay 64bit).
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/crack_nasm/crach_nasm_file_command.png)  
Từ kết quả trên, em biết được file này là file 32bit, và em bắt đầu chạy **IDA Pro** phiên bản 32bit để xem hợp ngữ của chương trình.

Trên **IDA**, hợp ngữ của chương trình như sau:  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/crack_nasm/crach_nasm_ida.png)

Dễ dàng nhận thấy những ký tự rất *kỳ lạ* kia là `S3CrE+Fl4G!`. Em liền test thử chuỗi này.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/crack_nasm/crack_nasm_correct.png)  
> Chuỗi này chính là `flag` bí mật. Vậy kết quả là `S3CrE+Fl4G!`.
