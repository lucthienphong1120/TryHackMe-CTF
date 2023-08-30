![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/42649d45-0df8-4668-bbbc-3b57e4dd1c7a)# [Investigating Windows 2.0](https://tryhackme.com/room/investigatingwindows2)

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













