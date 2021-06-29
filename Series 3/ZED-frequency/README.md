# ZED-frequency
## Task
```
└─$ ./ZED-Frequency.bin
usage: ./ZED-Frequency.bin <keyfile>
```
```
└─$ ./ZED-Frequency.bin input.txt
the generated key is: 11000000000000000000000000
you failed!!
```
> Ghi nội dụng vào một file sao cho key được tạo từ nội dụng file thỏa một số điều kiện nhất định nào đó.  

## Solution
Vì tên challenge có từ `frequency` nên ta có thể đoán được là chương trình sẽ đếm số lần xuất hiện của các ký tự nào đó.  

Khi test thử thì đúng là như vậy.  
```
└─$ cat input.txt
a
└─$ ./ZED-Frequency.bin input.txt
the generated key is: 10000000000000000000000000
you failed!!
```  
> Với nội dung của file `input.txt` là `a` thì `generated key` là `10000000000000000000000000` tương ứng frequency của ký tự `a` là `1` và của các ký tự khác là `0`.  


```
└─$ cat input.txt
ab
└─$ ./ZED-Frequency.bin input.txt
the generated key is: 11000000000000000000000000
you failed!!
```  
> Tương tự, với nội dung của file `input.txt` là `ab` thì `generated key` là `11000000000000000000000000` tương ứng frequency của ký tự `a` là `1`, của ký tự 'b' là `1` và của các ký tự khác là `0`.  

Xem pseudocode của chương trình.  
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int i; // [rsp+10h] [rbp-B0h]
  int v5; // [rsp+10h] [rbp-B0h]
  int j; // [rsp+14h] [rbp-ACh]
  FILE *stream; // [rsp+18h] [rbp-A8h]
  int v8[28]; // [rsp+20h] [rbp-A0h]
  char s1[40]; // [rsp+90h] [rbp-30h] BYREF
  unsigned __int64 v10; // [rsp+B8h] [rbp-8h]

  v10 = __readfsqword(0x28u);
  if ( argc <= 1 )
  {
    printf("usage: %s <keyfile>\n", *argv);
    exit(1);
  }
  stream = fopen(argv[1], "rt");
  for ( i = 0; i <= 25; ++i )
    v8[i] = 0;
  while ( 1 )
  {
    v5 = fgetc(stream);
    if ( v5 == -1 )
      break;
    if ( v5 <= '`' || v5 > 'z' )
    {
      if ( v5 > '@' && v5 <= 'Z' )
        ++v8[v5 - 'A'];
    }
    else
    {
      ++v8[v5 - 'a'];
    }
  }
  printf("the generated key is: ");
  for ( j = 0; j <= 25; ++j )
  {
    printf("%d", (unsigned int)v8[j]);
    s1[j] = LOBYTE(v8[j]) + '0';
  }
  s1[26] = 0;
  putchar(10);
  if ( !strcmp(s1, "01234567890123456789012345") )
    puts("you succeed!!");
  else
    puts("you failed!!");
  return 0;
}
```  

Ở pseudocode trên, ta có thể thấy rằng, chương trình chỉ xét các ký tự alphabet và xét case-insensitive.  

Và điều kiện thành công là frequency phải bằng `01234567890123456789012345`. Nghĩa là, ký tự `a` và `A` xuất hiện `0` lần, ký tự `b` và `B` xuất hiện 1 lần, v.v.  

### Script
```python
from string import ascii_lowercase

nums = "01234567890123456789012345"
nums = list(map(int, nums))
alpha = list(ascii_lowercase)
s = ''.join(alpha[i] * nums[i] for i in range(len(nums)))
print(s)
```

Chạy script.  
```
└─$ python3 solution.py
bccdddeeeefffffgggggghhhhhhhiiiiiiiijjjjjjjjjlmmnnnoooopppppqqqqqqrrrrrrrsssssssstttttttttvwwxxxyyyyzzzzz
```

### Kiểm tra kết quả
```
└─$ ./ZED-Frequency.bin input.txt
the generated key is: 01234567890123456789012345
you succeed!!
```
> Thành công!

