# [Advent of Cyber 2021](https://tryhackme.com/room/adventofcyber3)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 18: Playing With Containers

pull the latest image from GrinchEnterprises Elastic Container Registry (ECR)

```
docker pull public.ecr.aws/h0w1j9u3/grinch-aoc:latest
```

start a shell to interact with the container

```
docker run -it public.ecr.aws/h0w1j9u3/grinch-aoc:latest
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/16a5c4a3-6a50-4bb6-8057-8f07772a302f)

we can do some command here, but our challenge requires that we exit the shell

```
mkdir aoc
cd aoc
docker save -o aoc.tar public.ecr.aws/h0w1j9u3/grinch-aoc:latest
tar -xvf aoc.tar
```

let's read manifest.json using jq

```
cat manifest.json | jq
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4c065059-8fc2-4e5a-9a51-1dc238e0dce9)

we are guided to find the answer to the last question

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2e43a97d-ae64-4b23-9538-613b45c49868)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fa6814c3-f1cb-4f17-a701-c13411d24c41)

```
cat root/envconsul/config.hcl | grep "token"
```

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b5518ee2-98f7-4706-9ef2-9e456aedafc6)

## Day 19: Something Phishy Is Going On

firstly, we have a machine like that

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4d39aa52-05e7-4134-b5c9-14964d79c91f)

open the email.eml on the top panel

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5876396b-2972-40ad-8b84-6919120eb910)

it's like a phishing email, malicious reply and 1 misspelled word

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/163fe0d3-6bdf-4868-bb20-8c8ba98dc457)

on the button also contain a malicious link

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/573af55f-bc26-4efd-92d0-cba1f66faa5e)

click on more/view source, you can see a malicious header

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8c665635-7af2-4159-b88e-4897ffe5efaf)

check the Email Artifacts of other colleagues and read attachment.txt

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3c326511-6c26-46ca-9971-e7e1515b2012)

we need to decode the attachment to a file, you can load the file into CyberChef, decode and save it

in my case, i can use command to decode itself and open it

```
cat attachment-base64-only.txt | base64 -d > file.pdf
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4ef4de67-38a3-46c0-9e23-c823528114fa)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f8ecc006-a1e4-45ec-a7ac-d1a33de13b62)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3646bb97-79ea-41c2-b2ad-91d80c244c58)

## Day 20: What's the Worst That Could Happen?

print all readable content on testfile

```
strings testfile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/123625c6-a89e-4717-be17-e3f7edb86d3d)

check file type

```
file testfile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/88923ec7-043e-401d-a659-c956e4b720e9)

caculate hash and check with virustotal

```
md5sum testfile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/79e8d450-6e1a-42c5-950e-2fb07f92c10a)

on details tab, we can see the timestamp

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d61ea083-2b59-4766-b0ca-0f7dc6293b06)

on author description, i see 2 other name of that file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/550a71dd-87c1-46dd-bcb5-24ea52362ebc)

and the maximum number of total characters that can be in the file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/13baf09d-ea7a-4b60-a7fc-633d200a7ad2)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6cb9cfa1-1892-4e4e-b25d-2a89bd50dc60)

## Day 21: Needles In Computer Stacks

this challenge is all about yara document

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ad6f658a-db0a-44c5-8cdc-f8b938735214)

run the command with the -c option with return 0 or 1 as false or true

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7ccfda05-4e95-46e7-9ef5-b8365a710eae)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ef8b3eee-ec97-47e3-98f5-5cf1e9843d6f)

## Day 22: How It Happened

let's check the doc file with oledump

```
cd C:\Users\Administrator\Desktop\Santa_Claus_Naughty_List_2021
oledump.py Santa_Claus_Naughty_List_2021.doc
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5512a4cf-d568-4477-92d0-3b11f27d0bae)

this shows us the data streams of doc file and we can use it to reference stream index

```
oledump.py -s 8 -d Santa_Claus_Naughty_List_2021.doc
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/71edcd4b-5510-410a-a98f-37344e4b4a65)

copy the base64 and decode with cyberchef

you can use the given recipe from guide

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d834c494-685d-435d-a84d-9dc4963f348f)

we can check another stream, stream 7 seem interesting

```
oledump.py -s 7 -d Santa_Claus_Naughty_List_2021.doc
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e127e2c1-07a9-4ba9-9e46-97e6de6454cc)

now, we are looking for a path the script that we decoded

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f22a9d90-798b-4e26-b351-068d87c04a84)

navigate to `%USERPROFILE%\Pictures\Grinch2021\`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2813206b-47d9-4ed6-a471-1cf6e0570535)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/dff37723-cd0f-41f1-9b69-d36ffd4ed53c)

## Day 23: PowershELlF Magic

open FullEventLogView, click on Advanced Options, filter as the guide

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/11938f7a-14d2-4a06-a918-68d42ef147f7)

the result we have 7 records, first one is someone has download CVE-2021-1675 and save it as grab.ps1

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1ccc6793-b35d-4a22-af23-88a77b619350)

and we alse can see the user execute it at the end

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c55c0a10-1340-4fe0-bb6f-03a0059f62f9)

on elfmcnealy folder, check his Document folder, i see a folder 20211111 that have 2 log files

open first one, he exploit CVE-2021-1675 to create a new user with administrator privilege

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/947ead8a-5ef0-4aca-bc91-a7e9d5932edd)

on adm1n folder, check his Document folder, i see same folder 20211111 that contain 2 log files

i see it related to retrieve administrator's password using a file on elfmcnealy's Desktop

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6e8c4d06-be13-42d7-8f45-229fc069053e)

check the powershell script on elfmcnealy's Desktop, it encrypt password with a key then post to a remote server

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/938fca28-c178-416f-a56f-67929f018e3c)

the script then downloads a sdelete.zip, extract it then run the sdelete.exe the password file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a65073d6-a7e9-43bb-97a4-f1cbd3837784)

now, back to the FullEventLogView Advanced Options, filter with sdelete.exe

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fcedd8d5-650e-44e0-b470-1ff37c573667)

on last record, we has timestamp of the command we need in the log file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9755b421-0644-4244-964b-432767fed0c0)

now we need to retrive actual password file, direct to decryptor.ps1 on the Desktop of the VM

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d8859b57-6466-44f3-a7c6-bd48de343740)

refer to sendit.ps1 on elfmcnealy's Desktop, i think it relevant to the $encryptedData

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e343d32c-58d7-4806-a13f-12d794aa3765)

filter encryptedData by all event id

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/67eea118-2126-43b7-aa04-6b80d859d4db)

on last record, we have details about body of the post form

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e4d9625d-1bc5-4acf-992c-43fc03d11fa9)

copy encrypted data and key in above step into the decryptor.ps1 script to obtain flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a56a7426-518c-44a5-a108-46fae58c9799)

## Day 24: Learning From The Grinch















