# Keygen Me Part 1
## Task
```
└─$ ./Keygenme_Part_One.exe

__    __  ________  __      __   ______   ________  __    __  __       __  ________
|  \  /  \|        \|  \    /  \ /      \ |        \|  \  |  \|  \     /  \|        \
| $$ /  $$| $$$$$$$$ \$$\  /  $$|  $$$$$$\| $$$$$$$$| $$\ | $$| $$\   /  $$| $$$$$$$$
| $$/  $$ | $$__      \$$\/  $$ | $$ __\$$| $$__    | $$$\| $$| $$$\ /  $$$| $$__
| $$  $$  | $$  \      \$$  $$  | $$|    \| $$  \   | $$$$\ $$| $$$$\  $$$$| $$  \
| $$$$$\  | $$$$$       \$$$$   | $$ \$$$$| $$$$$   | $$\$$ $$| $$\$$ $$ $$| $$$$$
| $$ \$$\ | $$_____     | $$    | $$__| $$| $$_____ | $$ \$$$$| $$ \$$$| $$| $$_____
| $$  \$$\| $$     \    | $$     \$$    $$| $$     \| $$  \$$$| $$  \$ | $$| $$     \
 \$$   \$$ \$$$$$$$$     \$$      \$$$$$$  \$$$$$$$$ \$$   \$$ \$$      \$$ \$$$$$$$$
_______    ______   _______  ________         ______   __    __  ________
|       \  /      \ |       \|        \       /      \ |  \  |  \|        \
| $$$$$$$\|  $$$$$$\| $$$$$$$\\$$$$$$$$      |  $$$$$$\| $$\ | $$| $$$$$$$$
| $$__/ $$| $$__| $$| $$__| $$  | $$         | $$  | $$| $$$\| $$| $$__
| $$    $$| $$    $$| $$    $$  | $$         | $$  | $$| $$$$\ $$| $$  \
| $$$$$$$ | $$$$$$$$| $$$$$$$\  | $$         | $$  | $$| $$\$$ $$| $$$$$
| $$      | $$  | $$| $$  | $$  | $$         | $$__/ $$| $$ \$$$$| $$_____
| $$      | $$  | $$| $$  | $$  | $$          \$$    $$| $$  \$$$| $$     \
 \$$       \$$   \$$ \$$   \$$   \$$           \$$$$$$  \$$   \$$ \$$$$$$$$

Level= 1                 For beginners         Coded By : Sir_Zed
Rules: 1) No Patching 2) You have to make a keygen and a tutorial. 3) Self Keygen
If You have any question : radpak333@gmail.com
Good Luck!

Enter Username (Only Letters and numbers): datthinh
Enter Serial : 123456
[-]Come on man it's too easy !!!
[+]Try again boy!



Press any key to continue . . .
```
> Tìm `username` và `serial`.  

## Solution
Xem kiến trúc của file.  
```
└─$ file Keygenme_Part_One.exe
Keygenme_Part_One.exe: PE32 executable (console) Intel 80386, for MS Windows
```

Mở IDA Pro 32 bit và xem pseudocode của hàm `main`.  

