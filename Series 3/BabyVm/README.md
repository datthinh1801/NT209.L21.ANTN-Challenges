# BabyVm
## Task
```
└─$ ./VirtualMachine.exe
V1r7u41 M4ch1n3 by <@shockbyte>
password->321lkjr12;l3r
fq234f
Press any key to continue . . .
```
> Tìm `password`.  

## Solution
Khi chạy chương trình, ta thấy tên của tác giả `@shockbyte`. Khi tìm tác giả trên github thì ta phát hiện user sau:  

![image](https://user-images.githubusercontent.com/44528004/123901831-f04f1080-d995-11eb-897c-2b69dd81187a.png)  

Khi vào xem các repositories của tác giả thì ta thấy repo sau:  

[![image](https://user-images.githubusercontent.com/44528004/123901881-08269480-d996-11eb-8987-9b85220afa70.png)  ](https://github.com/n30np14gu3/VirtualMachine/tree/master/VirtualMachine)
> Có lẽ đây chính là source code của challenge này ?  

Vào đọc source thôi 😁  

Sau khi xem qua các source code thì ta phát hiện đoạn code sau:  
```c
		//push password (xored)
		{OPCODE_PUSH_VAL, ('!' ^ 0xAC), 0},//!
		{OPCODE_PUSH_VAL, ('r' ^ 0xAC), 0},//r
		{OPCODE_PUSH_VAL, ('3' ^ 0xAC), 0},//3
		{OPCODE_PUSH_VAL, ('k' ^ 0xAC), 0},//k
		{OPCODE_PUSH_VAL, ('c' ^ 0xAC), 0},//c
		{OPCODE_PUSH_VAL, ('4' ^ 0xAC), 0},//4
		{OPCODE_PUSH_VAL, ('r' ^ 0xAC), 0},//r
		{OPCODE_PUSH_VAL, ('c' ^ 0xAC), 0},//c
		{OPCODE_PUSH_VAL, ('_' ^ 0xAC), 0},//_
		{OPCODE_PUSH_VAL, ('p' ^ 0xAC), 0},//p
		{OPCODE_PUSH_VAL, ('0' ^ 0xAC), 0},//0
		{OPCODE_PUSH_VAL, ('t' ^ 0xAC), 0},//t
		{OPCODE_PUSH_VAL, ('_' ^ 0xAC), 0},//_
		{OPCODE_PUSH_VAL, ('m' ^ 0xAC), 0},//m
		{OPCODE_PUSH_VAL, ('4' ^ 0xAC), 0},//4
		{OPCODE_PUSH_VAL, ('_' ^ 0xAC), 0},//_
		{OPCODE_PUSH_VAL, ('i' ^ 0xAC), 0},//i
```  
> `i_4m_t0p_cr4ck3r!`  

Đồng thời chúng ta cũng phát hiện flag sau:  
```c
		//push end message												//			|
		{OPCODE_PUSH_VAL, ('\n' ^ 0x3B), 0},//\n	//<----------
		{OPCODE_PUSH_VAL, ('\r' ^ 0x3B), 0},//\r	
		{OPCODE_PUSH_VAL, ('}' ^ 0x3B), 0},//}	
		{OPCODE_PUSH_VAL, ('3' ^ 0x3B), 0},//3
		{OPCODE_PUSH_VAL, ('m' ^ 0x3B), 0},//m
		{OPCODE_PUSH_VAL, ('_' ^ 0x3B), 0},//_
		{OPCODE_PUSH_VAL, ('3' ^ 0x3B), 0},//3
		{OPCODE_PUSH_VAL, ('5' ^ 0x3B), 0},//5
		{OPCODE_PUSH_VAL, ('r' ^ 0x3B), 0},//r
		{OPCODE_PUSH_VAL, ('3' ^ 0x3B), 0},//3
		{OPCODE_PUSH_VAL, ('v' ^ 0x3B), 0},//v
		{OPCODE_PUSH_VAL, ('3' ^ 0x3B), 0},//3
		{OPCODE_PUSH_VAL, ('r' ^ 0x3B), 0},//r
		{OPCODE_PUSH_VAL, ('_' ^ 0x3B), 0},//_
		{OPCODE_PUSH_VAL, ('7' ^ 0x3B), 0},//7
		{OPCODE_PUSH_VAL, ('n' ^ 0x3B), 0},//n
		{OPCODE_PUSH_VAL, ('0' ^ 0x3B), 0},//0
		{OPCODE_PUSH_VAL, ('d' ^ 0x3B), 0},//d
		{OPCODE_PUSH_VAL, ('_' ^ 0x3B), 0},//_
		{OPCODE_PUSH_VAL, ('3' ^ 0x3B), 0},//3
		{OPCODE_PUSH_VAL, ('E' ^ 0x3B), 0},//s
		{OPCODE_PUSH_VAL, ('4' ^ 0x3B), 0},//4
		{OPCODE_PUSH_VAL, ('3' ^ 0x3B), 0},//3
		{OPCODE_PUSH_VAL, ('1' ^ 0x3B), 0},//1
		{OPCODE_PUSH_VAL, ('p' ^ 0x3B), 0},//p
		{OPCODE_PUSH_VAL, ('{' ^ 0x3B), 0},//{
		{OPCODE_PUSH_VAL, ('B' ^ 0x3B), 0},//B
		{OPCODE_PUSH_VAL, ('H' ^ 0x3B), 0},//H
		{OPCODE_PUSH_VAL, ('S' ^ 0x3B), 0},//S
		{OPCODE_PUSH_VAL, ('>' ^ 0x3B), 0},//>
		{OPCODE_PUSH_VAL, ('-' ^ 0x3B), 0},//-
		{OPCODE_PUSH_VAL, ('g' ^ 0x3B), 0},//g
		{OPCODE_PUSH_VAL, ('a' ^ 0x3B), 0},//a
		{OPCODE_PUSH_VAL, ('l' ^ 0x3B), 0},//l
		{OPCODE_PUSH_VAL, ('f' ^ 0x3B), 0},//f
```  
> `flag->SHB{p134E3_d0n7_r3v3r53_m3}`  

### Chạy thử chương trình
```
└─$ ./VirtualMachine.exe
V1r7u41 M4ch1n3 by <@shockbyte>
password->i_4m_t0p_cr4ck3r!
flag->SHB{p134E3_d0n7_r3v3r53_m3}
Press any key to continue . . .
```
> Thành công ?






