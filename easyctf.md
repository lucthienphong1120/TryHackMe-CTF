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

