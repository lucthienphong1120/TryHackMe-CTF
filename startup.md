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

let's go back to fpt and upload our shell

```
put shell.php
```











| Flag | user.txt |
| --- | --- |
| Answer | <flag> |

## Privilege Escalation

.

| Flag | root.txt |
| --- | --- |
| Answer | <flag> |
