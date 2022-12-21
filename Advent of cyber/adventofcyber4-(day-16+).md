# [Advent of Cyber 2022](https://tryhackme.com/room/adventofcyber4)

> Get started with Cyber Security in 24 Days - learn the basics by doing a new, beginner-friendly security challenge every day leading up to Christmas.

## Day 16: SQLi’s the king, the carolers sing

login to the code editor web

![image](https://user-images.githubusercontent.com/90561566/208238951-5dbdf03c-8915-4e81-bc4b-c18344efb5fb.png)

firstly, fix the elf.php with intval() function at line 4 and line 17

![image](https://user-images.githubusercontent.com/90561566/208239213-fee36b8b-9de0-4ef6-9d02-c1014ff751c8.png)

secondly, change it to prepare statement at search-toys.php

![image](https://user-images.githubusercontent.com/90561566/208239364-612d190b-f283-4e73-b0a0-83ddfb738fd1.png)

fix the toy.php with intval() function at line 4 and line 30

![image](https://user-images.githubusercontent.com/90561566/208239598-1849c47a-1715-46fc-977f-05b2aab2a066.png)

last one, fix the login.php with prepare statement

![image](https://user-images.githubusercontent.com/90561566/208239914-4e34c1f6-d918-4597-8907-8937146b5a4a.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/208239946-295dd51b-e96c-4f96-bb25-11587f6b278a.png)

## Day 17: Filtering for Order Amidst Chaos

this is a regex challenge, our job is filtering some words in that file

![image](https://user-images.githubusercontent.com/90561566/208287514-a910bd43-75c5-4ff1-bb0e-5d5f5bf9d422.png)

filter username

```
egrep '^[a-zA-Z0-9]{6,12}$' strings
```

![image](https://user-images.githubusercontent.com/90561566/208287658-f54fa92e-b091-48aa-9388-75b977da900c.png)

filter email

```
egrep '^.+@.+\.com$' strings
```

![image](https://user-images.githubusercontent.com/90561566/208287976-3bd87580-946a-4c11-b9b5-8be276abdbbf.png)

domain filter

```
egrep '^https?://(www.)?.+\..+' strings
```

![image](https://user-images.githubusercontent.com/90561566/208288210-710ff49a-a5f2-4ef2-a014-c3e159c7ea8e.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/208288270-87343200-0de1-4b0b-8cdb-1d0629b75d78.png)

![image](https://user-images.githubusercontent.com/90561566/208288278-4f12f4e7-8a63-426e-adb4-9fe351f885f5.png)

## Day 18: Lumberjack Lenny Learns New Rules

they give us a website with sigma template and we must writeout the rule for it IOC

![image](https://user-images.githubusercontent.com/90561566/208444236-456d1554-8e71-4658-a7bc-dca135ebcf5b.png)

before starting, you need remember some identifying characteristics below

![image](https://user-images.githubusercontent.com/90561566/208446122-209c33a3-d125-45c0-a462-419d9486a2ff.png)

firstly, edit the local_account_creation.yml

![image](https://user-images.githubusercontent.com/90561566/208445533-85c00bc7-6ad1-42ef-b734-0a50280a6bbb.png)

after finished, click on view log, a detection log has been captured

![image](https://user-images.githubusercontent.com/90561566/208445932-72b74eed-9f0d-413c-9134-27f34a9ee8f8.png)

next, edit the susp_software_discovery.yml

![image](https://user-images.githubusercontent.com/90561566/208448128-64dba09b-735e-4cfd-ac40-9760d5ed359c.png)

finally, edit the schtasks_creation.yml

![image](https://user-images.githubusercontent.com/90561566/208449251-8a99201e-ae45-4633-9559-7514c697dbfe.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/208449447-73528265-2ec1-451f-9911-69ec2c161566.png)

## Day 19: Wiggles go brrr

open the Logic 2.4.2 on Desktop with santa captured file to start analysis, ctrl and wheel your mouse to zoom out the signal

![image](https://user-images.githubusercontent.com/90561566/208622560-8f9e87a1-0289-4436-a21c-7d33ba934332.png)

zoom in at the first think line of D1 channel 1

![Untitled](https://user-images.githubusercontent.com/90561566/208624685-09c77009-0363-4823-9cb2-12a2c0647a91.png)

click on analyzer tab, choose async serial, let's analysis channel 1, Bit rate 4800

![Untitled](https://user-images.githubusercontent.com/90561566/208625727-29366b7f-3ef9-410d-b72c-41111662ebf7.png)

on the terminal icon, you can see the data

![image](https://user-images.githubusercontent.com/90561566/208626026-3762db2d-203b-44d7-9170-efbbfcb07471.png)

click on the plus and add another async serial, do the same with channel 0, Bit rate 4800

![image](https://user-images.githubusercontent.com/90561566/208626493-ccbf324b-9972-4962-9b30-7662bfacd833.png)

add another with channel 0 but Bit rate is 9600

![Untitled](https://user-images.githubusercontent.com/90561566/208627223-611b0760-b03b-4f93-ac8d-9228f8179a59.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/208627725-07e91f41-6bdf-4951-a364-84c71c17227e.png)

![image](https://user-images.githubusercontent.com/90561566/208627752-9cf2c012-ce5a-47eb-abd8-0f6abc4ebbfd.png)

## Day 20: Binwalkin’ around the Christmas tree



















