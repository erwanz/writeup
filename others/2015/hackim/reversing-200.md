###Solved by bitvijays

We were provided a with a upx.zip file. Unzipping it provided us with a windows executable file

```
bitvijays@kali:~/Desktop/CTF/Nullcon/rev$ file upx.exe 
upx.exe: PE32 executable (GUI) Intel 80386, for MS Windows
```
Executing it in Windows XP provided us with Error Dialog

![](/images/2015/hackim/reversing200/upx1.png)

Searching Google for Reversing Engineering with Ollydbg resulted in <a href="http://resources.infosecinstitute.com/reverse-engineering-ollydbg/">Reverse Engineering with OllyDbg</a>. Following the step in the guide i.e Viewing the call stack (Alt-K), show call etc. We reached to the point where the Error Message Dialog Box is called. It has a interesting string "-pl34se-give-me-th3-k3y", it also has kernel32.GetCommandLineA. Maybe it read the command line arguments and need "-pl34se-give-me-th3-k3y"

![](/images/2015/hackim/reversing200/upx2.png)

Scrolling up the code we found that there is a check of IsDebuggerPresent also there is a code to output the flag as a debug(kernel32.OutputDebugStringA) and messagebox with String "Since you asked nicely...here you go".

![](/images/2015/hackim/reversing200/upx3.png)

If we check the second image, there are commands 
```
08001160   85C0             TEST EAX,EAX       #Checking for commandline argument "-pl34se-give-me-th3-k3y"
08001162   75 07            JNZ SHORT 0800116B #Call the "You didn't ask Nicely Messagebox"
08001164   E8 97FEFFFF      CALL 08001000      #Refer to commands at the address, Checks for the Debugger present check, Do some processing and output the flag.
08001169   EB 14            JMP SHORT 0800117F
```
First part of analysis is done, Now we need to reach this code, let's try to put a breakpoint at 0x8001160 and we get a warning, maybe because the code is generated at runtime

![](/images/2015/hackim/reversing200/upx4.png)

So the way to reach this code is to do Step In (F7) and Step Over (F8). With multiple running of instances, testing and putting multiple breakpoints, a statement at 0x00401686 CALL EAX takes the execution at 0x8001536
```
0040166A  |. 68 541A4100    PUSH upx.00411A54                        ;  ASCII "[+] Executing Entry Point: 0x%08x
"
0040166F  |. 8945 F8        MOV DWORD PTR SS:[EBP-8],EAX
00401672  |. E8 C4500000    CALL upx.0040673B
00401677  |. 83C0 40        ADD EAX,40
0040167A  |. 50             PUSH EAX
0040167B  |. E8 70520000    CALL upx.004068F0
00401680  |. 83C4 0C        ADD ESP,0C
00401683  |. 8B45 F8        MOV EAX,DWORD PTR SS:[EBP-8]
00401686  |. FFD0           CALL EAX
```

By pressing F8 (Step Over), multiple functions calls and step into (F7) at 
```
08001507   E8 F4FBFFFF      CALL 08001100
```
changing the value of EAX manually to 0 at
```
08001160   85C0             TEST EAX,EAX
```
and passing the check of IsDebuggerPresent at 
```
08001024   FF15 08A00008    CALL DWORD PTR DS:[800A008]              ; kernel32.IsDebuggerPresent
0800102A   85C0             TEST EAX,EAX
```
We get to the Debugoutput
```
080010E5   FF15 04A00008    CALL DWORD PTR DS:[800A004]              ; kernel32.OutputDebugStringA
```

and got the flag

![](/images/2015/hackim/reversing200/upx5.png)

The flag is **flag{s1mpl3-c0mpr3ss10n-f0r-fun&p}**



