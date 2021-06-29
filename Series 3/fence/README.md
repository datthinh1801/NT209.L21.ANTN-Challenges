# fence
## Task
```
└─$ cat readme.txt
the encrypted flag is: "arln_pra_dfgafcchsrb_l{ieeye_ea}"
```
```
└─$ ./encryptor hello
lhleo
└─$ ./encryptor arln_pra_dfgafcchsrb_l{ieeye_ea}
lp_gcs_iyeanrdacrleear_affhb{e_}
```
> Cần tìm plaintext sao cho khi mã hóa thì ciphertext có giá trị là `arln_pra_dfgafcchsrb_l{ieeye_ea}`.  

## Solution
### Thử
```
└─$ ./encryptor a
a
└─$ ./encryptor ab
ab
└─$ ./encryptor abc
cab
└─$ ./encryptor abcd
cadb
└─$ ./encryptor abcde
cadbe
└─$ ./encryptor abcdef
cfadbe
```  

Sau các phép thử trên thì ta có quy luật encryption như sau:  
- Lấy các phần tử tại vị trí thứ `3k + 2` ví dụ 2, 5, 8, 11,...) của plaintext và đặt vào các vị trí đầu tiên của ciphertext. Nghĩa là `ciphertext[0] = plaintext[2]`, `ciphertext[1]  = plaintext[5]`, v.v.
- Lấy các phần từ tại vị trí thứ `3k` (ví dụ 0, 3, 6, 9,...) của plaintext và đặt vào các vị trí kế tiếp của ciphertext.
- Lấy các phần tử tại ví trị thứ `3k + 1` (ví dụ 1, 4, 7, 9,...) của plaintext và đặt vào các vị trí cuối cùng của ciphertext.  

Từ quy luật trên thì ta có script sau để decrypt:  
```python
ciphertext = r"arln_pra_dfgafcchsrb_l{ieeye_ea}"
plain = [None] * 32

index_1 = [i for i in range(2, len(ciphertext), 3)]
index_2 = [i for i in range(0, len(ciphertext), 3)]
index_3 = [i for i in range(1, len(ciphertext), 3)]
index = index_1 + index_2 + index_3
print(index)

for i, j in enumerate(index):
    plain[j] = ciphertext[i]

print(''.join(plain))
```  
### Chạy script
```
└─$ python3 solution.py
[2, 5, 8, 11, 14, 17, 20, 23, 26, 29, 0, 3, 6, 9, 12, 15, 18, 21, 24, 27, 30, 1, 4, 7, 10, 13, 16, 19, 22, 25, 28, 31]
flag{railfence_cyphers_are_bad_}
```  
> `flag{railfence_cyphers_are_bad_}`  
### Kiểm tra flag
```
└─$ ./encryptor flag{railfence_cyphers_are_bad_}
arln_pra_dfgafcchsrb_l{ieeye_ea}
```
> Chính xác!
