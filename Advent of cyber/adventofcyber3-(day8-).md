![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1d59ad51-5827-47ed-8f29-00b6182d0555)# [Advent of Cyber 2021](https://tryhackme.com/room/adventofcyber3)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 8: Santa's Bag of Toys

use Remmina and rdp to santa desktop

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/14bb163e-6efa-4387-9743-230d222af0f8)

open first file on santalaptoplog, scroll down to see os version

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/637fa4c7-bb59-4f22-9d27-5ba16344ef7a)

open the 3rd file to see a command to add a backdoor user

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fd429fd8-dc1b-4834-86e2-37cf1697aea5)

on last log file, you can see a command to copy a file to desktop

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e03465ec-cedc-486d-b109-abcd1e42b988)

at the same file, scroll down a bit to see he encode the file with certutil.exe

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/34dd93b6-ac92-4772-a90d-003b27ec42d7)

decode base64 with cyberchef and save it to file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c9e93404-8cfb-4644-a278-7b5c90bba8a5)

open the file with ShellBagsExplorer on ShellBagsExplorer folder

We see the santarat as folder on the desktop. Opening it we see .github

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bdb1a9c7-fe2f-4dc2-9e19-ca6096381c6b)

Additionally, there is a unique folder named Bag of Toys on the Desktop

navigate to the folder, we have 1 file bag_of_toys.zip

search on github i see the owner of rat

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ff0e217f-74de-4d9a-ba68-584bc117c746)

look around his account, i see a operation-bag-of-toys repo

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8184b663-1cfa-4bbd-a0c7-03c3074aeea3)

back to the log files, open the 2nd log file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/111cf7bb-8d24-4003-aba8-4f7db4ab34a4)

download the zip from the github and open one file with notepad

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d42803d3-a69e-4ca1-878b-dfff841dfb47)

check the commit, you can see a note about password to the original bag_of_toys.uha archive

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/937b77c7-50f4-475d-9813-6bc3560468df)

open bag_of_toys.uha archive with UHARC in the desktop

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c4d57e61-0d96-43e1-b2a3-39e500173e69)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d9072a76-2f30-4d8a-951c-57791bab8360)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/55c29ce6-5c4c-46c6-95e5-0aa19a7a4d8e)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9e220c07-e550-4b73-85b3-9dbf11ad96a9)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3a920d6b-9aee-414c-aa33-5ba2b8d73a71)

## Day 9: Where Is All This Data Going












































