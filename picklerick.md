# Pickle Rick

> A Rick and Morty CTF. Help turn Rick back into a human!

## Scanning

Scan the target with nmap

```
nmap -A -T4 10.10.197.149
```

![image](https://user-images.githubusercontent.com/90561566/198048801-c6fab2ef-a96b-4a83-b5d4-3c07ff43ae30.png)

I found 2 port open are SSH (22) and HTTP (80)

## HTTP

Go to the webpage

![image](https://user-images.githubusercontent.com/90561566/198049923-948dc644-54cf-469b-9ea6-b6baf279fb4c.png)

It's an username `R1ckRul3s`

![image](https://user-images.githubusercontent.com/90561566/198051564-2b2260db-9d45-47cb-b434-f5827de6e595.png)

## Enumeration

scan directories with gobuster

```
gobuster dir -u http://10.10.197.149/ -w /usr/share/wordlists/dirb/common.txt -t 30 -x php
```

![image](https://user-images.githubusercontent.com/90561566/198052681-c707007f-f40b-4733-82e5-673bdd46ecef.png)

I see a robots.txt seen meaningless `Wubbalubbadubdub`

![image](https://user-images.githubusercontent.com/90561566/198052867-3a41c8d9-179f-48b0-833c-cd1fe52044c5.png)

Go to login page

![image](https://user-images.githubusercontent.com/90561566/198053586-cd22ad1b-80c0-4dcb-aec9-94ae7de50310.png)




## Exploitation

.

| Flag | user.txt |
| --- | --- |
| Answer | <flag> |

## Privilege Escalation

.

| Flag | root.txt |
| --- | --- |
| Answer | <flag> |

