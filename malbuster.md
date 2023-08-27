# [MalBuster](https://tryhackme.com/room/malbuster)

> You are tasked to analyse unknown malware samples detected by your SOC team.

## Dissecting PE Headers

start the machine, i will use Flare VM based on windows

```
cd C:\Users\Administrator\Desktop\Samples
Get-ChildItem
Get-ChildItem | Get-FileHash -Algorithm MD5 | select Hash,Path
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d1d0de77-5063-45ca-96f3-2f3ac1cdfd09)

if you check first file with REMnux VM using file command

you can see it's PE32 executable, or Portable Executable 32-bit

now, search md5 hash of first file on virustotal

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/471888ed-41e8-45d4-8a54-c2f48bacf15d)

search md5 hash of second file on virustotal

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1d4c5b5c-f39f-4e4b-8dc2-ddf5c0127ebe)

check Details tab, you can see the dll it import

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a4d8ee8d-d68c-4c12-962c-935945a1a499)

and original name of it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ee77ac19-ae42-473d-a350-a3a47b024d42)

check malware signature of 3rd and 4th file on abuse.ch

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/86fee45a-1eb9-437f-8b6a-611f86aaecd5)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e9dfcfef-82fe-4c89-934c-9f1523036921)

open PE-Bear in Flare VM, find the message in DOS_STUB of malbuster_4

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cb344dc8-7a9b-411d-86bc-266959c875c4)

or you can use PE Tree in REMnux VM will return same answer

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/39b660cf-100e-4c0a-a9c4-b2e695094d57)

search md5 hash of 4th file on virustotal, check Details tab to see imported dll

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6565881a-e5e3-46b0-b16d-63f2d21cfcf5)

```
capa malbuster_1 | findstr /I "anti"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3a3ee640-ac73-4f55-9ae9-10d54d0b4ab4)

```
capa malbuster_1 | findstr /I "log keystrokes"
capa malbuster_2 | findstr /I "log keystrokes"
capa malbuster_3 | findstr /I "log keystrokes"
capa malbuster_4 | findstr /I "log keystrokes"
```

you can find the answer in malbuster_3

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ec63e90e-2c77-4237-a18b-1b9567a114db)

```
capa malbuster_4 | findstr /I "DISCOVERY"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2a1c3363-f5ae-4aa4-99f2-08b94f7f582d)

```
strings malbuster_1 | findstr /I "GodMode"
strings malbuster_2 | findstr /I "GodMode"
strings malbuster_3 | findstr /I "GodMode"
strings malbuster_4 | findstr /I "GodMode"
```

you can find the answer in malbuster_2

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ab355894-eb43-466d-aa8e-9e940f453e96)

```
strings malbuster_1 | findstr /I "Mozilla/4.0"
strings malbuster_2 | findstr /I "Mozilla/4.0"
strings malbuster_3 | findstr /I "Mozilla/4.0"
strings malbuster_4 | findstr /I "Mozilla/4.0"
```

you can find the answer in malbuster_1

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e12138fb-b755-4395-a5c0-c2205f2c50e9)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4533458e-8a2d-4a81-8a09-19b2088de76b)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/eb362f5f-23f5-43f8-a3f4-d64d0de67bf8)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4f9c68a2-e3a6-42b0-9d3d-42d2435d474f)
