### Solved by superkojiman

This is actually a dll file. Open it in Hopper and there's a function howtouse():

```
            exp_?fnhowtouse@@YAHH@Z:
10001130         sub        esp, 0xb4
10001136         mov        eax, 0x10001080
1000113b         mov        dword [ss:esp+var_B4], eax
1000113e         mov        dword [ss:esp+var_B0], eax
10001142         mov        eax, 0x10001090
10001147         mov        dword [ss:esp+var_90], eax
1000114b         mov        dword [ss:esp+var_84], eax
1000114f         mov        dword [ss:esp+var_80], eax
10001153         push       esi
10001154         mov        eax, 0x100010a0
10001159         mov        edx, 0x10001030
1000115e         push       edi
1000115f         mov        edi, 0x100010e0
10001164         mov        ecx, 0x10001100
10001169         mov        dword [ss:esp+var_74], eax
1000116d         mov        dword [ss:esp+var_60], eax
10001171         mov        eax, 0x10001050
10001176         mov        esi, 0x10001040
1000117b         mov        dword [ss:esp+var_98], edx
1000117f         mov        dword [ss:esp+var_84], edx
10001183         mov        dword [ss:esp+var_6C], edx
10001187         mov        edx, 0x100010f0
1000118c         mov        dword [ss:esp+var_94], edi
10001190         mov        dword [ss:esp+var_64], edi
10001194         mov        dword [ss:esp+var_50], edi
10001198         mov        dword [ss:esp+var_38], eax
1000119c         mov        dword [ss:esp+var_34], eax
100011a3         mov        dword [ss:esp+var_30], edi
100011aa         mov        dword [ss:esp+var_2C], eax
100011b1         mov        dword [ss:esp+var_1C], eax
100011b8         mov        dword [ss:esp+var_8], eax
100011bf         mov        eax, dword [ss:esp+arg_8]
100011c6         mov        dword [ss:esp+var_90], esi
100011ca         mov        dword [ss:esp+var_4C], esi
100011ce         mov        dword [ss:esp+var_40], esi
100011d2         mov        dword [ss:esp+var_4], esi
100011d9         pop        edi
100011da         mov        dword [ss:esp+var_A8], 0x10001070
100011e2         mov        dword [ss:esp+var_A4], 0x10001110
100011ea         mov        dword [ss:esp+var_A0], 0x10001060
100011f2         mov        dword [ss:esp+var_90], ecx
100011f6         mov        dword [ss:esp+var_84], 0x10001010
100011fe         mov        dword [ss:esp+var_74], 0x10001060
10001206         mov        dword [ss:esp+var_6C], edx
1000120a         mov        dword [ss:esp+var_60], 0x100010b0
10001212         mov        dword [ss:esp+var_5C], 0x100010d0
1000121a         mov        dword [ss:esp+var_58], ecx
1000121e         mov        dword [ss:esp+var_4C], edx
10001222         mov        dword [ss:esp+var_48], edx
10001226         mov        dword [ss:esp+var_40], ecx
1000122a         mov        dword [ss:esp+var_2C], 0x10001060
10001235         mov        dword [ss:esp+var_28], 0x10001010
10001240         mov        dword [ss:esp+var_24], ecx
10001247         mov        dword [ss:esp+var_1C], ecx
1000124e         mov        dword [ss:esp+var_18], 0x10001020
10001259         mov        dword [ss:esp+var_14], 0x100010c0
10001264         mov        dword [ss:esp+var_10], 0x100010b0
1000126f         mov        dword [ss:esp+var_4], edx
10001276         mov        dword [ss:esp+ret_addr], 0x10001120
10001281         mov        ecx, dword [ss:esp+eax*4+var_B0]
10001285         pop        esi
10001286         add        esp, 0xb4
1000128c         jmp        ecx
                        ; endp
1000128e         cmp        ecx, dword [ds:0x10003000]                          ; 0x10003000, XREF=sub_10001a31+17
10001294         jne        j_sub_1000164b
10001296         ret        
```

Basically it's copying function pointers into registers. If we examine each function the registers point to we see that they all return a single character. One function returns "M", another "A", another "{", and another "}", which means it's actually building the flag one character at a time. 

Rename the functions to what character they return, and step through the disassembly to build the flag: MMA{fc7d90ca001fc8712497d88d9ee7efa9e9b32ed8}


