# no strings attached
## Task
```
└─$ ./crackme.exe
Enter password: password
WRONG PASSWORD
```
> Tìm password.  

## Solution
Xem kiến trúc của file.  
```
└─$ file crackme.exe
crackme.exe: PE32 executable (console) Intel 80386, for MS Windows
```  

Mở IDA Pro 32bit và xem pseudocode.  

```c
int __cdecl main_0(int argc, const char **argv, const char **envp)
{
  int v3; // eax
  char encrypted_c_string[108]; // [esp+14h] [ebp-E0h] BYREF
  char input[36]; // [esp+80h] [ebp-74h] BYREF
  char correct_string[16]; // [esp+A4h] [ebp-50h] BYREF
  char wrong_string[24]; // [esp+B4h] [ebp-40h] BYREF
  char Str[24]; // [esp+CCh] [ebp-28h] BYREF
  int v10; // [esp+F0h] [ebp-4h]

  strcpy(Str, "Enter password: ");
  strcpy(wrong_string, "WRONG PASSWORD");
  strcpy(correct_string, "CORRECT");
  sub_F712D0(std::cout, Str);
  sub_F710C8();
  v10 = 0;
  sub_F710AF(std::cin, (int)input);
  sub_F71352(encrypted_c_string);
  LOBYTE(v10) = 1;
  sub_F712C1((int)input);
  if ( (unsigned __int8)sub_F71438(encrypted_c_string) )
    v3 = sub_F712D0(std::cout, correct_string);
  else
    v3 = sub_F712D0(std::cout, wrong_string);
  std::ostream::operator<<(v3, sub_F713C5);
  while ( !std::ios_base::eof((std::ios_base *)(*(_DWORD *)(std::cin + 4) + std::cin)) )
    ;
  LOBYTE(v10) = 0;
  sub_F71177(encrypted_c_string);
  v10 = -1;
  sub_F71343(input);
  return 0;
}
```  

Từ pseudocode trên, để chương trình in ra chuỗi `correct` (được lưu tại biến `correct_string` như trong pseudocode), thì câu lệnh `if ( (unsigned __int8)sub_F71438(encrypted_c_string) )` phải trả về `true`.  

Tiến hành debug và trace theo flow của chương trình.  

