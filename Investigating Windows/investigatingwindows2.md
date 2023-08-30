# [Investigating Windows 2.0](https://tryhackme.com/room/investigatingwindows2)

> In the previous challenge you performed a brief analysis. Within this challenge, you will take a deeper dive into the attack.

## Investigating Windows 2.0

the first thing i come into the machine is a script run automatically

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c1c70629-e371-4adb-be13-e237bd1860b4)

check task scheduler, i found that task here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4fe6c763-20df-4beb-b093-b47ef966abb2)

check all event, it shows that 2 malicious process are `mim.exe` and `powershell.exe`

open regedit, search for registry key contain the command `sekurlsa::LogonPasswords`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/632e0df8-7ec0-4a7f-a14b-732e8a006a7f)

noticed that we have Loki (IOC scanner) installed on this machine

```
cd C:\Users\Administrator\Desktop\Tools\loki_0.33.0\loki
loki.exe > lokiscan.txt
```

note that LOKI takes a few minutes to run. Get up, stretch, have a cup of coffee, and come back in ten minutes 

near the top of result, you can see a program return an event filter when attempt to open

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/66a5e4f4-3659-4172-b606-4a378c52c4cb)

under that, i see it run 2 scripting engines call `LaunchBeaconingBackdoor` and `KillProcess`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/79b6c316-cf0d-4dcd-a776-d0e9ecf97d76)

you can find the copyright of software company create this script

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/51616362-f662-487e-8349-b7dd9bb6b6a5)

that can find 2 websites of it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ddfda778-671e-40d8-967f-0204dd4d5926)

search for the previous script, you can find it apprear with `WMIBackdoor.ps1`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/73f95c9a-76b8-44ed-9d36-8676ea2a8705)

open procmon64.exe in Tools\SysinternalsSuite, filter process `mim.exe`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/093cb360-401f-4b5d-bbb7-6d899bf8a669)

when it start again, we will get result like this

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3712f385-c79d-4b72-b6ea-023e2004d977)

on process start event, i see the parent pid 936

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d9147876-237a-4b14-9268-4df7b3ff4481)

and it's svchost.exe

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e52ba205-fd65-4c2e-9165-709aff8fdd5b)

open Process Hacker (troubleshoot process tools) in Tools, inspect the disk operations

when the malicious powershell event start, you get the result like this

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5987d93d-0717-42be-858c-ba7de4dad6fb)

find `Init` module from lokiscan output, i get `WMIScan` and 2nd, 4th warning name

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/699dc524-43de-4350-8ca9-0f73d73a6574)

from loki output FIRST_BYTES section find 4d5a90000300000004000000ffff0000b8000000

from DESC section

from FILE section

from MATCHES section

from FILE/INFO section

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f4185fd9-910d-4816-bf20-3c0b591dee04)

loki found a xor-encrypted binary (Derusbi trojan) on xCmd.exe

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0f336ada-8e8a-4a65-8fce-33aa2e8a66eb)

remember pid 916 under name svchost, that masquerade itself as a legitimate core Windows process/image

loki raised an alert of it at C:\Users\Public\ instead of C:\Windows\System32

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0026c655-e475-4781-afe0-073d976f788a)

from DESC section

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/081be7ca-2b19-4ea5-bdd9-ef41289bca8f)

at same location, i see en-US.js also tagged as hacktool

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4e6f76f3-5ee5-4c0f-b318-390fc5aacc01)

loki seem very very useful, but the problem is it didn't detect our malicious mim.exe

now, we need to complete the regex of the provided yara rule

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0fd93063-e69f-4d4e-a39f-f400212114ca)

we can help ourselves with strings.exe from SysInternals suite to test our regexps through findstr:

```
strings.exe C\TMP\mim.exe | findstr "??.?x?"
strings.exe C\TMP\mim.exe | findstr "...exe"
strings.exe C\TMP\mim.exe | findstr "mk.exe"
```

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/27867d20-035e-437e-a5da-f9a2c109a265)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/67c8b600-e846-4e91-9098-bb2d19ec7a7c)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9a5a0975-cb20-4b29-b2fd-1c6254d6d884)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0a909bc2-09f9-4907-9db6-be8beaccd42d)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/05393daf-10bb-4b1a-a23c-18221c400520)
