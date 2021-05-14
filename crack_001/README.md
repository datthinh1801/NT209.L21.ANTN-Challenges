# crack_001
## Task
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/crack_001/crack_001.png)  

![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/crack_001/crack_001_test.png)
> Tìm `NAME` và `PASS` hợp lệ để `register`.  

## Solution
Kiểm tra kiến trúc của file.
```
└─$ file Creakme.exe
Creakme.exe: PE32 executable (GUI) Intel 80386, for MS Windows
```  

Mở IDA Pro 32bit.  

Vì đây là lần đầu dịch ngược một file thực thi có giao diện, nên em sẽ tiến hành xem từng hàm của chương trình để dự đoán flow.  

Hàm `start()`.  
```c
void __stdcall __noreturn start(int a1, int a2, int a3, int a4)
{
  INT_PTR v4; // eax

  hInstance = GetModuleHandleA(0);
  InitCommonControls();
  v4 = DialogBoxParamA(hInstance, (LPCSTR)0x3E8, 0, DialogFunc, 0);
  ExitProcess(v4);
}
```  

Theo em dự đoán thì hàm này sẽ load cửa sổ chương trình.  

Hàm `DialogFunc()`.  
```c
INT_PTR __stdcall DialogFunc(HWND hWnd, UINT a2, WPARAM a3, LPARAM a4)
{
  HICON v4; // eax
  WPARAM v5; // eax
  UINT v6; // eax

  ::hWnd = hWnd;
  switch ( a2 )
  {
    case 0x110u:
      v4 = LoadIconA(hInstance, (LPCSTR)0xC8);
      SendMessageA(hWnd, 0x80u, 1u, (LPARAM)v4);
      break;
    case 0x111u:
      v5 = a3;
      if ( a3 == 1001 )
        v5 = SendMessageA(hWnd, 0x10u, 0, 0);
      if ( v5 == 1004 )
      {
        GetDlgItemTextA(hWnd, 1003, String, 255);
        v6 = GetDlgItemTextA(hWnd, 1002, byte_403014, 255);

        if ( v6 < 3 )
        {
          MessageBoxA(hWnd, "Min 3 Char on Name!!!", ".:: DiS[IP] Programer ::.", 0);
        }
        else
        {
          NAME_len = v6;
          sub_4010FE();
        }
      }
      break;
    case 0x10u:
      EndDialog(hWnd, 0);
      break;
  }
  return 0;
}
```  
> Biến `NAME_len` là biến đã được đổi tên để tiện cho việc theo dõi.  

Có thể dự đoán được là hàm này sẽ đọc `NAME` và kiểm tra xem độ dài của `NAME` có nhỏ hơn 3 hay không. Nếu có thì sẽ in chuỗi thông báo `"Min 3 Char on Name!!!", ".:: DiS[IP] Programer ::."` ra màn hình. Ngược lại, độ dài của `NAME` sẽ được gán vào biến `NAME_len` trước khi gọi hàm `sub_4010FE()`.  

Tiếp theo, xem hàm `sub_4010FE()`.  

```c
int sub_4010FE()
{
  int v0; // eax
  __int16 v1; // bx
  int v3; // [esp-8h] [ebp-8h]

  v0 = 0;
  dword_403039 = 0;
  while ( 1 )
  {
    v1 = (unsigned __int8)byte_403014[v0];

    if ( (_BYTE)v1 == 90 )
      LOBYTE(v1) = 89;

    if ( (_BYTE)v1 == 122 )
      LOBYTE(v1) = 121;

    if ( (_BYTE)v1 == 57 )
      LOBYTE(v1) = 56;

    HIBYTE(v1) += v0 + 97;
    LOBYTE(v1) = v1 + 1;
    v3 = v0 + 1;

    if ( v1 != *(_WORD *)&String[2 * v0] )
      break;

    ++dword_403039;
    ++v0;
    if ( v3 == NAME_len )
    {
      if ( dword_403039 == NAME_len )
        return MessageBoxA(hWnd, "Register complite!!!", ".:: DiS[IP] Programer ::.", 0);
      return MessageBoxA(hWnd, "Name or Password is BAD!!", ".:: DiS[IP] Programer ::.", 0);
    }
  }
  return MessageBoxA(hWnd, "Name or Password is BAD!!", ".:: DiS[IP] Programer ::.", 0);
}
```  

Có thể thấy được là hàm này sẽ thực hiện một số việc kiểm tra và tính toán trước khi in kết quả ra màn hình với message tương ứng với việc `register` thành công hay thất bại.  

Việc tiếp theo mà em làm là debug hàm `DialogFunc()` để xem xem giá trị `NAME` và `PASS` được lưu ở ô nhớ như thế nào để tiện cho việc phân tích hàm `sub_4010FE()`.
> Với điều kiện là `NAME` phải dài từ 3 ký tự trở lên.  

