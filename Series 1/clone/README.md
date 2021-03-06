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
  char accumulated_user_chars; // dl
  unsigned __int16 v6; // cx
  unsigned __int32 v7; // ecx
  unsigned int v8; // ecx
  unsigned int v9; // ecx
  unsigned int v10; // ecx
  unsigned int v11; // ecx
  int processed_serial_index; // ecx
  CHAR *cur_serial_char_addr; // eax
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
        // START PROCESS USER
        v4 = &User[4];
        accumulated_user_chars = 0;
        do
          accumulated_user_chars += *v4++;
        while ( *v4 );
        LOBYTE(v6) = accumulated_user_chars;
        HIBYTE(v6) = accumulated_user_chars;
        v7 = _byteswap_ulong(v6);
        LOBYTE(v7) = accumulated_user_chars;
        BYTE1(v7) = accumulated_user_chars;
        v8 = _byteswap_ulong(_byteswap_ulong(_byteswap_ulong(*(_DWORD *)User ^ v7) + 0x3022006) + 0x21523F22);
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
                      _byteswap_ulong(_byteswap_ulong(_byteswap_ulong(_byteswap_ulong(v9) ^ 0xEDB88320) - 0x28955B88) + 0x4FF40532)
                    + 0xBADBEEF)
                  + 1)
                - 1));
        LOWORD(v10) = v10 + 1;
        v11 = _byteswap_ulong(v10);
        LOWORD(v11) = v11 + 1;
        processed_user = _byteswap_ulong(v11);
        
        // START PROCESS SERIAL
        processed_serial_index = 0;
        cur_serial_char_addr = Serial;
        while ( 1 )
        {
          cur_serial_char = *cur_serial_char_addr;
          if ( !*cur_serial_char_addr )
            break;
          if ( (unsigned __int8)cur_serial_char < '0' )
            return 0;
          if ( (unsigned __int8)cur_serial_char > '9' )
          {
            if ( (unsigned __int8)cur_serial_char < 'A' || (unsigned __int8)cur_serial_char > 'F' )
              return 0;
            processed_serial[processed_serial_index] = cur_serial_char - '7';
            ++cur_serial_char_addr;
            ++processed_serial_index;
          }
          else
          {
            processed_serial[processed_serial_index] = cur_serial_char - '0';
            ++cur_serial_char_addr;
            ++processed_serial_index;
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

### Process `User`

```c
// START PROCESS USER
        v4 = &User[4];
        accumulated_user_chars = 0;
        do
          accumulated_user_chars += *v4++;
        while ( *v4 );
        LOBYTE(v6) = accumulated_user_chars;
        HIBYTE(v6) = accumulated_user_chars;
        v7 = _byteswap_ulong(v6);
        LOBYTE(v7) = accumulated_user_chars;
        BYTE1(v7) = accumulated_user_chars;
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
```  

Ở đoạn code xử lý `User`, nhìn chung, các ký tự thứ 5 trở đi của `User` sẽ được cộng dồn vào 1 biến gọi là `accumulated_user_chars`. Sau đó giá trị của biến này sẽ được dùng để thực hiện các phép tính toán và kết quả cuối cùng sẽ được lưu vào 1 biến gọi là `processed_user`.

### Process `Serial`

```c
// START PROCESS SERIAL
        processed_serial_index = 0;
        cur_serial_char_addr = Serial;
        while ( 1 )
        {
          cur_serial_char = *cur_serial_char_addr;
          if ( !*cur_serial_char_addr )
            break;
          if ( (unsigned __int8)cur_serial_char < '0' )
            return 0;
          if ( (unsigned __int8)cur_serial_char > '9' )
          {
            if ( (unsigned __int8)cur_serial_char < 'A' || (unsigned __int8)cur_serial_char > 'F' )
              return 0;
            processed_serial[processed_serial_index] = cur_serial_char - '7';
            ++cur_serial_char_addr;
            ++processed_serial_index;
          }
          else
          {
            processed_serial[processed_serial_index] = cur_serial_char - '0';
            ++cur_serial_char_addr;
            ++processed_serial_index;
          }
        }
```  

Ở đoạn code xử lý `Serial`, từng ký tự của `Serial` được lấy ra để thực hiện tính toán và so sánh. Khác với khi xử lý `User`, việc xử lý `Serial` sẽ bị kết thúc nếu không thỏa các điều kiện sau:
1. Một ký tự nào đó < `'0'`
2. Một ký tự nào đó > `'9'` nhưng nằm ngoài các giá trị từ `'A'` đến `'F'`.

> Vậy các ký tự của `Serial` phải `>= '0'` và `<= '9'`, nếu `> '9'` thì phải là các ký tự từ `'A'` đến `'F'`. Và nhờ đó, giá trị sau khi tính, thuộc [0, 15], luôn có thể được biểu diễn bằng 1 ký tự hexa hỗ trợ việc tính toán bên dưới.  

### Điều kiện quyết định

```c
if ( processed_user == _byteswap_ulong(
                                 (unsigned __int8)(((processed_serial[7] + 16 * processed_serial[6]) ^ 0xCD) - 17)
                               + (((unsigned __int8)(((processed_serial[5] + 16 * processed_serial[4]) ^ 0x90) - 85)
                                 + (((unsigned __int8)(((processed_serial[3] + 16 * processed_serial[2]) ^ 0x56) + 120)
                                   + ((unsigned __int8)(((processed_serial[1] + 16 * processed_serial[0]) ^ 0x12) + 52) << 8)) << 8)) << 8)) )
        {
          MessageBoxA(0, Text, Caption, 0x40u);
          SetWindowTextA(hWnd, aCloneDefeated);
        }
```

Ở điều kiện quyết định này, giá trị của `processed_user` phải bằng với giá trị của `processed_serial` sau thêm một vài bước xử lý.  

Cụ thể hơn, `byte 0` của `processed_serial` (sau các bước tính toán này) sẽ phải bằng với `byte 3` của `processed_user`; `byte 1` của `processed_serial` phải bằng với `byte 2` của `processed_user`; `byte 2` của `processed_serial` phải bằng với `byte 1` của `processed_user`; và `byte 3` của `processed_serial` phải bằng với `byte 0` của `processed_user`.

Vậy để giải được bài trên, chúng ta có thể chọn 1 chuỗi `User` sau đó tìm `Serial` tương ứng để thỏa các điều kiện, hoặc ngược lại.

### Script
Tính `User`.
```python
def get_low_word(dword):
    return dword & 0xffff


def get_high_word(dword):
    return (dword >> 16) & 0xffff


def get_low_byte(word):
    return word & 0xff


def get_high_byte(word):
    return (word >> 8) & 0xff


def swap_word(dword):
    low_word = get_low_word(dword)
    high_word = get_high_word(dword)
    reversed_low_word = (get_low_byte(low_word) << 8) ^ get_high_byte(low_word)
    reversed_high_word = (get_low_byte(high_word) <<
                          8) ^ get_high_byte(high_word)
    return (reversed_low_word << 16) ^ reversed_high_word


def string_to_ascii_little_endian(s):
    res = 0
    for c in s[::-1]:
        res <<= 8
        res ^= ord(c)
    return res


# Process user
user = "abcde"
accumulated_user_chars = 0

for c in user[4:]:
    accumulated_user_chars += ord(c)
    accumulated_user_chars &= 0xff

v6 = accumulated_user_chars
v6 = (accumulated_user_chars << 8) ^ accumulated_user_chars
v7 = swap_word(v6)
v7 ^= accumulated_user_chars
v7 ^= (accumulated_user_chars << 8)

v8 = swap_word(swap_word(swap_word((string_to_ascii_little_endian(
    user) ^ v7) & 0xffffffff) + 0x3022006) + 0x21523f22)

v8 += 0x1
v8 += 0x100

v9 = swap_word(v8)
v9 -= 0x1
v9 -= 0x100

v10 = swap_word(
    (string_to_ascii_little_endian(user) & 0xffffffff) +
    swap_word(
        swap_word(
            swap_word(
                swap_word(
                    swap_word(
                        swap_word(
                            swap_word(v9) ^ 0xEDB88320)
                        - 0x28955B88)
                    + 0x4FF40532)
                + 0xBADBEEF)
            + 0x1) - 0x1))

v10 += 0x1
v11 = swap_word(v10)
v11 += 0x1
processed_user = swap_word(v11)
print(hex(processed_user))  # processed_user = 0x827bbae0

##### REVERSE SERIAL #####
final_serial = processed_user
serial = [0] * 8
"""
x = swap_word(0xff & (((processed_serial[7] + 16 * processed_serial[6]) ^ 0xCD) - 17)
              + ((0xff & (((processed_serial[5] + 16 * processed_serial[4]) ^ 0x90) - 85)
                  + ((0xff & (((processed_serial[3] + 16 * processed_serial[2]) ^ 0x56) + 120)
                        + (0xff & (((processed_serial[1] + 16 * processed_serial[0]) ^ 0x12) + 52)  # byte 0 of final_serial
                        << 8))      # byte 1 of final_serial
                    << 8))      # byte 2 of final_serial
                << 8))      # byte 3 of final_serial
# trước khi swap_word(),
# byte layout là [b0][b1][b2][b3], tương đương [s0s1][s2s3][s4s5][s6s7] (với s[i] là ký tự thứ i của processed_serial).

# sau khi swap_word(),
# byte layout là [b3][b2][b1][b0] và các bytes này phải bằng với byte của processed_user ở cùng vị trí.
# Trong trường hợp này, khi processed_user = 0x827bbae0,
# b3 = 0x82; b2 = 0x7b; b1 = 0xba; b0 = 0xe0.
"""

# tính ngược để tìm giá trị của các bytes của processed_serial
byte_3_serial = ((final_serial >> 24) + 17) ^ 0xCD              # extract pre-computed byte 0 of final_result
byte_2_serial = (((final_serial >> 16) & 0xff) + 85) ^ 0x90     # extract pre-computed byte 1 of final_result
byte_1_serial = (((final_serial >> 8) & 0xff) - 120) ^ 0x56     # extract pre-computed byte 2 of final_result
byte_0_serial = ((final_serial & 0xff) - 52) ^ 0x12             # extract pre-computed byte 3 of final_result

# sắp xếp lại vị trí của các byte này giống với thứ tự trước khi swap_word()
serial_bytes_arr = [byte_0_serial, byte_1_serial, byte_2_serial, byte_3_serial]

# fun fact: 0xe + 16 * 0xf = 0xfe => s[1] + 16 * s[0] = s[0]s[1]
for i in range(8):
    # nếu ta đang xét ký tự ở vị trí chẵn
    if i % 2 == 0:
        # giá trị của nó phải bằng với 4 bit cao ở byte tương ứng (theo fun fact ở trên)
        must_match_value = serial_bytes_arr[i // 2] >> 4
    else:
    # nếu không, giá trị bằng 4 bit thấp ở byte tương ứng.
        must_match_value = serial_bytes_arr[i // 2] & 0xf

    if must_match_value > 0x9:
        serial[i] = must_match_value + ord('7')
    else:
        serial[i] = must_match_value + ord('0')
    print(chr(serial[i]), end="")
# BE14405E

```  

Vậy với `User = abcde`, `Serial = BE14405E`.

### Kiểm tra kết quả
![image](https://user-images.githubusercontent.com/44528004/118386843-83c9bd80-b644-11eb-8819-ea44d0fe1e4d.png)  

> Bravo!!!!!!!!!!

