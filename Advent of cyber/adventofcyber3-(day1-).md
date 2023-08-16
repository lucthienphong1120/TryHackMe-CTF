![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1d59ad51-5827-47ed-8f29-00b6182d0555)# [Advent of Cyber 2021](https://tryhackme.com/room/adventofcyber3)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 1: Save The Gifts

after open the site, you will see a panel like that

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2ba36efb-decf-4e49-981a-c242701a1563)

click on activity tab, you can see McSkidy profile with `?user_id=11`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/03e3ab66-e0a1-40c5-a813-ee234aeafb18)

by changing the user id to another, i can see other profile

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3140931d-3e85-4bc5-a6e0-2486c6dd2cb3)

continue find other user, i found the grinch

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b4bc7cee-7981-424c-9b10-5ffb1aeaab5c)

revert all action the grinch did, we got our flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c16777ab-5d27-443b-8d2b-d163e49cbdb9)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2fcd1460-c626-4307-931b-ef7e66e45242)

## Day 2: Elf HR Problems

on statis site, try to register an account and check cookie

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7640834b-2074-4dad-9c55-9dee3d151b33)

use magic decode from cyberchef, we know its value

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e01f2075-d7a8-4907-bafe-fd7728153364)

edit the value username to admin and encode to hex

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ae078e71-7055-41cb-b879-4fe26222926f)

edit the cookie and refresh the page

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/169212a7-f870-4ce3-92f1-566ab57dad04)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9a23811a-40d4-44fe-a097-0fcbc839e976)

## Day 3: Christmas Blackout

enum the directory to find admin dashboard

```
gobuster dir -u http://10.10.108.41/ -w /usr/share/wordlists/dirb/common.txt -t 30
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5457ff1d-fada-4a6a-9773-6a1cc021e2ce)

go to /admin and try to login with default credentials

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3a146657-4b6f-48c3-93b2-ba8f93bea994)

success login with default: administrator:administrator

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bd182971-d5aa-40e1-9f3c-150dabedb02c)

our flag appear on admin panel

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5de467c8-5ca0-48e9-8311-f898e408faa7)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/7822c421-b7f1-4dd4-8cd9-3fdf09dc92c6)

## Day 4: Santa's Running Behind

open burpsuite and intercept a login request

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/15b30611-62ee-4838-aaa9-f3e2f803cae8)

add payload at password

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8aa0e577-8379-453a-bb66-6aaf342fc82a)

load our password provided by task

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/58166197-6277-477b-a089-3333a594cf87)

run the attack and you can see a diffirent record which is our answer

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1e1927bf-442d-4cca-94cb-ab34c426197f)

login and get our flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/de8390ce-e185-43e2-9175-64dec4f82aa1)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/64e44728-0e75-4763-99b6-a1d5a1cc2ba3)

## Day 5: Pesky Elf Forum

log into the forum

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/239011b1-ecc6-4eb5-a03b-5a75403e0117)

go to setting and change password, you will see a link that

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cfaa078a-845e-4bbd-adab-2e1d8bfbaa5c)

if you can somehow trick the Grinch into visiting this URL, it will change their password to pass123

post a comment `<script>fetch('/settings?new_password=pass123');</script>` and wait to grinch got in

now login again with grinch:pass123

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5d4b4b44-984a-4fc4-8145-eec9e8c83eef)

disable the plugin in setting to get flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/72e4746b-af9d-49e6-ab37-40927f22d08c)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d5889b15-66ed-45d1-9119-0b8808da85b7)

## Day 6: Patch Management Is Hard























































































