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

some time you can see it popup some terminal at C:\TMP, check it folder

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/da28dc42-63bb-463f-9064-52d508781e75)






































