# TryHackMe CTF writeups

Homepage: https://tryhackme.com/

In here, i wrote writeups my walkthrough for any challenges i have played on TryHackMe

(CTF rooms only and maybe any interesting room)

## Some useful tools

### Nmap

```
nmap -A -T4 <target>
```

+ `-sV`: Probe open ports to determine service/version info
+ `-O`: Enable OS detection
+ `-p <port ranges>`: Only scan specified ports
+ `-sC`: equivalent to --script=default
+ `-A`: Enable OS detection, version detection, script scanning, and traceroute
+ `--script=vuln`: detect vulnerability script on target

### GoBuster

```
gobuster dir -u <url> -w /usr/share/wordlists/dirb/common.txt -t 30
```

+ `dir` – (scan for directories).
+ `-u`: Target URL.
+ `-w`: the wordlist we are using to scan

### Netcat

```
nc -lvnp 4444
```

+ `-l`: Listen
+ `-v`: Verbose
+ `-n`: Do not use DNS
+ `-p`: What port to listen on

### Searchsploit

```
searchsploit <keyword>
```

+ `-m`: mirror download the exploit
+ `-u`: show url to its CVE

### Privilege Escalation

```
sudo -l -l
```

```
find / -user root -perm /4000 2>/dev/null
```

Reference: https://gtfobins.github.io/gtfobins/











