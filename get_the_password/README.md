# get_the_password
## Task
```
└─$ ./crackme1.EXE
Enter password: 12345
Wrong password.
```
> Tìm password.  

## Solution
Xem kiến trúc của file.
```
└─$ file crackme1.EXE
crackme1.EXE: PE32 executable (console) Intel 80386, for MS Windows
```  

Mở IDA Pro 32bit và xem mã giả:  
```c
void __noreturn start()
{
  char v0; // cl
  int v1; // edx
  char *v2; // esi
  char v3; // al

  hConsoleInput = GetStdHandle(0xFFFFFFF6);
  hConsoleOutput = GetStdHandle(0xFFFFFFF5);
  WriteConsoleA(hConsoleOutput, aEnterPassword, 0x11u, 0, 0);
  ReadConsoleA(hConsoleInput, &unk_402010, 0x64u, &NumberOfCharsRead, 0);
  v0 = 0;
  v1 = 0;
  v2 = (char *)&unk_402010;
  while ( 1 )
  {
    while ( 1 )
    {
      v3 = *v2++;
      if ( v3 == 13 )
        goto LABEL_32;
      if ( ++v0 != 1 )
        break;
      ++v1;
      if ( (unsigned __int8)v3 <= 0x47u )
        --v1;
    }
    switch ( v0 )
    {
      case 2:
        ++v1;
        if ( v3 >= 109 )
          --v1;
        break;
      case 3:
        ++v1;
        if ( v3 != 86 )
          --v1;
        break;
      case 4:
        ++v1;
        if ( (unsigned __int8)v3 < 0x66u )
          --v1;
        break;
      case 5:
        ++v1;
        if ( (unsigned __int8)v3 > 0x33u )
          --v1;
        break;
      case 6:
        ++v1;
        if ( (unsigned __int8)v3 <= 0x79u )
          --v1;
        break;
      case 7:
        ++v1;
        if ( v3 < 56 )
          --v1;
        break;
      case 8:
        ++v1;
        if ( v3 >= 78 )
          --v1;
        break;
      case 9:
        ++v1;
        if ( v3 == 82 )
          --v1;
        break;
      default:
        ++v1;
        if ( v3 != 50 )
          --v1;
LABEL_32:
        if ( v1 == 10 )
          WriteConsoleA(hConsoleOutput, aPasswordIsCorr, 0x18u, 0, 0);
        else
          WriteConsoleA(hConsoleOutput, aWrongPassword, 0x10u, 0, 0);
        ExitProcess(0);
    }
  }
}
```  
### Phân tích từng đoạn
Chương trình này gồm 1 vòng lặp `while` lớn. Bên trong vòng lặp `while` này là 1 vòng lặp `while` khác và 1 câu lệnh `switch`.  
Bắt đầu với vòng lặp `while` nhỏ.  
```c
while ( 1 )
    {
      v3 = *v2++;
      if ( v3 == 13 )
        goto LABEL_32;
      if ( ++v0 != 1 )
        break;
      ++v1;
      // if v3 <= 'G'
      if ( (unsigned __int8)v3 <= 0x47u )       // 'G'
        --v1;
    }
```  

Đầu tiên, vòng lặp `while` này sẽ `break` khi `v0 != 1` hoặc khi `v3 == 13` (vì khi đó chương trình sẽ nhảy tới 1 block lệnh khác).  
Vì giá trị khởi tạo của `v0 = 0` nên khi bắt đầu chương trình, vòng lặp này sẽ lặp `2` lần, và ở những lần lặp sau đó (của vòng lặp `while` lớn), vòng lặp `while` nhỏ chỉ thực thi 1 lần.  
Sau mỗi lần lặp `v2` sẽ tăng 1 đơn vị tương đương với việc truy xuất ký tự tiếp theo của `password` mà ta vừa nhập.  

Tiếp theo, ở câu lệnh `switch`.  
```cc
switch ( v0 )
    {
      case 2:
        ++v1;
        if ( v3 >= 109 )                        // 'm'
          --v1;
        break;
      case 3:
        ++v1;
        if ( v3 != 86 )                         // 'V'
          --v1;
        break;
      case 4:
        ++v1;
        if ( (unsigned __int8)v3 < 0x66u )      // 'f'
          --v1;
        break;
      case 5:
        ++v1;
        if ( (unsigned __int8)v3 > 0x33u )      // '3'
          --v1;
        break;
      case 6:
        ++v1;
        if ( (unsigned __int8)v3 <= 0x79u )     // 'y'
          --v1;
        break;
      case 7:
        ++v1;
        if ( v3 < 56 )                          // '8'
          --v1;
        break;
      case 8:
        ++v1;
        if ( v3 >= 78 )                         // 'N'
          --v1;
        break;
      case 9:
        ++v1;
        if ( v3 == 82 )                         // 'R'
          --v1;
        break;
      default:
        ++v1;
        if ( v3 != 50 )                         // '2'
          --v1;
LABEL_32:
        if ( v1 == 10 )
          WriteConsoleA(hConsoleOutput, aPasswordIsCorr, 0x18u, 0, 0);
        else
          WriteConsoleA(hConsoleOutput, aWrongPassword, 0x10u, 0, 0);
        ExitProcess(0);
```

Ở câu lệnh `switch` này, ở các case, `v1` đều được cộng 1 nhưng sẽ bị trừ đi 1 nếu thỏa điều kiện tương ứng của từng case (*như đã ghi chú ở trên*).  
Có một điều đặc biệt là khi `v0` không thuộc `[2, 9]` thì các câu lệnh ở `LABEL_32` sẽ được thực thi. Các câu lệnh này sẽ kiểm tra `v1 == 10`, nếu đúng thì sẽ thành công.
> Vậy việc cần làm là nhập 1 `password` có độ dài là `10` vì khi xét đến ký tự thứ 10, `v1` sẽ bằng 10 và block lệnh `default` sẽ được thực thi. Đồng thời mỗi ký tự này đều không thỏa các điều kiện mà sẽ dẫn đến việc `--v1`.  

### Tổng hợp phân tích
```c
s[0] > 'G'  => chọn 'Z'
s[1] < 'm'  => chọn 'a'
s[2] == 'V' => 'V'
s[3] >= 'f' => chọn 'f'
s[4] <= '3' => chọn '3'
s[5] > 'y'  => chọn 'z'
s[6] > '8'  => chọn '9'
s[7] < 'N'  => chọn 'A'
s[8] != 'R' => chọn 'A'
s[9] == '2' => '2'
```

## Kiểm tra kết quả
```
└─$ ./crackme1.EXE
Enter password: ZaVf3z9AA2
Password is correct! ;)
```
