# NoBastian
Universal Ring3 IPC based BattlEye/EAC/FaceIt/ESEA/MRAC bypass

# Instructions
You should inject the server.dll into whitelisted process that has a handle to the game.
Injected code automatically opens up a named pipe. Client connects to a pipe and uses it to perform synchronous IPC I/O to perform certain operations with an existing host's remote handle. Client process must have admin rights.

Errors some people get when connecting to pipe:
0x2 (ERROR_FILE_NOT_FOUND) - client couldn't open pipe because it doesn't exist.

0x5 (ERROR_ACCESS_DENIED) - pipe exists, but client can't connect to pipe because client runs not as admin.

Can't inject into host process? Use an injector with handle elevation capabilities, for example @Striekcarl's Loadlibrayy

**Capabilities:**
- Force server to use certain handle
- Force server to automatically select existing handle with certain access mask
- Remote ReadProcessMemory, WriteProcessMemory, VirtualAllocEx calls
- More features and bugfixes are coming soon!

**Detection vectors:**
- Presence of named pipe opened from inside whitelisted process and hence presence of shared memory section in the whitelisted process
- Presence of a thread having it's context somewhere outside of any registered module, e.g somewhere in a manually mapped image. I'm saying "manually mapped" because i hope you're not retarded and not injecting into the host/server process via LoadLibraryA/LdrLoadDll call

**Other ways of implementing RPC:**
- LPC/ALPC ports 
- Shared memory section with a mutex or/and a spinlock 
- Network sockets
- RtlRemoteCall on any remote routine (it does direct thread context hijacking via NtSuspendThread -> GetThreadContext -> SetThreadContext -> NtResumeThread). IMO it's most flexible and simple way if done properly. Sadly, you won't find much information about RtlRemoteCall, but, if you decompile it, you can understand how to use it or you can code your own analogue that suits your needs. The function is undocumented and is exported via ntdll.dll. 

**Patchability:**
- It would be kinda difficult to painlessly strip csrss.exe/lsass.exe handles due to windows internal architecture, so this bypassing method will probably work for a long time

UC Link : https://www.unknowncheats.me/forum/downloads.php?do=file&id=22073

Memory read/write test screenshot (included in ABClient/Main.cpp): https://image.prntscr.com/image/bbZnESi6RPW0-aVSMmx9SQ.png

The code is not a big deal at all and i don't pretend like it is. You can rewrite it from scratch in few hours. The concept has been proposed long time ago, implemented, and is being used by many people on this forum, but I've never seen copypaste-ready source so i decided to post mine. If you decide to C&P this, i suggest you atleast encrypting strings lol

# Contact
+ Discord : asmjs#3945