```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v3; // edi
  int v4; // eax
  char *v5; // esi
  unsigned int v6; // edi
  int v7; // ecx
  char v8; // cl
  int *v9; // esi
  int v10; // eax
  int v11; // eax
  int *v12; // ecx
  size_t v13; // esi
  size_t v14; // edx
  void **v15; // eax
  char *v16; // edi
  size_t v17; // ecx
  void **v18; // eax
  void **v19; // ecx
  size_t v20; // edi
  size_t v21; // edx
  void **v22; // eax
  char *v23; // esi
  void *v24; // ecx
  int v25; // eax
  void *v26; // ecx
  unsigned __int8 v28; // al
  size_t v29; // ecx
  void **v30; // eax
  void *v31; // ecx
  void **v32; // ecx
  char *v33; // edi
  void **v34; // edx
  int v35; // eax
  size_t v36; // esi
  bool v37; // cf
  unsigned __int8 v38; // al
  unsigned __int8 v39; // al
  unsigned __int8 v40; // al
  int v41; // eax
  int v42; // eax
  int v43; // eax
  char *v44; // eax
  int v45; // [esp+0h] [ebp-430h]
  _BYTE alphanum_string[36]; // [esp+Ch] [ebp-424h] BYREF
  int v47; // [esp+30h] [ebp-400h]
  int v48; // [esp+34h] [ebp-3FCh]
  int v49; // [esp+38h] [ebp-3F8h]
  unsigned int v50; // [esp+3Ch] [ebp-3F4h]
  int *v51; // [esp+40h] [ebp-3F0h]
  int v52; // [esp+44h] [ebp-3ECh]
  unsigned int v53; // [esp+48h] [ebp-3E8h]
  void *Src[4]; // [esp+4Ch] [ebp-3E4h] BYREF
  size_t Size[2]; // [esp+5Ch] [ebp-3D4h]
  int v56; // [esp+64h] [ebp-3CCh]
  char v57; // [esp+6Bh] [ebp-3C5h]
  void *v58; // [esp+6Ch] [ebp-3C4h] BYREF
  int v59; // [esp+7Ch] [ebp-3B4h]
  unsigned int v60; // [esp+80h] [ebp-3B0h]
  void *Block[4]; // [esp+84h] [ebp-3ACh] BYREF
  size_t v62[2]; // [esp+94h] [ebp-39Ch]
  char v63[16]; // [esp+9Ch] [ebp-394h] BYREF
  int v64[2]; // [esp+ACh] [ebp-384h] BYREF
  char v65[16]; // [esp+B4h] [ebp-37Ch] BYREF
  int v66; // [esp+C4h] [ebp-36Ch]
  int v67; // [esp+C8h] [ebp-368h]
  char v68[16]; // [esp+CCh] [ebp-364h] BYREF
  int v69; // [esp+DCh] [ebp-354h]
  int v70; // [esp+E0h] [ebp-350h]
  char v71[16]; // [esp+E4h] [ebp-34Ch] BYREF
  int v72; // [esp+F4h] [ebp-33Ch]
  int v73; // [esp+F8h] [ebp-338h]
  char v74[16]; // [esp+FCh] [ebp-334h] BYREF
  int v75; // [esp+10Ch] [ebp-324h]
  int v76; // [esp+110h] [ebp-320h]
  char v77[16]; // [esp+114h] [ebp-31Ch] BYREF
  int v78; // [esp+124h] [ebp-30Ch]
  int v79; // [esp+128h] [ebp-308h]
  char v80[16]; // [esp+12Ch] [ebp-304h] BYREF
  int v81; // [esp+13Ch] [ebp-2F4h]
  int v82; // [esp+140h] [ebp-2F0h]
  char v83[16]; // [esp+144h] [ebp-2ECh] BYREF
  int v84; // [esp+154h] [ebp-2DCh]
  int v85; // [esp+158h] [ebp-2D8h]
  char v86[16]; // [esp+15Ch] [ebp-2D4h] BYREF
  int v87; // [esp+16Ch] [ebp-2C4h]
  int v88; // [esp+170h] [ebp-2C0h]
  char v89[16]; // [esp+174h] [ebp-2BCh] BYREF
  int v90; // [esp+184h] [ebp-2ACh]
  int v91; // [esp+188h] [ebp-2A8h]
  char v92[16]; // [esp+18Ch] [ebp-2A4h] BYREF
  int v93; // [esp+19Ch] [ebp-294h]
  int v94; // [esp+1A0h] [ebp-290h]
  char v95[16]; // [esp+1A4h] [ebp-28Ch] BYREF
  int v96; // [esp+1B4h] [ebp-27Ch]
  int v97; // [esp+1B8h] [ebp-278h]
  char v98[16]; // [esp+1BCh] [ebp-274h] BYREF
  int v99; // [esp+1CCh] [ebp-264h]
  int v100; // [esp+1D0h] [ebp-260h]
  char v101[16]; // [esp+1D4h] [ebp-25Ch] BYREF
  int v102; // [esp+1E4h] [ebp-24Ch]
  int v103; // [esp+1E8h] [ebp-248h]
  char v104[16]; // [esp+1ECh] [ebp-244h] BYREF
  int v105; // [esp+1FCh] [ebp-234h]
  int v106; // [esp+200h] [ebp-230h]
  char v107[16]; // [esp+204h] [ebp-22Ch] BYREF
  int v108; // [esp+214h] [ebp-21Ch]
  int v109; // [esp+218h] [ebp-218h]
  char v110[16]; // [esp+21Ch] [ebp-214h] BYREF
  int v111; // [esp+22Ch] [ebp-204h]
  int v112; // [esp+230h] [ebp-200h]
  char v113[16]; // [esp+234h] [ebp-1FCh] BYREF
  int v114; // [esp+244h] [ebp-1ECh]
  int v115; // [esp+248h] [ebp-1E8h]
  char v116[16]; // [esp+24Ch] [ebp-1E4h] BYREF
  int v117; // [esp+25Ch] [ebp-1D4h]
  int v118; // [esp+260h] [ebp-1D0h]
  char v119[16]; // [esp+264h] [ebp-1CCh] BYREF
  int v120; // [esp+274h] [ebp-1BCh]
  int v121; // [esp+278h] [ebp-1B8h]
  char v122[16]; // [esp+27Ch] [ebp-1B4h] BYREF
  int v123; // [esp+28Ch] [ebp-1A4h]
  int v124; // [esp+290h] [ebp-1A0h]
  char v125[16]; // [esp+294h] [ebp-19Ch] BYREF
  int v126; // [esp+2A4h] [ebp-18Ch]
  int v127; // [esp+2A8h] [ebp-188h]
  char v128[16]; // [esp+2ACh] [ebp-184h] BYREF
  int v129; // [esp+2BCh] [ebp-174h]
  int v130; // [esp+2C0h] [ebp-170h]
  char v131[16]; // [esp+2C4h] [ebp-16Ch] BYREF
  int v132; // [esp+2D4h] [ebp-15Ch]
  int v133; // [esp+2D8h] [ebp-158h]
  char v134[16]; // [esp+2DCh] [ebp-154h] BYREF
  int v135; // [esp+2ECh] [ebp-144h]
  int v136; // [esp+2F0h] [ebp-140h]
  char v137[16]; // [esp+2F4h] [ebp-13Ch] BYREF
  int v138; // [esp+304h] [ebp-12Ch]
  int v139; // [esp+308h] [ebp-128h]
  char v140[16]; // [esp+30Ch] [ebp-124h] BYREF
  int v141; // [esp+31Ch] [ebp-114h]
  int v142; // [esp+320h] [ebp-110h]
  char v143[16]; // [esp+324h] [ebp-10Ch] BYREF
  int v144; // [esp+334h] [ebp-FCh]
  int v145; // [esp+338h] [ebp-F8h]
  char v146[16]; // [esp+33Ch] [ebp-F4h] BYREF
  int v147; // [esp+34Ch] [ebp-E4h]
  int v148; // [esp+350h] [ebp-E0h]
  char v149[16]; // [esp+354h] [ebp-DCh] BYREF
  int v150; // [esp+364h] [ebp-CCh]
  int v151; // [esp+368h] [ebp-C8h]
  char v152[16]; // [esp+36Ch] [ebp-C4h] BYREF
  int v153; // [esp+37Ch] [ebp-B4h]
  int v154; // [esp+380h] [ebp-B0h]
  char v155[16]; // [esp+384h] [ebp-ACh] BYREF
  int v156; // [esp+394h] [ebp-9Ch]
  int v157; // [esp+398h] [ebp-98h]
  char v158[16]; // [esp+39Ch] [ebp-94h] BYREF
  int v159; // [esp+3ACh] [ebp-84h]
  int v160; // [esp+3B0h] [ebp-80h]
  char v161[16]; // [esp+3B4h] [ebp-7Ch] BYREF
  int v162; // [esp+3C4h] [ebp-6Ch]
  int v163; // [esp+3C8h] [ebp-68h]
  char v164[16]; // [esp+3CCh] [ebp-64h] BYREF
  int v165; // [esp+3DCh] [ebp-54h]
  int v166; // [esp+3E0h] [ebp-50h]
  char v167[16]; // [esp+3E4h] [ebp-4Ch] BYREF
  int v168; // [esp+3F4h] [ebp-3Ch]
  int v169; // [esp+3F8h] [ebp-38h]
  char v170[36]; // [esp+3FCh] [ebp-34h] BYREF
  int v171; // [esp+420h] [ebp-10h] BYREF
  int v172; // [esp+42Ch] [ebp-4h]

  v3 = 0;
  v53 = 0;
  v50 = 0;
  SetConsoleTitleW(L"KeyGenMe Part 1 | Sir_Zed");
  v62[0] = 0;
  v62[1] = 15;
  LOBYTE(Block[0]) = 0;
  v172 = 1;
  v59 = 0;
  v60 = 15;
  LOBYTE(v58) = 0;
  qmemcpy(alphanum_string, "ABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890", sizeof(alphanum_string));
  v64[0] = 0;
  v64[1] = 15;
  v63[0] = 0;
  sub_F71E90(v63, ".-", 2u);
  LOBYTE(v172) = 2;
  v66 = 0;
  v67 = 15;
  v65[0] = 0;
  sub_F71E90(v65, "-...", 4u);
  LOBYTE(v172) = 3;
  v69 = 0;
  v70 = 15;
  v68[0] = 0;
  sub_F71E90(v68, "-.-.", 4u);
  LOBYTE(v172) = 4;
  v72 = 0;
  v73 = 15;
  v71[0] = 0;
  sub_F71E90(v71, "-..", 3u);
  LOBYTE(v172) = 5;
  v75 = 0;
  v76 = 15;
  v74[0] = 0;
  sub_F71E90(v74, ".", 1u);
  LOBYTE(v172) = 6;
  v78 = 0;
  v79 = 15;
  v77[0] = 0;
  sub_F71E90(v77, "..-.", 4u);
  LOBYTE(v172) = 7;
  v81 = 0;
  v82 = 15;
  v80[0] = 0;
  sub_F71E90(v80, "--.", 3u);
  LOBYTE(v172) = 8;
  v84 = 0;
  v85 = 15;
  v83[0] = 0;
  sub_F71E90(v83, "....", 4u);
  LOBYTE(v172) = 9;
  v87 = 0;
  v88 = 15;
  v86[0] = 0;
  sub_F71E90(v86, "..", 2u);
  LOBYTE(v172) = 10;
  v90 = 0;
  v91 = 15;
  v89[0] = 0;
  sub_F71E90(v89, ".---", 4u);
  LOBYTE(v172) = 11;
  v93 = 0;
  v94 = 15;
  v92[0] = 0;
  sub_F71E90(v92, ".-.", 3u);
  LOBYTE(v172) = 12;
  v96 = 0;
  v97 = 15;
  v95[0] = 0;
  sub_F71E90(v95, ".-..", 4u);
  LOBYTE(v172) = 13;
  v99 = 0;
  v100 = 15;
  v98[0] = 0;
  sub_F71E90(v98, "--", 2u);
  LOBYTE(v172) = 14;
  v102 = 0;
  v103 = 15;
  v101[0] = 0;
  sub_F71E90(v101, "-.", 2u);
  LOBYTE(v172) = 15;
  v105 = 0;
  v106 = 15;
  v104[0] = 0;
  sub_F71E90(v104, "---", 3u);
  LOBYTE(v172) = 16;
  v108 = 0;
  v109 = 15;
  v107[0] = 0;
  sub_F71E90(v107, ".--.", 4u);
  LOBYTE(v172) = 17;
  v111 = 0;
  v112 = 15;
  v110[0] = 0;
  sub_F71E90(v110, "--.-", 4u);
  LOBYTE(v172) = 18;
  v114 = 0;
  v115 = 15;
  v113[0] = 0;
  sub_F71E90(v113, ".-.", 3u);
  LOBYTE(v172) = 19;
  v117 = 0;
  v118 = 15;
  v116[0] = 0;
  sub_F71E90(v116, "...", 3u);
  LOBYTE(v172) = 20;
  v120 = 0;
  v121 = 15;
  v119[0] = 0;
  sub_F71E90(v119, "-", 1u);
  LOBYTE(v172) = 21;
  v123 = 0;
  v124 = 15;
  v122[0] = 0;
  sub_F71E90(v122, "..-", 3u);
  LOBYTE(v172) = 22;
  v126 = 0;
  v127 = 15;
  v125[0] = 0;
  sub_F71E90(v125, "...-", 4u);
  LOBYTE(v172) = 23;
  v129 = 0;
  v130 = 15;
  v128[0] = 0;
  sub_F71E90(v128, ".--", 3u);
  LOBYTE(v172) = 24;
  v132 = 0;
  v133 = 15;
  v131[0] = 0;
  sub_F71E90(v131, "-..-", 4u);
  LOBYTE(v172) = 25;
  v135 = 0;
  v136 = 15;
  v134[0] = 0;
  sub_F71E90(v134, "-.--", 4u);
  LOBYTE(v172) = 26;
  v138 = 0;
  v139 = 15;
  v137[0] = 0;
  sub_F71E90(v137, "--..", 4u);
  LOBYTE(v172) = 27;
  v141 = 0;
  v142 = 15;
  v140[0] = 0;
  sub_F71E90(v140, ".----", 5u);
  LOBYTE(v172) = 28;
  v144 = 0;
  v145 = 15;
  v143[0] = 0;
  sub_F71E90(v143, "..---", 5u);
  LOBYTE(v172) = 29;
  v147 = 0;
  v148 = 15;
  v146[0] = 0;
  sub_F71E90(v146, "...--", 5u);
  LOBYTE(v172) = 30;
  v150 = 0;
  v151 = 15;
  v149[0] = 0;
  sub_F71E90(v149, "....-", 5u);
  LOBYTE(v172) = 31;
  v153 = 0;
  v154 = 15;
  v152[0] = 0;
  sub_F71E90(v152, ".....", 5u);
  LOBYTE(v172) = 32;
  v156 = 0;
  v157 = 15;
  v155[0] = 0;
  sub_F71E90(v155, "-....", 5u);
  LOBYTE(v172) = 33;
  v159 = 0;
  v160 = 15;
  v158[0] = 0;
  sub_F71E90(v158, "--...", 5u);
  LOBYTE(v172) = 34;
  v162 = 0;
  v163 = 15;
  v161[0] = 0;
  sub_F71E90(v161, "---..", 5u);
  LOBYTE(v172) = 35;
  v165 = 0;
  v166 = 15;
  v164[0] = 0;
  sub_F71E90(v164, "----.", 5u);
  LOBYTE(v172) = 36;
  v168 = 0;
  v169 = 15;
  v167[0] = 0;
  sub_F71E90(v167, "-----", 5u);
  LOBYTE(v172) = 37;
  system("color 1");
  v4 = sub_F72140(
         std::cout,
         "\n"
         "__    __  ________  __      __   ______   ________  __    __  __       __  ________ \n"
         "|  \\  /  \\|        \\|  \\    /  \\ /      \\ |        \\|  \\  |  \\|  \\     /  \\|        \\\n"
         "| $$ /  $$| $$$$$$$$ \\$$\\  /  $$|  $$$$$$\\| $$$$$$$$| $$\\ | $$| $$\\   /  $$| $$$$$$$$\n"
         "| $$/  $$ | $$__      \\$$\\/  $$ | $$ __\\$$| $$__    | $$$\\| $$| $$$\\ /  $$$| $$__    \n"
         "| $$  $$  | $$  \\      \\$$  $$  | $$|    \\| $$  \\   | $$$$\\ $$| $$$$\\  $$$$| $$  \\   \n"
         "| $$$$$\\  | $$$$$       \\$$$$   | $$ \\$$$$| $$$$$   | $$\\$$ $$| $$\\$$ $$ $$| $$$$$   \n"
         "| $$ \\$$\\ | $$_____     | $$    | $$__| $$| $$_____ | $$ \\$$$$| $$ \\$$$| $$| $$_____ \n"
         "| $$  \\$$\\| $$     \\    | $$     \\$$    $$| $$     \\| $$  \\$$$| $$  \\$ | $$| $$     \\\n"
         " \\$$   \\$$ \\$$$$$$$$     \\$$      \\$$$$$$  \\$$$$$$$$ \\$$   \\$$ \\$$      \\$$ \\$$$$$$$$               "
         "                                                                                            \n"
         "_______    ______   _______  ________         ______   __    __  ________           \n"
         "|       \\  /      \\ |       \\|        \\       /      \\ |  \\  |  \\|        \\          \n"
         "| $$$$$$$\\|  $$$$$$\\| $$$$$$$\\\\$$$$$$$$      |  $$$$$$\\| $$\\ | $$| $$$$$$$$          \n"
         "| $$__/ $$| $$__| $$| $$__| $$  | $$         | $$  | $$| $$$\\| $$| $$__              \n"
         "| $$    $$| $$    $$| $$    $$  | $$         | $$  | $$| $$$$\\ $$| $$  \\             \n"
         "| $$$$$$$ | $$$$$$$$| $$$$$$$\\  | $$         | $$  | $$| $$\\$$ $$| $$$$$             \n"
         "| $$      | $$  | $$| $$  | $$  | $$         | $$__/ $$| $$ \\$$$$| $$_____           \n"
         "| $$      | $$  | $$| $$  | $$  | $$          \\$$    $$| $$  \\$$$| $$     \\          \n"
         " \\$$       \\$$   \\$$ \\$$   \\$$   \\$$           \\$$$$$$  \\$$   \\$$ \\$$$$$$$$       \n"
         "   \n"
         "Level= 1                 For beginners         Coded By : Sir_Zed                                              "
         "                     \n"
         "Rules: 1) No Patching 2) You have to make a keygen and a tutorial. 3) Self Keygen\n"
         "If You have any question : radpak333@gmail.com\n"
         "Good Luck!                                                                              \n"
         "\t\t");
  std::ostream::operator<<(v4, sub_F72390);
  sub_F72140(std::cout, "Enter Username (Only Letters and numbers): ");
  sub_F723C0(v45);
  v5 = v170;
  v56 = &v171 < (int *)v170 ? 0 : 36;
  do
  {
    ++v3;
    *v5 = toupper(*v5);
    ++v5;
  }
  while ( v3 != v56 );
  v6 = v53;
  v7 = 0;
  v56 = 0;
  do
  {
    v8 = v170[v7];
    v9 = v64;
    v10 = 0;
    v57 = v8;
    v52 = 0;
    v51 = v64;
    do
    {
      if ( v8 == alphanum_string[v10] )
      {
        LOBYTE(v172) = 38;
        v11 = *v9 + 1;
        Size[0] = 0;
        Size[1] = 15;
        LOBYTE(Src[0]) = 0;
        v53 = v6 | 1;
        v50 = v6 | 1;
        sub_F72880(Src, v11);
        v12 = v9 - 4;
        if ( (unsigned int)v9[1] >= 0x10 )
          v12 = (int *)*v12;
        v13 = *v9;
        v14 = Size[0];
        if ( v13 > Size[1] - Size[0] )
        {
          LOBYTE(v49) = 0;
          sub_F726C0(Src, v13, v49, (int)v12, v13);
        }
        else
        {
          Size[0] += v13;
          v15 = Src;
          if ( Size[1] >= 0x10 )
            v15 = (void **)Src[0];
          v16 = (char *)v15 + v14;
          memmove((char *)v15 + v14, v12, v13);
          v16[v13] = 0;
        }
        v17 = Size[0];
        if ( Size[1] == Size[0] )
        {
          LOBYTE(v48) = 0;
          sub_F726C0(Src, 1, v48, (int)" ", 1u);
        }
        else
        {
          ++Size[0];
          v18 = Src;
          if ( Size[1] >= 0x10 )
            v18 = (void **)Src[0];
          *(_WORD *)((char *)v18 + v17) = 32;
        }
        v19 = Src;
        if ( Size[1] >= 0x10 )
          v19 = (void **)Src[0];
        v20 = Size[0];
        v21 = v62[0];
        if ( Size[0] > v62[1] - v62[0] )
        {
          LOBYTE(v47) = 0;
          sub_F726C0(Block, Size[0], v47, (int)v19, Size[0]);
        }
        else
        {
          v62[0] += Size[0];
          v22 = Block;
          if ( v62[1] >= 0x10 )
            v22 = (void **)Block[0];
          v23 = (char *)v22 + v21;
          memmove((char *)v22 + v21, v19, Size[0]);
          v23[v20] = 0;
        }
        LOBYTE(v172) = 37;
        v6 = v53 & 0xFFFFFFFE;
        if ( Size[1] >= 0x10 )
        {
          v24 = Src[0];
          if ( Size[1] + 1 >= 0x1000 )
          {
            v24 = (void *)*((_DWORD *)Src[0] - 1);
            if ( (unsigned int)(Src[0] - v24 - 4) > 0x1F )
              goto LABEL_74;
          }
          sub_F72D8B(v24);
        }
        v8 = v57;
        v10 = v52;
        v9 = v51;
      }
      ++v10;
      v9 += 6;
      v52 = v10;
      v51 = v9;
    }
    while ( v10 < 36 );
    v7 = v56 + 1;
    v56 = v7;
  }
  while ( v7 < 36 );
  if ( v62[0] )
  {
    sub_F72140(std::cout, "Enter Serial : ");
    std::istream::ignore(std::cin, 1, 0, -1);
    v28 = std::ios::widen(std::cin + *(_DWORD *)(std::cin + 4), 10);
    sub_F72A20(v28);
    Size[0] = 0;
    Size[1] = 15;
    LOBYTE(Src[0]) = 0;
    v29 = v62[0] - 1;
    if ( v62[0] < v62[0] - 1 )
      v29 = v62[0];
    v30 = Block;
    if ( v62[1] >= 0x10 )
      v30 = (void **)Block[0];
    sub_F71E90(Src, v30, v29);
    if ( v62[1] >= 0x10 )
    {
      v31 = Block[0];
      if ( v62[1] + 1 >= 0x1000 )
      {
        v31 = (void *)*((_DWORD *)Block[0] - 1);
        if ( (unsigned int)(Block[0] - v31 - 4) > 0x1F )
          goto LABEL_74;
      }
      sub_F72D8B(v31);
    }
    v32 = &v58;
    v33 = (char *)v58;
    v34 = Block;
    if ( v60 >= 0x10 )
      v32 = (void **)v58;
    v35 = _mm_cvtsi128_si32(*(__m128i *)Src);
    *(_OWORD *)Block = *(_OWORD *)Src;
    if ( Size[1] >= 0x10 )
      v34 = (void **)v35;
    *(_QWORD *)v62 = *(_QWORD *)Size;
    if ( Size[0] != v59 )
      goto LABEL_66;
    v36 = Size[0] - 4;
    if ( Size[0] < 4 )
    {
LABEL_54:
      if ( v36 == -4 )
        goto LABEL_63;
    }
    else
    {
      while ( *v34 == *v32 )
      {
        ++v34;
        ++v32;
        v37 = v36 < 4;
        v36 -= 4;
        if ( v37 )
          goto LABEL_54;
      }
    }
    v37 = *(_BYTE *)v34 < *(_BYTE *)v32;
    if ( *(_BYTE *)v34 != *(_BYTE *)v32
      || v36 != -3
      && ((v38 = *((_BYTE *)v34 + 1), v37 = v38 < *((_BYTE *)v32 + 1), v38 != *((_BYTE *)v32 + 1))
       || v36 != -2
       && ((v39 = *((_BYTE *)v34 + 2), v37 = v39 < *((_BYTE *)v32 + 2), v39 != *((_BYTE *)v32 + 2))
        || v36 != -1 && (v40 = *((_BYTE *)v34 + 3), v37 = v40 < *((_BYTE *)v32 + 3), v40 != *((_BYTE *)v32 + 3)))) )
    {
      v41 = v37 ? -1 : 1;
      goto LABEL_64;
    }
LABEL_63:
    v41 = 0;
LABEL_64:
    if ( !v41 )
    {
      v42 = sub_F72140(std::cout, "[-]Wow! You're god damn genius!\n[+]Now make a keygen :)");
      std::ostream::operator<<(v42, sub_F72390);
      system("color 2");
LABEL_67:
      std::ostream::operator<<(std::cout, sub_F72390);
      std::ostream::operator<<(std::cout, sub_F72390);
      std::ostream::operator<<(std::cout, sub_F72390);
      system("pause");
      LOBYTE(v172) = 1;
      `eh vector destructor iterator'(v63, 0x18u, 0x24u, sub_F71DA0);
      if ( v60 >= 0x10 )
      {
        v44 = v33;
        if ( v60 + 1 >= 0x1000 )
        {
          v33 = (char *)*((_DWORD *)v33 - 1);
          if ( (unsigned int)(v44 - v33 - 4) > 0x1F )
            goto LABEL_74;
        }
        sub_F72D8B(v33);
      }
      if ( v62[1] < 0x10 )
        return 0;
      v26 = Block[0];
      if ( v62[1] + 1 < 0x1000 )
        goto LABEL_36;
      v26 = (void *)*((_DWORD *)Block[0] - 1);
      if ( (unsigned int)(Block[0] - v26 - 4) <= 0x1F )
        goto LABEL_36;
