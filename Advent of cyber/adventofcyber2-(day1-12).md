# [Advent of Cyber 2 [2020]](https://tryhackme.com/room/adventofcyber2)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 1: A Christmas Crisis

go to the webpage, you will see a chrismas control centre

![image](https://user-images.githubusercontent.com/90561566/214785295-fef573af-99a6-427b-bfeb-3294d50cbd9c.png)

register one acc and login, check the cookie

![image](https://user-images.githubusercontent.com/90561566/214785594-933afb60-44e0-49b3-b0c9-35e3ebd6a0a0.png)

decode with cyberchef from hex, we got a json format

![image](https://user-images.githubusercontent.com/90561566/214786429-06a30488-1bfb-46c8-ab81-eca3e35eb084.png)

reencode with username santa

![image](https://user-images.githubusercontent.com/90561566/214787080-9509aab6-67b5-42f1-b649-1b2d50616203.png)

edit the cookie and reload, yah, we got santa's access

![image](https://user-images.githubusercontent.com/90561566/214787254-09f86564-f5ec-4f4a-87d2-b7d1b88fa802.png)

enable all to get flag

![image](https://user-images.githubusercontent.com/90561566/214787424-f1c9e1f9-4a72-4fc3-922a-7116f8c5b4e6.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/214787519-bcc0aab0-9b28-4f56-b45a-63ae0fb411d3.png)

## Day 2: The Elf Strikes Back!

go to the webpage

![image](https://user-images.githubusercontent.com/90561566/215031356-e657878f-4a91-4294-8055-ad0e7e9b816f.png)

provide our id numder `?id=ODIzODI5MTNiYmYw`

![image](https://user-images.githubusercontent.com/90561566/215031951-b6ec3e33-3bfa-4aed-8a19-cbcf65666fef.png)

check source code

![image](https://user-images.githubusercontent.com/90561566/215032120-2d596691-109e-4fa5-87cd-55d2f2861bf9.png)

prepare our shell

```
cp /usr/share/webshells/php/php-reverse-shell.php shell.php
vi shell.php
```

change extension to bypass filter

```
mv shell.php shell.jpg.php
```

![image](https://user-images.githubusercontent.com/90561566/215034419-4b5bb5a3-883b-4cce-bda5-d79194b52c65.png)

go to /uploads, we can see a list of uploaded files here

![image](https://user-images.githubusercontent.com/90561566/215034581-8bcb5625-d934-4df0-88f7-901e8ce8afc8.png)

```
nc -vlnp 4444
```

![image](https://user-images.githubusercontent.com/90561566/215034758-8c9d5b51-49d6-43ef-86df-10752d0ee7ba.png)

```
cat /var/www/flag.txt
```

Answer

![image](https://user-images.githubusercontent.com/90561566/215035035-957dfc91-edc1-471d-9d8a-37b7711f9382.png)

## Day 3: Christmas Chaos

go to the webpage

![image](https://user-images.githubusercontent.com/90561566/215256268-28311024-39f1-4fd6-b9f6-f5c86ca99c9e.png)

catch request login and send to intruder

![image](https://user-images.githubusercontent.com/90561566/215256346-6f66563e-619c-48a0-8833-2b0a7dc23220.png)

set auto positions and attack type is cluster bomd, then go to Payload tab

![image](https://user-images.githubusercontent.com/90561566/215256470-661c823d-9513-4425-9ad5-5893e9cbf5bc.png)

add 3 each, total is 9, start attack

![image](https://user-images.githubusercontent.com/90561566/215256538-4fe503e4-c21f-4270-9b2f-36982d4b6e72.png)

all status is 302, but you can filter by length, it's one return with 255 value

login and here we go, our flag

![image](https://user-images.githubusercontent.com/90561566/215256651-61b5a009-da0c-41ec-b75c-b371372409ac.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/215256681-c7f6353a-c319-4040-a63a-a13b65c41cc0.png)

## Day 4: Santa's watching

view the webpage

![image](https://user-images.githubusercontent.com/90561566/215312098-a4632e49-5224-4118-b5e6-7cff6ddb43f4.png)

```
gobuster dir -w /usr/share/wordlists/dirb/big.txt -u http://10.10.134.163
```

you will found the /api directory

![image](https://user-images.githubusercontent.com/90561566/215312405-123a2d07-da4e-4159-a41c-687ca750e3b4.png)

i found a site-log.php on /api

![image](https://user-images.githubusercontent.com/90561566/215312595-123c04b0-8e44-4b3d-a0ce-0825eb908351.png)

download the provided wordlists

```
wget https://assets.tryhackme.com/additional/cmn-aoc2020/day-4/wordlist -o wordlist.txt
```

```
wfuzz -c -z file,wordlist.txt http://10.10.134.163/api/site-log.php?date=FUZZ
curl http://10.10.134.163/api/site-log.php?date=20201125
```

Answer

![image](https://user-images.githubusercontent.com/90561566/215312928-50c93887-8f5d-4af6-9908-66078b1f2d0d.png)

## Day 5: Someone stole Santa's gift list!

scan the target

```
nmap -sS -sV -T4 10.10.81.14
```

![image](https://user-images.githubusercontent.com/90561566/215713821-073d7927-9469-4cdd-88ec-272a99001b95.png)

there are a training web at port 3000 and our challenge at port 8000 and MySQL version 8.0.22

![image](https://user-images.githubusercontent.com/90561566/215713972-3ab5fe11-bee3-4a42-854e-c6f28cc36a64.png)

view the website at port 8000

![image](https://user-images.githubusercontent.com/90561566/215714939-29a2ef79-d0f4-45e5-a046-a50f48090c94.png)

i found santa's panel at /santapanel

![image](https://user-images.githubusercontent.com/90561566/215715675-e08be8f4-0a1f-45b1-ace3-4f351f516112.png)

bypass the login by basic sqli

```
admin' OR 1=1--
```

after login as santa

![image](https://user-images.githubusercontent.com/90561566/215716058-d0dbc960-9c43-48bd-844c-648783e84022.png)

catch a search request with burpsuite and save to search.txt

```
sqlmap -r search.txt --tamper=space2comment --dbms sqlite
```

![image](https://user-images.githubusercontent.com/90561566/215719302-094db17c-710b-4047-bfab-d5dad7719217.png)

```
sqlmap -r search.txt --tamper=space2comment --dbms sqlite --count
```

there are 3 tables

![image](https://user-images.githubusercontent.com/90561566/215719719-b69bd70d-b772-464c-beed-3f9b2951fc14.png)

```
sqlmap -r search.txt --tamper=space2comment --dbms sqlite -D gift -T sequels --dump
```

dump sequels table

![image](https://user-images.githubusercontent.com/90561566/215719968-229214a7-2d06-484f-af7f-682e5dd0200b.png)

```
sqlmap -r search.txt --tamper=space2comment --dbms sqlite -D gift -T hidden_table --dump
```

dump hidden_table table

![image](https://user-images.githubusercontent.com/90561566/215720305-8df1fab2-7875-4d3f-b4a3-d2de8810c197.png)

```
sqlmap -r search.txt --tamper=space2comment --dbms sqlite -D gift -T users --dump
```

dump users table

![image](https://user-images.githubusercontent.com/90561566/215720658-79367815-78db-4588-ad49-eaf49612d0ba.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/215720797-5a9e3226-a299-4ea4-8d76-e9c15b2ad2b2.png)

## Day 6: Be careful with what you wish on a Christmas night

view the webpage at port 5000

![image](https://user-images.githubusercontent.com/90561566/216258882-a6379ea8-93a1-487a-ba50-e1443cd5903b.png)

try post a comment

![image](https://user-images.githubusercontent.com/90561566/216260805-f334cae7-da79-4d59-b40c-bbcbed425056.png)

Because you can post comments that allow you to save a script and have it execute everytime the page is loaded this is Store XSS

open owasp zap

![image](https://user-images.githubusercontent.com/90561566/216259658-9b892f21-4f1a-41f8-8296-29d7979db1e1.png)

run a automatic scan

![image](https://user-images.githubusercontent.com/90561566/216260362-bed62e49-af40-4d9f-a022-1970a40925b1.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/216261088-8dd9db6c-9cb8-4391-8fce-ab6a6b76db82.png)

## Day 7: The Grinch Really Did Steal Christmas

first open file pcap1.pcap, filter by icmp

![image](https://user-images.githubusercontent.com/90561566/216743505-d2b8fe5e-6d67-42a0-a0f6-129545dee310.png)

filter by http.request.method == GET, you will find an article

![image](https://user-images.githubusercontent.com/90561566/216743808-1ea3b6c6-ac01-45eb-b3b7-82d6025421db.png)

filter ftp with pcap2.pcap, i found a user elfmcskidy

![image](https://user-images.githubusercontent.com/90561566/216744021-61aeed8e-ff0a-4347-9d24-367e0c3d2d34.png)

right click and follow TCP stream

![image](https://user-images.githubusercontent.com/90561566/216744039-ab85b6e0-0615-48bd-9afc-5b322e602b74.png)

sort by protocols, you will see a encrypted protocol is sshv2

![image](https://user-images.githubusercontent.com/90561566/216744253-01f2149f-18c5-418e-a415-ffbde26d6f23.png)

open pcap3.pcap, export http

![image](https://user-images.githubusercontent.com/90561566/216744658-98709ced-9f99-4d2a-8c87-36845af473be.png)

there you go

![image](https://user-images.githubusercontent.com/90561566/216744887-09a71405-5a56-4aae-a4a7-b0e445a3d720.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/216745182-9ead446d-9ea7-4148-a9a1-75cfd395a827.png)

## Day 8: What's Under the Christmas Tree?

scan the target

```
nmap -A -T4 10.10.137.74
```

![image](https://user-images.githubusercontent.com/90561566/216816507-dc3b6506-f5a8-4784-a284-070acfe20843.png)

it's very easy today

Answer

![image](https://user-images.githubusercontent.com/90561566/216816839-7bfcc329-6db9-414c-ac1d-2f271a6f89a6.png)

## Day 9: Anyone can be Santa!































