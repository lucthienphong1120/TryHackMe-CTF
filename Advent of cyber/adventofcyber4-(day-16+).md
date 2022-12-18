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

## Day 18: 



















