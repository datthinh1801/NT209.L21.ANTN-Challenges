# S_Crackme1
## Task  
Cửa số đầu tiên của chương trình:  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/S_Crackme1/s_crackme1_1.png)  

Khi nhấn vào nút `Check`.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/S_Crackme1/s_crackme1_failed.png)  

Khi nhấn vào nút `About`.  
![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/S_Crackme1/s_crackme1_info.png)

> Khác với các challenge trước là chúng ta sẽ được prompted để nhập `username` hay `password`, challenge này khá là huyền bí vì ngay từ đầu không thể biết được chương trình này muốn gì.  

## Solution
Kiểm tra kiến trúc của file.  

```
└─$ file KeyMe1.exe
KeyMe1.exe: PE32 executable (GUI) Intel 80386, for MS Windows
```

Mở IDA Pro 32bit và xem pseudocode.  

Trong chương trình này có khá là nhiều hàm, nhưng em đoán là hàm em cần quan tâm là hàm `DialogFunc()` vì ở challenge trước ([crack_001](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/tree/main/crack_001)), `DiaglogFunc()` cũng là hàm bắt đầu cho các thao tác tính toán và kiểm tra các điều kiện để dẫn đến việc crack thành công hay thất bại.  Do đó, em sẽ bắt đầu xem pseudocode của hàm `DialogFunc()`.  

```c
INT_PTR __stdcall DialogFunc(HWND hWnd, UINT a2, WPARAM a3, LPARAM a4)
{
  HICON v4; // eax
  unsigned int v5; // kr00_4
  CHAR *v6; // esi
  unsigned __int8 v7; // al
  unsigned __int16 v9; // ax
  HANDLE v10; // eax
  DWORD v11; // eax
  HGLOBAL v12; // eax
  HANDLE v13; // eax
  unsigned int v14; // kr04_4
  int v15; // ecx

  if ( a2 != 272 )
  {
    if ( a2 == 16 )
      EndDialog(hWnd, 0);
    if ( a2 != 273 )
      return 0;
    v9 = a3;
    if ( (_WORD)a3 == 104 )
      v9 = EndDialog(hWnd, 0);
    if ( v9 == 105 )
      v9 = MessageBoxA(0, Text, Caption, 0x40u);
    if ( v9 != 106 )
    {
LABEL_24:
      if ( v9 == 103 )
      {
        OpenClipboard(0);
        v13 = GetClipboardData(1u);
        if ( !v13 )
          goto LABEL_30;
        dword_403233 = (int)v13;
        v14 = __readeflags();
        dword_403342 = dword_40333E;
        v15 = dword_403346;
        do
        {
          dword_403342 -= *(unsigned __int8 *)(v15 + dword_403233 - 1);
          --v15;
        }
        while ( v15 );
        __writeeflags(v14);
        if ( dword_403342 )
        {
LABEL_30:
          SetDlgItemTextA(hWnd, 102, aRegistrationFa);
        }
        else
        {
          SetDlgItemTextA(hWnd, 102, aStep1OkNowRegi);
          EnableWindow(dword_403356, 0);
          EnableWindow(::hWnd, 1);
        }
        CloseClipboard();
      }
      return 0;
    }
    v10 = CreateFileA(FileName, 0x80000000, 1u, 0, 3u, 0x80u, 0);
    if ( v10 == (HANDLE)-1 )
      goto LABEL_22;
    hFile = v10;
    v11 = GetFileSize(v10, 0);
    if ( v11 < 8 )
    {
      CloseHandle(hFile);
LABEL_22:
      SetDlgItemTextA(hWnd, 102, aRegistrationFa);
      EnableWindow(::hWnd, 0);
      EnableWindow(dword_403356, 1);
      goto LABEL_23;
    }
    nNumberOfBytesToRead = v11;
    v12 = GlobalAlloc(0x40u, v11);
    if ( v12 )
    {
      lpBuffer = v12;
      if ( !ReadFile(hFile, v12, nNumberOfBytesToRead, &NumberOfBytesRead, 0) )
      {
        CloseHandle(hFile);
        GlobalFree(lpBuffer);
        goto LABEL_22;
      }
      CloseHandle(hFile);
      if ( dword_403342 + (*((_DWORD *)lpBuffer + 1) ^ *(_DWORD *)lpBuffer) != dword_40333E )
        goto LABEL_22;
      SetDlgItemTextA(hWnd, 102, aGoodWorkYouHav);
      EnableWindow(::hWnd, 0);
    }
    else
    {
      SetDlgItemTextA(hWnd, 102, aMemoryAllocati);
      CloseHandle(hFile);
    }
LABEL_23:
    v9 = (unsigned __int16)GlobalFree(lpBuffer);
    goto LABEL_24;
  }
  v4 = LoadIconA(hInstance, (LPCSTR)0x3E8);
  SendMessageA(hWnd, 0x80u, 1u, (LPARAM)v4);
  dword_403356 = GetDlgItem(hWnd, 103);
  ::hWnd = GetDlgItem(hWnd, 106);
  nSize = 255;
  GetComputerNameA(String, &nSize);
  dword_403346 = lstrlenA(String);
  v5 = __readeflags();
  v6 = String;
  dword_40333E = 0;
  do
  {
    v7 = *v6++;
    dword_40333E += v7;
  }
  while ( v7 );
  __writeeflags(v5);
  return 0;
}
```

