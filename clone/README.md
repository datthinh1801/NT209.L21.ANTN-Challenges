# clone
## Task
![image](https://user-images.githubusercontent.com/44528004/118383011-78ff3080-b624-11eb-9032-10f9ae6f745a.png)  

![image](https://user-images.githubusercontent.com/44528004/118383014-80bed500-b624-11eb-9768-48c1587df09d.png)  

> Tiếp tục là 1 bài tìm `User` và `Serial`.

## Solution
Kiểm tra kiến trúc của file.  
```
└─$ file clone.exe
clone.exe: PE32 executable (GUI) Intel 80386, for MS Windows
```  

Mở IDA Pro 32bit.  

Đầu tiên, xem cửa sổ string để tìm chuỗi chúc mừng.  
![image](https://user-images.githubusercontent.com/44528004/118383297-ced4d800-b626-11eb-912a-654a865c8182.png)  

Sau đó nhảy tới hàm in ra chuỗi này.  
![image](https://user-images.githubusercontent.com/44528004/118383317-e8761f80-b626-11eb-9c14-0a4a1b45b166.png)  

Hàm in ra chuỗi chúc mừng là hàm `sub_401180()`, từ đó chúng ta xem pseudocode của hàm.  

```c
LRESULT __stdcall sub_401180(HWND hWnd, UINT Msg, WPARAM wParam, LPARAM lParam)
{
  CHAR *v4; // ecx
  char v5; // dl
  unsigned __int16 v6; // cx
  unsigned __int32 v7; // ecx
  unsigned int v8; // ecx
  unsigned int v9; // ecx
  unsigned int v10; // ecx
  unsigned int v11; // ecx
  int v12; // ecx
  CHAR *v13; // eax
  CHAR v14; // bl

  switch ( Msg )
  {
    case 2u:
      PostQuitMessage(0);
      break;
    case 0x10u:
      DestroyWindow(hWnd);
      break;
    case 0x111u:
      if ( lParam
        && wParam == 103
        && GetDlgItemTextA(hWnd, 102, String, 25) == 8
        && GetDlgItemTextA(hWnd, 101, byte_40307C, 30) >= 5 )
      {
        // START PROCESS USER
        v4 = &byte_40307C[4];
        v5 = 0;
        do
          v5 += *v4++;
        while ( *v4 );
        LOBYTE(v6) = v5;
        HIBYTE(v6) = v5;
        v7 = _byteswap_ulong(v6);
        LOBYTE(v7) = v5;
        BYTE1(v7) = v5;
        v8 = _byteswap_ulong(_byteswap_ulong(_byteswap_ulong(*(_DWORD *)byte_40307C ^ v7) + 50470918) + 559038242);
        LOBYTE(v8) = v8 + 1;
        ++BYTE1(v8);
        v9 = _byteswap_ulong(v8);
        LOBYTE(v9) = v9 - 1;
        --BYTE1(v9);
        v10 = _byteswap_ulong(
                *(_DWORD *)byte_40307C
              + _byteswap_ulong(
                  _byteswap_ulong(
                    _byteswap_ulong(
                      _byteswap_ulong(_byteswap_ulong(_byteswap_ulong(_byteswap_ulong(v9) ^ 0xEDB88320) - 680876936) + 1341392178)
                    + 195935983)
                  + 1)
                - 1));
        LOWORD(v10) = v10 + 1;
        v11 = _byteswap_ulong(v10);
        LOWORD(v11) = v11 + 1;
        dword_4030C8 = _byteswap_ulong(v11);
        
        // START PROCESS SERIAL
        v12 = 0;
        v13 = String;
        while ( 1 )
        {
          v14 = *v13;
          if ( !*v13 )
            break;
          if ( (unsigned __int8)v14 < 0x30u )
            return 0;
          if ( (unsigned __int8)v14 > 0x39u )
          {
            if ( (unsigned __int8)v14 < 0x41u || (unsigned __int8)v14 > 0x46u )
              return 0;
            byte_4030B8[v12] = v14 - 55;
            ++v13;
            ++v12;
          }
          else
          {
            byte_4030B8[v12] = v14 - 48;
            ++v13;
            ++v12;
          }
        }

        if ( dword_4030C8 == _byteswap_ulong(
                               (unsigned __int8)(((byte_4030B8[7] + 16 * byte_4030B8[6]) ^ 0xCD) - 17)
                             + (((unsigned __int8)(((byte_4030B8[5] + 16 * byte_4030B8[4]) ^ 0x90) - 85)
                               + (((unsigned __int8)(((byte_4030B8[3] + 16 * byte_4030B8[2]) ^ 0x56) + 120)
                                 + ((unsigned __int8)(((byte_4030B8[1] + 16 * byte_4030B8[0]) ^ 0x12) + 52) << 8)) << 8)) << 8)) )
        {
          MessageBoxA(0, Text, Caption, 0x40u);
          SetWindowTextA(hWnd, aCloneDefeated);
        }
      }
      break;
    default:
      return DefWindowProcA(hWnd, Msg, wParam, lParam);
  }
  return 0;
}  

Trước tiên, nhìn vào điều kiện của `case` mà sẽ in ra chuỗi chúc mừng.  

```c
if ( lParam
        && wParam == 103
        && GetDlgItemTextA(hWnd, 102, String, 25) == 8
        && GetDlgItemTextA(hWnd, 101, byte_40307C, 30) >= 5 )
```  

`lParam` và `wParam` là gì thì em không rõ, nhưng em có thể đoán được 2 điều kiện sau là `User` hoặc `Serial` phải bằng `8` hoặc lớn hơn hoặc bằng `5`. Em tiến hành test thử từng trường hợp thì thấy là khi nhập `User` có độ dài là `5` và `Serial` có độ dài là `8` thì điều kiện trả về `true`.  
> Vậy `String` chính là `Serial` và `byte_40307C` chính là `User`.  

Sau một hồi debug thì đây là pseudocode của chương trình với các tên biến đã được thay đổi để hỗ trợ cho việc đọc code.  

```c
LRESULT __stdcall sub_401180(HWND hWnd, UINT Msg, WPARAM wParam, LPARAM lParam)
{
  CHAR *v4; // ecx
  char v5; // dl
  unsigned __int16 v6; // cx
  unsigned __int32 v7; // ecx
  unsigned int v8; // ecx
  unsigned int v9; // ecx
  unsigned int v10; // ecx
  unsigned int v11; // ecx
  int v12; // ecx
  CHAR *v13; // eax
  CHAR cur_serial_char; // bl

  switch ( Msg )
  {
    case 2u:
      PostQuitMessage(0);
      break;
    case 0x10u:
      DestroyWindow(hWnd);
      break;
    case 0x111u:
      if ( lParam
        && wParam == 103
        && GetDlgItemTextA(hWnd, 102, Serial, 25) == 8
        && GetDlgItemTextA(hWnd, 101, User, 30) >= 5 )
      {
        v4 = &User[4];
        v5 = 0;
        do
          v5 += *v4++;
        while ( *v4 );
        LOBYTE(v6) = v5;
        HIBYTE(v6) = v5;
        v7 = _byteswap_ulong(v6);
        LOBYTE(v7) = v5;
        BYTE1(v7) = v5;
        v8 = _byteswap_ulong(_byteswap_ulong(_byteswap_ulong(*(_DWORD *)User ^ v7) + 50470918) + 559038242);
        LOBYTE(v8) = v8 + 1;
        ++BYTE1(v8);
        v9 = _byteswap_ulong(v8);
        LOBYTE(v9) = v9 - 1;
        --BYTE1(v9);
        v10 = _byteswap_ulong(
                *(_DWORD *)User
              + _byteswap_ulong(
                  _byteswap_ulong(
                    _byteswap_ulong(
                      _byteswap_ulong(_byteswap_ulong(_byteswap_ulong(_byteswap_ulong(v9) ^ 0xEDB88320) - 680876936) + 1341392178)
                    + 195935983)
                  + 1)
                - 1));
        LOWORD(v10) = v10 + 1;
        v11 = _byteswap_ulong(v10);
        LOWORD(v11) = v11 + 1;
        processed_user = _byteswap_ulong(v11);
        v12 = 0;
        v13 = Serial;
        while ( 1 )
        {
          cur_serial_char = *v13;
          if ( !*v13 )
            break;
          if ( (unsigned __int8)cur_serial_char < '0' )
            return 0;
          if ( (unsigned __int8)cur_serial_char > '9' )
          {
            if ( (unsigned __int8)cur_serial_char < 'A' || (unsigned __int8)cur_serial_char > 'F' )
              return 0;
            processed_serial[v12] = cur_serial_char - '7';
            ++v13;
            ++v12;
          }
          else
          {
            processed_serial[v12] = cur_serial_char - '0';
            ++v13;
            ++v12;
          }
        }

        if ( processed_user == _byteswap_ulong(
                                 (unsigned __int8)(((processed_serial[7] + 16 * processed_serial[6]) ^ 0xCD) - 17)
                               + (((unsigned __int8)(((processed_serial[5] + 16 * processed_serial[4]) ^ 0x90) - 85)
                                 + (((unsigned __int8)(((processed_serial[3] + 16 * processed_serial[2]) ^ 0x56) + 120)
                                   + ((unsigned __int8)(((processed_serial[1] + 16 * processed_serial[0]) ^ 0x12) + 52) << 8)) << 8)) << 8)) )
        {
          MessageBoxA(0, Text, Caption, 0x40u);
          SetWindowTextA(hWnd, aCloneDefeated);
        }
      }
      break;
    default:
      return DefWindowProcA(hWnd, Msg, wParam, lParam);
  }
  return 0;
}
```
