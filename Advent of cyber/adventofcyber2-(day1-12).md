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



