Hàm này khá là dài nhưng khi xem kỹ 1 chút thì em thấy có 2 hàm liên quan đến clipboard, đó là `OpenClipboard(0)` và `CloseClipboard()`. Từ đây em mạnh dạn đoán là chương trình sẽ đọc clipboard từ máy và thực hiện các tác tính toán và so sánh trên giá trị này.  

Chúng ta bắt đầu phân tích đoạn code làm việc với clipboard.  

```c
OpenClipboard(0);
        v13 = GetClipboardData(1u);

        if ( !v13 )
          goto LABEL_30;

        input_address = (int)v13;
        v14 = __readeflags();
        dword_403342 = const_1058;
        v15 = const_15;
        do
        {
          dword_403342 -= *(unsigned __int8 *)(v15 + input_address - 1);
          --v15;
        }
        while ( v15 );
        __writeeflags(v14);
        if ( dword_403342 )
        {
LABEL_30:
          SetDlgItemTextA(hWnd, 102, aRegistrationFa);
        }
        else
        {
          SetDlgItemTextA(hWnd, 102, aStep1OkNowRegi);
          EnableWindow(dword_403356, 0);
          EnableWindow(::hWnd, 1);
        }
        CloseClipboard();
```  

Sau một hồi debug, thì flow của đoạn code này như sau.  

Chương trình có 2 biến hằng số là `dword_403342` với giá trị bằng `1058` và `v15` với giá trị bằng `15`.  Tiếp theo, chương trình sẽ lấy ký tự thứ `v15` từ clipboard, và trừ giá trị của `dword_403342` đi một con số bằng với giá trị của ký tự thứ `v15` này.  
Vì `v15` là một biến lặp, nên nó sẽ giảm dần từ `15`, `14`, `13`, ..., `1`. Điều này tương đương với việc, chương trình sẽ lấy các ký tự từ clipboard theo thứ tự từ 15 tới 1 và trừ giá trị của `dword_403342` đi một lượng bằng giá trị của ký tự đang xét.  

Sau đó chương trình sẽ kiểm tra xem sau vòng lặp này, giá trị của `dword_403342` có bằng `0` hay không. Nếu có thì in ra cửa sổ 1 chuỗi `Step 1 ok -> now Register it!`.  

![](https://github.com/datthinh1801/NT209.L21.ANTN-Challenges/blob/main/S_Crackme1/s_crackme1_ida_view.png)  

Vậy ta chọn 15 ký tự để copy sao cho tổng giá trị bằng `1058`. Chuỗi được chọn là `GGGGGGGGFFFFFFF`.  

### Kiểm tra kết quả
![image](https://user-images.githubusercontent.com/44528004/118341879-14bc6e00-b54b-11eb-8d03-f5e9e81381a5.png)

> Bước 1 thành công.  

Bấm thử nút `Register`.  
![image](https://user-images.githubusercontent.com/44528004/118341921-551bec00-b54b-11eb-8fb2-fea96fad1def.png)

> Chưa thể đăng ký.
