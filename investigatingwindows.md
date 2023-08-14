# [Investigating Windows](https://tryhackme.com/room/investigatingwindows)

> A windows machine has been hacked, its your job to go investigate this windows machine and find clues to what the hacker might have done.

## Investigating Windows

check the os version

```
systeminfo
```

there is a windows server 2016 datacenter

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d4f193ff-0717-416c-b609-bed07a1de739)

list all users

```
net user
```

you will see 2 users, check the Administrator

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e733b92f-f697-4f0d-98c7-a4a7da15fd3f)

check John login last

```
net user John
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/57090330-6d9a-41c7-8d70-68f2c3bc6e3d)

when booting the machine, you will notice an ip

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/69401b8d-abbd-48af-86a3-acd01ff32ddb)

right click on windows and go to computer management

on Local users and groups/groups/administrator, you can see 2 other users

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/dae51dcc-1d0e-46b2-8686-27d57ca536ca)

check Task Scheduler, i found a very malicious task

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5ab4bd65-058f-4be6-a37f-5231768a81da)

that task run a nc.ps1 at port 1348 everyday

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8d7c8104-e2fc-4c67-9f89-1ac2042acf5b)

check Jenny last login

```
net user Jenny
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d8399983-9046-41e6-801c-f76c2dd8e4de)

maybe you know, sometime the windows popup some terminal that running a program in C:\TMP

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/466d79b1-6466-4ec5-a280-b8d9b1cf0b6c)

check the malicious folder, it appear the first time in 3/2/2019

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e011882f-5093-4a67-beda-5a4df5929533)

check the Event Viewer at that time, filter the Windows Security logs

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/edfe8817-9999-4f40-8c22-90cca901064b)

you can see the only event has second in 49s

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d880fcfc-5643-4a07-82ea-94cff772d178)

in the malicious C:\TMP folder, i see a min-out file is the output of well-known tool Mimikatz

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/083f3789-ca60-48ed-b7f8-6b3461a7d353)

in general, an attacker will oftentimes add the C2 server IP address to the hosts file, we got 76.32.97.132

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/85fe53ea-42b6-44d5-9e14-4214b4da5459)

check the location of windows server website at C:\inetpub\wwwroot

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/89849999-07b3-4179-a11d-67ff5c05e6f3)

to find last port the attacker opened, you need to look at firewall

in inbound rules, you may see a suspicious rule here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6254b839-fa0d-45e9-becb-74e7ed2fae96)

the port is 1337

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f8a30d2d-c100-40f4-ad9b-344aae5c5deb)

check the hosts file for DNS poisoning at C:\Windows\System32\drivers\etc

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/85fe53ea-42b6-44d5-9e14-4214b4da5459)

there are some malicious dns that point to famous website google.com
