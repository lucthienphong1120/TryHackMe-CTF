# [CMesS](https://tryhackme.com/room/cmess)

> Can you root this Gila CMS box?

## Scanning

scan the machine

```
nmap -A -T4 10.10.29.227
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ca733caf-5386-49c6-ad42-76bf0a244dd6)

## HTTP

before starting, add machine to host

```
echo "10.10.29.227 cmess.thm" >> /etc/hosts
```

check webpage, we have a gila cms

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/32887f06-8966-403c-b4df-71e49ec315ea)

i tested with the searchbar, nothing there

check robots.txt, seem nothing

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c535ee25-4263-4a78-8e67-393fe19110ba)

## Enumeration

as a hint, scan the subdomain

```
gobuster dns -d cmess.thm -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 30
```





.

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