Khi nhập vào `NAME=abcd` và `PASS=1234`, các giá trị sẽ được lưu ở các ô nhớ sau:  

![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/crack_001/crack_001_address.png)  

Từ đây, ta biết được là `NAME` được bắt đầu lưu tại `403014` và ta cũng thấy được là `PASS` được bắt đầu lưu tại `40301A` với tên biến là `String`.  

### Bắt đầu phân tích hàm `sub_4010FE()`
Sau quá trình debug, thì tên biến của hàm `sub_4010FE()` được đổi lại như sau:  
```c
int sub_4010FE()
{
  int v0; // eax
  __int16 cur_NAME_char; // bx
  int v3; // [esp-8h] [ebp-8h]

  v0 = 0;
  dword_403039 = 0;
  while ( 1 )
  {
    cur_NAME_char = (unsigned __int8)byte_403014[v0];

    if ( (_BYTE)cur_NAME_char == 'Z' )
      LOBYTE(cur_NAME_char) = 'Y';

    if ( (_BYTE)cur_NAME_char == 'z' )
      LOBYTE(cur_NAME_char) = 'y';

    if ( (_BYTE)cur_NAME_char == '9' )
      LOBYTE(cur_NAME_char) = '8';

    HIBYTE(cur_NAME_char) += v0 + 'a';
    LOBYTE(cur_NAME_char) = cur_NAME_char + 1;
    v3 = v0 + 1;

    if ( cur_NAME_char != *(_WORD *)&String[2 * v0] )
      break;

    ++dword_403039;
    ++v0;
    if ( v3 == NAME_len )
    {
      if ( dword_403039 == NAME_len )
        return MessageBoxA(hWnd, "Register complite!!!", ".:: DiS[IP] Programer ::.", 0);
      return MessageBoxA(hWnd, "Name or Password is BAD!!", ".:: DiS[IP] Programer ::.", 0);
    }
  }
  return MessageBoxA(hWnd, "Name or Password is BAD!!", ".:: DiS[IP] Programer ::.", 0);
}
```  

### Về flow của hàm
Chương trình sẽ lấy từng ký tự của `NAME`, sau đó kiểm tra ký này này có bằng `Z`, `z`, `9` hay không, nếu có thì sẽ gán bằng 1 ký tự khác.  

Sau đó, byte liền trước của byte đang chứa ký tự hiện tại sẽ được gán giá trị bằng `'a' + v0`.
> Lưu ý: Từng ký tự của `NAME` sẽ được copy sang 1 ô nhớ khác cho việc kiểm tra và tính toán nên sẽ không làm thay đổi chuỗi `NAME` ban đầu.  

`v0` này là 1 biến tăng theo số vòng lặp, vì vậy ở vòng lặp đầu tiên, giá trị được gán vào byte liền trước này là `a`, ở vòng lặp kế là `b` và kế tiếp là `c`, vân vân.  
Tiếp theo, giá trị của ký tự hiện tại sẽ tăng lên 1.  

Tóm lại, nếu `NAME=ABC` thì ở vòng lặp 1, chuỗi mà ta sẽ dùng dể so sánh là `aB`; ở vòng lặp 2 là 'bC'; vòng lặp 3 là 'cD'.  

Tiếp theo.  
```c
    if ( cur_NAME_char != *(_WORD *)&String[2 * v0] )
      break;
```

Nếu 2-gram vừa tính được khác với 2 ký tự `2*v0 + 1` và `2*v0` thì sẽ `break` vòng `while` và in ra màn hình chuỗi `"Name or Password is BAD!!"`.
> Vì đây là little endian nên khi load giá trị của `PASS` vào thanh ghi thì thứ tự sẽ bị ngược lại: Nhập vào `1234` thì khi load 4 bytes này vào thanh ghi thì sẽ là `4321`.  

Tiếp theo.  
```c
++dword_403039;
    ++v0;
    if ( v3 == NAME_len )
    {
      if ( dword_403039 == NAME_len )
        return MessageBoxA(hWnd, "Register complite!!!", ".:: DiS[IP] Programer ::.", 0);
      return MessageBoxA(hWnd, "Name or Password is BAD!!", ".:: DiS[IP] Programer ::.", 0);
    }
```

Giá trị `dword_403039` và `v0` sẽ tăng 1. Nếu `v3 == NAME_len` nghĩa là đã đến cuối chuỗi `NAME`, thì tiến hành kiểm tra xem `dword_403039` có bằng `NAME_len` hay không. Nếu bằng thì register thành công. Và để register thành công thì tất cả 2-grams được tính từ các ký tự của `NAME` phải bằng với các 2-grams của `PASS` như theo phân tích ở trên.  

Vậy nếu `NAME=abc` thì `PASS` phải là `bacbdc`.

## Kiểm tra kết quả
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/crack_001/crack_001_success.png)