![image](https://user-images.githubusercontent.com/44528004/121341260-af417e80-c94a-11eb-9933-9f8c2010d836.png)  

![image](https://user-images.githubusercontent.com/44528004/121341283-b5375f80-c94a-11eb-98fe-262315515b27.png)  

![image](https://user-images.githubusercontent.com/44528004/121341306-bcf70400-c94a-11eb-88cc-72f4b6a1af0f.png)  

Khi trace đến đây thì ta thấy hàm `sub_F763D0` có thực hiện các phép so sánh và sau đó trả về các giá trị `0` hoặc `1` làm ảnh hưởng đến câu lệnh `if` ở hàm `main`. Vậy để in được `correct_string` thì hàm này phải trả về `1`, điều này tương đương với việc câu lệnh `if ( sub_F71645(this + 72, -858993460) != 18 )` và 
`if ( *(char *)sub_F7164A(i) != this[4 * i] )` trong vòng lặp `for` phải sai để hàm không trả về `0`.  

Tiếp tục debug chương trình với câu lệnh `if ( sub_F71645(this + 72, -858993460) != 18 )`.  

![image](https://user-images.githubusercontent.com/44528004/121341353-c7190280-c94a-11eb-8b35-eec53b69a68f.png)  

![image](https://user-images.githubusercontent.com/44528004/121341375-cbddb680-c94a-11eb-8dbf-157d4b245ed2.png)

Sau vài lần debug thì ta biết được hàm này trả về độ dài chuỗi nhập. Trong trường hợp này, khi chuỗi nhập là `abcdef` thì hàm trả về `6` tương đương độ dài của chuỗi. Vậy để bypass câu lệnh `if` đầu tiên thì chuỗi nhập phải có độ dài là `18`.  

Tiếp tục debug với chuỗi nhập mới gồm 18 ký tự `a`.  

![image](https://user-images.githubusercontent.com/44528004/121343800-41e31d00-c94d-11eb-9a9d-13da9c383560.png)

![image](https://user-images.githubusercontent.com/44528004/121343976-6e973480-c94d-11eb-9ea7-b5427db24fd1.png)  

Từ hình trên thì có thể đoán được là ký tự tại vị trí `i` của chuỗi nhập sẽ được so sánh với một ký tự nào đó (`0x65` là mã ascii của ký tự `e`). Các ký tự *nào đó* này chính là các ký tự của chuỗi *không liên tiếp* `encrypted-c-string` (như stack view ở bên dưới). Đồng thời, câu lệnh `if` so sánh ký tự thứ `i` của chuỗi nhập với ký tự thứ `4 * i` của chuỗi `encrypted-c-string` (bỏ qua 3 bytes padding`.  

```
Stack[000007E4]:0053FD44 db  65h ; e
Stack[000007E4]:0053FD45 db    0
Stack[000007E4]:0053FD46 db    0
Stack[000007E4]:0053FD47 db    0
Stack[000007E4]:0053FD48 db  6Eh ; n
Stack[000007E4]:0053FD49 db    0
Stack[000007E4]:0053FD4A db    0
Stack[000007E4]:0053FD4B db    0
Stack[000007E4]:0053FD4C db  63h ; c
Stack[000007E4]:0053FD4D db    0
Stack[000007E4]:0053FD4E db    0
Stack[000007E4]:0053FD4F db    0
Stack[000007E4]:0053FD50 db  72h ; r
Stack[000007E4]:0053FD51 db    0
Stack[000007E4]:0053FD52 db    0
Stack[000007E4]:0053FD53 db    0
Stack[000007E4]:0053FD54 db  79h ; y
Stack[000007E4]:0053FD55 db    0
Stack[000007E4]:0053FD56 db    0
Stack[000007E4]:0053FD57 db    0
Stack[000007E4]:0053FD58 db  70h ; p
Stack[000007E4]:0053FD59 db    0
Stack[000007E4]:0053FD5A db    0
Stack[000007E4]:0053FD5B db    0
Stack[000007E4]:0053FD5C db  74h ; t
Stack[000007E4]:0053FD5D db    0
Stack[000007E4]:0053FD5E db    0
Stack[000007E4]:0053FD5F db    0
Stack[000007E4]:0053FD60 db  65h ; e
Stack[000007E4]:0053FD61 db    0
Stack[000007E4]:0053FD62 db    0
Stack[000007E4]:0053FD63 db    0
Stack[000007E4]:0053FD64 db  64h ; d
Stack[000007E4]:0053FD65 db    0
Stack[000007E4]:0053FD66 db    0
Stack[000007E4]:0053FD67 db    0
Stack[000007E4]:0053FD68 db  2Dh ; -
Stack[000007E4]:0053FD69 db    0
Stack[000007E4]:0053FD6A db    0
Stack[000007E4]:0053FD6B db    0
Stack[000007E4]:0053FD6C db  63h ; c
Stack[000007E4]:0053FD6D db    0
Stack[000007E4]:0053FD6E db    0
Stack[000007E4]:0053FD6F db    0
Stack[000007E4]:0053FD70 db  2Dh ; -
Stack[000007E4]:0053FD71 db    0
Stack[000007E4]:0053FD72 db    0
Stack[000007E4]:0053FD73 db    0
Stack[000007E4]:0053FD74 db  73h ; s
Stack[000007E4]:0053FD75 db    0
Stack[000007E4]:0053FD76 db    0
Stack[000007E4]:0053FD77 db    0
Stack[000007E4]:0053FD78 db  74h ; t
Stack[000007E4]:0053FD79 db    0
Stack[000007E4]:0053FD7A db    0
Stack[000007E4]:0053FD7B db    0
Stack[000007E4]:0053FD7C db  72h ; r
Stack[000007E4]:0053FD7D db    0
Stack[000007E4]:0053FD7E db    0
Stack[000007E4]:0053FD7F db    0
Stack[000007E4]:0053FD80 db  69h ; i
Stack[000007E4]:0053FD81 db    0
Stack[000007E4]:0053FD82 db    0
Stack[000007E4]:0053FD83 db    0
Stack[000007E4]:0053FD84 db  6Eh ; n
Stack[000007E4]:0053FD85 db    0
Stack[000007E4]:0053FD86 db    0
Stack[000007E4]:0053FD87 db    0
Stack[000007E4]:0053FD88 db  67h ; g
Stack[000007E4]:0053FD89 db    0
Stack[000007E4]:0053FD8A db    0
Stack[000007E4]:0053FD8B db    0
```  

Khi đã debug tới đây thì ta không ngần ngại thử ngay `password` là `encrypted-c-string` vì chuỗi này cũng có 18 ký tự.  

### Kiểm tra kết quả
```
└─$ ./crackme.exe
Enter password: encrypted-c-string
CORRECT
```
> Chính xác!
