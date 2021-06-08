# CrackMe2
## Task
```
└─$ ./CrackMe2.exe
-----------------------------------
--           CrackMe 2           --
--      by github.com/dajoh      --
-----------------------------------

Enter password: 25151
Fail! You entered the wrong password.

Enter password: hello ?
Fail! You entered the wrong password.
```
> Tìm password.  

## Solution
Xem kiến trúc file.  
```
└─$ file CrackMe2.exe
CrackMe2.exe: PE32 executable (console) Intel 80386, for MS Windows
```

Mở IDA Pro 32 bit và xem pseudocode.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  unsigned __int8 v3; // al
  char *i; // ecx
  int v5; // eax
  char v7; // [esp-Ch] [ebp-110h]
  char v8; // [esp-8h] [ebp-10Ch]
  char v9; // [esp-8h] [ebp-10Ch]
  char v10; // [esp-4h] [ebp-108h]
  char Buffer[256]; // [esp+0h] [ebp-104h] BYREF

  sub_4010B0("-----------------------------------\n", Buffer[0]);
  sub_4010B0("--           CrackMe 2           --\n", v10);
  sub_4010B0("--      by github.com/dajoh      --\n", v8);
  sub_4010B0("-----------------------------------\n\n", v7);
  while ( 1 )
  {
    sub_401010((int)"Enter password: ", 0xFu, Buffer[0]);
    gets_s(Buffer, 0x100u);
    v3 = Buffer[0];
    for ( i = Buffer; *i; v3 = *i )
      *i++ = byte_4188F0[v3];
    v5 = strcmp(Buffer, "HfyhgrgfgrlmYlc579");
    if ( v5 )
      v5 = v5 < 0 ? -1 : 1;
    if ( !v5 )
      break;
    sub_401010((int)"Fail! You entered the wrong password.\n\n", 0xCu, Buffer[0]);
  }
  sub_401010((int)"Congratulations! You entered the correct password.\n\n", 0xAu, Buffer[0]);
  sub_401010((int)"Press enter to exit...", 8u, v9);
  sub_404D1E();
  return 0;
}
```  

Từ đoạn pseudocode trên, để chuỗi `Congratulations!` được in ra màn hình, chúng ta phải `break` được vòng lặp `while`. Để `break` được vòng lặp `while` thì chúng ta phải làm sao cho `Buffer == "HfyhgrgfgrlmYlc579"`.  

`Buffer` khi được so sánh với chuỗi `HfyhgrgfgrlmYlc579` là `Buffer` đã qua xử lý ở vòng lặp `for`. Ở vòng lặp `for` này, từng phần tử của `Buffer` sẽ được thay đổi như sau:  
- Lấy giá trị số nguyên của ký tự thứ `i`, gán vào `v3`
- Truy xuất tới phần tử thứ `v3` của mảng `byte_4188F0`
- Gán giá trị của phần tử thứ `v3` của mảng `byte_4188F0` vào `Buffer[i]`
- `i += `, xét phần tử kế tiếp cho đến khi gặp giá trị `null`

### Script exploit
```python
import string


# construct an array from 0x0 to 0x20
consts = [i for i in range(0x21)]
# construct an array from '!' to '/'
consts.extend(list(string.punctuation[:15]))
# construct an array from '9' to '0'
consts.extend(list(string.digits[::-1]))
# construct an array from ':' to '@'
consts.extend(list(string.punctuation[15:22]))
# construct an array from 'Z' to 'A'
consts.extend(list(string.ascii_uppercase[::-1]))
# construct an array from '[' to '`'
consts.extend(list(string.punctuation[22: 28]))
# construct an array from 'z' to 'a'
consts.extend(list(string.ascii_lowercase[::-1]))

print(consts)

s = "HfyhgrgfgrlmYlc579"
result = []

for c in s:
    result.append(chr(consts.index(c)))

print(''.join(result))
```  

Chạy script.  
```
└─$ python3 solution.py
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, '!', '"', '#', '$', '%', '&', "'", '(', ')', '*', '+', ',', '-', '.', '/', '9', '8', '7', '6', '5', '4', '3', '2', '1', '0', ':', ';', '<', '=', '>', '?', '@', 'Z', 'Y', 'X', 'W', 'V', 'U', 'T', 'S', 'R', 'Q', 'P', 'O', 'N', 'M', 'L', 'K', 'J', 'I', 'H', 'G', 'F', 'E', 'D', 'C', 'B', 'A', '[', '\\', ']', '^', '_', '`', 'z', 'y', 'x', 'w', 'v', 'u', 't', 's', 'r', 'q', 'p', 'o', 'n', 'm', 'l', 'k', 'j', 'i', 'h', 'g', 'f', 'e', 'd', 'c', 'b', 'a']
SubstitutionBox420
```  

### Kiểm tra kết quả
```
└─$ ./CrackMe2.exe
-----------------------------------
--           CrackMe 2           --
--      by github.com/dajoh      --
-----------------------------------

Enter password: SubstitutionBox420
Congratulations! You entered the correct password.

Press enter to exit...
```
> Thành công!
