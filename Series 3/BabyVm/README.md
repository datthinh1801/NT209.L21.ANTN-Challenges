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
Sau một hồi xem pseudocode trên IDA Pro thì thấy rằng, chương trình này có khá nhiều hàm và có vẻ là mỗi hàm sẽ tương ứng với một câu lệnh assembly. Một trong số đó là nhóm lệnh `mov` được chương trình biến đổi như sau:  
```c
unsigned __int8 *__thiscall sub_404600(void *this, unsigned __int8 *a2)
{
  unsigned __int8 *result; // eax
  int v3; // [esp+0h] [ebp-30h] BYREF
  int v4; // [esp+4h] [ebp-2Ch] BYREF
  int v5; // [esp+8h] [ebp-28h] BYREF
  int v6; // [esp+Ch] [ebp-24h] BYREF
  int v7; // [esp+10h] [ebp-20h] BYREF
  int v8; // [esp+14h] [ebp-1Ch] BYREF
  int v9; // [esp+18h] [ebp-18h] BYREF
  int v10; // [esp+1Ch] [ebp-14h] BYREF
  int v11; // [esp+20h] [ebp-10h]
  _DWORD *v12; // [esp+24h] [ebp-Ch]
  unsigned __int8 v13; // [esp+28h] [ebp-8h] BYREF
  unsigned __int8 v14; // [esp+29h] [ebp-7h] BYREF
  unsigned __int8 v15; // [esp+2Ah] [ebp-6h] BYREF
  unsigned __int8 v16; // [esp+2Bh] [ebp-5h] BYREF
  unsigned __int8 v17; // [esp+2Ch] [ebp-4h] BYREF
  unsigned __int8 v18; // [esp+2Dh] [ebp-3h] BYREF
  unsigned __int8 v19; // [esp+2Eh] [ebp-2h] BYREF
  unsigned __int8 v20; // [esp+2Fh] [ebp-1h] BYREF

  v12 = this;
  result = a2;
  v11 = *a2;
  switch ( v11 )
  {
    case 0:
      v20 = a2[4];
      result = mov_to_reg(v12, &v20, (_DWORD *)a2 + 2);
      break;
    case 1:
      v19 = a2[8];
      v10 = get_from_reg(v12, &v19);
      v18 = a2[4];
      result = mov_to_reg(v12, &v18, &v10);
      break;
    case 2:
      v3 = get_from_mem(v12, (_DWORD *)a2 + 2);
      v13 = a2[4];
      result = mov_to_reg(v12, &v13, &v3);
      break;
    case 3:
      v17 = a2[4];
      v9 = get_from_reg(v12, &v17);
      result = (unsigned __int8 *)mov_to_mem(v12, &v9, (_DWORD *)a2 + 2);
      break;
    case 4:
      v16 = a2[8];
      v8 = get_from_reg(v12, &v16);
      v7 = get_from_mem(v12, &v8);
      v15 = a2[4];
      result = mov_to_reg(v12, &v15, &v7);
      break;
    case 5:
      v6 = a2[4];
      result = (unsigned __int8 *)mov_to_mem(v12, &v6, (_DWORD *)a2 + 2);
      break;
    case 6:
      v14 = a2[8];
      v5 = get_from_reg(v12, &v14);
      v4 = a2[4];
      result = (unsigned __int8 *)mov_to_mem(v12, &v4, &v5);
      break;
    default:
      return result;
  }
  return result;
}
```  
> Tuy nhiên, vì mình khá lười khi phải dịch toàn bộ hàm sang assembly nên mình thử tìm source code của chương trình và có các phát hiện thú vị 😊.  


Khi chạy chương trình, ta thấy tên của tác giả `@shockbyte`. Khi tìm tác giả trên github thì ta phát hiện user sau:  

![image](https://user-images.githubusercontent.com/44528004/123901831-f04f1080-d995-11eb-897c-2b69dd81187a.png)  

Khi vào xem các repositories của tác giả thì ta thấy repo sau:  

[![image](https://user-images.githubusercontent.com/44528004/123901881-08269480-d996-11eb-8987-9b85220afa70.png)  ](https://github.com/n30np14gu3/VirtualMachine/tree/master/VirtualMachine)
> (👆 Click vào hình trên để xem repository) Có lẽ đây chính là source code của challenge này ?  

Vào đọc source thôi 😁  

### Phân tích sơ bộ
Xem sơ qua source code ta thấy rằng, chương trình sẽ đọc file `vm_dump.vm` và các bytes trong file này là biểu diễn của các câu lệnh assembly.  

Sau khi đọc file `vm_dump.vm`, chương trình sẽ duyệt qua từng block bytes, dịch sang câu lệnh tương ứng và thực thi câu lệnh đó.  
```c
void VMCore::exec()
{
	byte* opcodes = (programSector + sizeof(VM_HEADER));
	cpu.eip = reinterpret_cast<DWORD>(opcodes);
	OPCODE* opcode;
	do
	{
		opcode = reinterpret_cast<OPCODE*>(cpu.eip);
		translate(opcode);
		cpu.eip += sizeof(OPCODE);

	} while (opcode->command != OPCODE_END);

}
```

Trong source code, ta thấy được hàm `create_program` có lẽ là để khởi tạo các giá trị cho chương trình, thì ta phát hiện:  
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
> Thành công!






