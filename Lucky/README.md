# Lucky
## Task
```
└─$ ./lucky_nb
Lucky Numbers: 1234
Sorry :((
└─$ 34
34: command not found
└─$ ./lucky_nb
Lucky Numbers: 1 2 3
Sorry :((
└─$ 2 3
2: command not found
```
> Okay, việc cần làm là tìm ra 1 con số may mắn có tối đa 2 chữ số.  

## Solution
Kiểm tra kiến trúc của file.
```
└─$ file lucky_nb
lucky_nb: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, stripped
```

Mở IDA Pro 32bit.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/Lucky/lucky_ida.png)  

Có thể thấy từ các câu lệnh setup cho lệnh write, chương trình sẽ đọc 2 ký tự từ người dùng.  
```
mov     eax, 3
mov     ebx, 2          ; fd
mov     ecx, offset byte_804A024 ; addr
mov     edx, 2          ; len
int     80h             ; LINUX - sys_read
```

Ở các dòng lệnh tiếp theo:  
```
mov     al, ds:byte_804A024
sub     al, 30h ; '0'
mov     bl, byte ptr ds:unk_804A025
sub     bl, 30h ; '0'
adc     al, bl
```  
Ký tự thứ nhất sẽ được lưu vào thanh ghi `al` và ký tự thứ 2 sẽ được lưu vào thanh ghi `bl`. Sau đó, giá trị của `al` và `bl` sẽ được trừ đi `0x30`. Tiếp theo, `bl` sẽ được cộng vào `al`.  
Ở câu lệnh tiếp theo:
```
daa
```  
Google thì em biết được là câu lệnh này sẽ format lại định dạng khi biểu diễn các chữ số. Thí dụ số 1 sẽ được biểu diễn là `0001`, 2 là `0010`, và 9 là `1001`. Và câu lệnh `daa` sẽ format các chữ số ở dạng **packed BCD**. Ở định dạng này, 1 chữ số sẽ được biểu diễn bằng 4 bits, và 1 byte có thể biểu diễn được 2 chữ số. Và đặc biệt, các chữ số này luôn được biểu diễn bằng các bit thấp và không bị ảnh hưởng bởi endian của máy.  

Tiếp tục với các câu lệnh tiếp theo:
```
add     bl, 30h ; '0'
cmp     al, 16h
jnz     short sub_8049000
```  
Cộng `0x30` vào giá trị của thanh ghi `bl`.  
Sau đó so sánh giá trị của thanh ghi `al` với `0x16`. Nếu `al` không bằng `0x16` thì chương trình sẽ nhảy tới hàm `sub_8049000`.  
Hàm `sub_8049000` này sẽ in ra màn hình dòng chữ ***Sorry :((***. Vậy điều kiện đầu tiên mà chúng ta cần vượt qua là  
```
daa((so1 - 0x30) + (so2 - 0x30)) == 0x16
```

Tiếp theo:
```
cmp     bl, 38h ; '8'
jnz     sub_8049000
```
Chương trình sẽ so sánh chữ số thứ 2 với chữ số `8`, nếu không bằng `8` thì chương trình sẽ in ra ***Sorry :((***.  
Một lưu ý nho nhỏ là chữ số thứ 2 này, sau khi được trừ `0x30` đã được cộng lại `0x30` nên ở câu lệnh này, chúng ta đang so sánh giá trị ban đầu của chữ số thứ 2.  

Vậy chữ số thứ 2 là `8`.  
Từ đó chúng ta tính chữ số thứ nhất `daa((so1 - 0x30) + 0x8) = 0x16`. Theo hướng dẫn [này](https://www.tutorialspoint.com/daa-instruction-in-8085-microprocessor), thì giá trị `so1` cần tìm là `8` vì
```
daa((0x38 - 0x30) + 0x8) = daa(0x8 + 0x8) = daa(0xf)
```
Mà `0xf = 16` và lớn hơn `10` nên kết quả cuối cùng sẽ bằng `0xf + 0x6 = 0x16`.

> Vậy giá trị cần tìm là 88.

## Kiểm tra kết quả
```
└─$ ./lucky_nb
Lucky Numbers: 88
Good Job !
```
