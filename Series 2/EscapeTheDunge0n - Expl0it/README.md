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

Khi dò ngược lên thì có thể tìm được cách để chương trình thực thi block lệnh này là `1 -> 2 -> 1 -> 2`.  
> Việc dò này có thể được thực hiện dễ dàng trên graph view của IDA với các mũi tên liên kết giữa các block lệnh được tô màu xanh dương nhạt (như hình dưới).  
> ![image](https://user-images.githubusercontent.com/44528004/120321190-310f2780-c30d-11eb-9d28-96d6d0520c18.png)