LABEL_74:
      invalid_parameter_noinfo_noreturn();
    }
LABEL_66:
    v43 = sub_F72140(std::cout, "[-]Come on man it's too easy !!!\n[+]Try again boy!");
    std::ostream::operator<<(v43, sub_F72390);
    system("color 4 ");
    goto LABEL_67;
  }
  v25 = sub_F72140(std::cout, "[*]Don't cheat u little....\n[+](Only Only ___Letters____ and ____numbers____)");
  std::ostream::operator<<(v25, sub_F72390);
  LOBYTE(v172) = 1;
  `eh vector destructor iterator'(v63, 0x18u, 0x24u, sub_F71DA0);
  if ( v62[1] >= 0x10 )
  {
    v26 = Block[0];
    if ( v62[1] + 1 < 0x1000 || (v26 = (void *)*((_DWORD *)Block[0] - 1), (unsigned int)(Block[0] - v26 - 4) <= 0x1F) )
    {
LABEL_36:
      sub_F72D8B(v26);
      return 0;
    }
    goto LABEL_74;
  }
  return 0;
}
```  

Khi xem pseudocode thì phát hiện chuỗi mà chúng ta muốn chương trình in ra là `[-]Wow! You're god damn genius!\n[+]Now make a keygen :)`.  
```c
if ( !v41 )
    {
      v42 = sub_F72140(std::cout, "[-]Wow! You're god damn genius!\n[+]Now make a keygen :)");
      std::ostream::operator<<(v42, sub_F72390);
      system("color 2");
```

Vậy để chương trình in ra chuỗi trên thì `v41` phải bằng `0`.  

Mà để `v41` bằng `0` thì `LABEL_63` phải được thực thi.  
```c
LABEL_63:
    v41 = 0;
```  

Để `LABEL_63` được thực thi thì `LABEL_54` phải được thực thi.  
```c
LABEL_54:
      if ( v36 == -4 )
        goto LABEL_63;
```
