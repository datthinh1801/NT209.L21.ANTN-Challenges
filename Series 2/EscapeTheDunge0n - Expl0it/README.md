# EscapeTheDunge0n
## Task
![image](https://user-images.githubusercontent.com/44528004/120316045-30739280-c307-11eb-8c9b-7baecb7a8432.png)  

![image](https://user-images.githubusercontent.com/44528004/120316186-56993280-c307-11eb-96ae-044a9faa2d34.png)  

![image](https://user-images.githubusercontent.com/44528004/120316207-5b5de680-c307-11eb-9029-97aad44dc8f2.png)  

![image](https://user-images.githubusercontent.com/44528004/120316219-60bb3100-c307-11eb-8dfc-42e27f26d7e7.png)  

![image](https://user-images.githubusercontent.com/44528004/120316240-66b11200-c307-11eb-9016-44a6b4eee7d6.png)  

> Đây có vẻ là một mini-game yêu cầu chúng ta làm điều gì đó trong ngục tối và tẩu thoát thành công?  

## Solution
Xem kiến trúc của file.  
```
└─$ file CrackMe.exe
CrackMe.exe: PE32 executable (console) Intel 80386, for MS Windows
```  

Mở IDA Pro 32bit.  

Xem cửa sổ string để tìm chuỗi chúc mừng.  
Ở đây chúng ta có một chuỗi khá là hấp dẫn là:  
![image](https://user-images.githubusercontent.com/44528004/120317894-5ef26d00-c309-11eb-80c4-6565d60f9e29.png)  

Nhưng khi double click vào thì còn có một chuỗi khác hấp dẫn hơn:  
![image](https://user-images.githubusercontent.com/44528004/120319909-b691d800-c30b-11eb-850b-5672535bbb81.png)  

Chúng ta liền nhảy đến hàm sử dụng chuỗi này:  
![image](https://user-images.githubusercontent.com/44528004/120319955-ca3d3e80-c30b-11eb-9ac0-8a97c7a9d364.png)  

Khi dò ngược lên thì có thể tìm được cách để chương trình thực thi block lệnh này là chúng ta phải nhập theo thự tự sau: `1 -> 2 -> 1 -> 2`.  
> Việc dò này có thể được thực hiện dễ dàng trên graph view của IDA với các mũi tên liên kết giữa các block lệnh được tô màu xanh dương nhạt (như hình dưới).  
> ![image](https://user-images.githubusercontent.com/44528004/120321190-310f2780-c30d-11eb-9d28-96d6d0520c18.png)  

### Chạy thử chương trình

![image](https://user-images.githubusercontent.com/44528004/120321272-52701380-c30d-11eb-8fda-8aefc2fea2ae.png)

![image](https://user-images.githubusercontent.com/44528004/120321295-5865f480-c30d-11eb-81e5-7ba0ba5aee5d.png)

![image](https://user-images.githubusercontent.com/44528004/120321314-5d2aa880-c30d-11eb-9181-ebea7041fadc.png)  

![image](https://user-images.githubusercontent.com/44528004/120321328-6287f300-c30d-11eb-9937-c482ac730556.png)

![image](https://user-images.githubusercontent.com/44528004/120321347-674ca700-c30d-11eb-95e8-b21c0f680328.png)  

**Congratulation**. Vậy hướng đi đã chính xác, việc cần làm bây giờ là tìm ra `Key code`.  

Xem pseudocode ở block lệnh này trên IDA:  
```c
sub_401770(std::cout, "Key Code:\n\x1B[31m$ ");
        std::istream::operator>>(std::cin, &v29);
        if ( v29 == 788960 )
          MessageBoxW(0, L"Congratulations you won! You are a good cracker...\n @Expl0it", L"CONGRATULATIONS!!!", 0x40u);
        else
          MessageBoxW(0, L"You lost the game...Try again", L"YOU LOST!", 0x10u);
        return system("pause");
```  

Có thể thấy, để thành công, `v29` cần phải bằng `788960`. Vậy không chần chừ gì nữa, thử giá trị này ngay thôi.  

### Kiểm tra kết quả

![image](https://user-images.githubusercontent.com/44528004/120321583-af6bc980-c30d-11eb-9a93-5c7ca96f50e3.png)

> Thành công!
