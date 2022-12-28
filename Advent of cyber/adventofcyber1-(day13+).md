# [Advent of Cyber 1 [2019]](https://tryhackme.com/room/25daysofchristmas)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 13: Accumulate

scan the target, it seem block the ping, so we do the following

```
nmap -Pn -sV -T4 10.10.84.176
```

![image](https://user-images.githubusercontent.com/90561566/209768620-0ad3fe80-0677-4a38-ad9b-0bbff645c8cc.png)

go to the webpage, it's default IIS of windows

![image](https://user-images.githubusercontent.com/90561566/209768825-e5b1da43-124e-4b76-a947-afa46e058ab3.png)

because this is more difficult than other challenge, the hint suggests to use directory-list-2.3-medium

```
gobuster dir -u http://10.10.84.176/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 30
```

![image](https://user-images.githubusercontent.com/90561566/209769591-34fa63e9-c8c7-43cf-85d3-048f8dfa18b8.png)

go to /retro webpage

![image](https://user-images.githubusercontent.com/90561566/209769457-bc7ce811-2621-4c01-a295-0d0106824d97.png)

it seems like a blog post, i found an author of all blog is someone callled Wade

![image](https://user-images.githubusercontent.com/90561566/209769786-b37838de-bc30-47f7-a309-16321c8650d1.png)

now, we can see all of his catagories and 1 comment

![image](https://user-images.githubusercontent.com/90561566/209769922-95bd96e7-25d9-4617-ad57-0f60c128ac85.png)

let's see it, maybe it's a password `parzival`

![image](https://user-images.githubusercontent.com/90561566/209769989-76857c17-b50c-4353-940a-f12d7c365bab.png)

comeback to ms-wbt-server at port 3389, use `remmina` to connect to RDP 10.10.84.176

![image](https://user-images.githubusercontent.com/90561566/209770459-ed056b89-da93-4a48-8beb-013e200019b1.png)

we can see user.txt at Desktop

![image](https://user-images.githubusercontent.com/90561566/209770608-a9d8da92-43fc-4ac2-8c8a-f978cc329b25.png)

the other app maybe interesting is hhupd.exe, quick research about it

![image](https://user-images.githubusercontent.com/90561566/209771258-bd74471d-d10a-49db-a456-b136af22804c.png)

open it with administrator permission

![image](https://user-images.githubusercontent.com/90561566/209771504-1c8b09d2-f446-4553-8a66-57549463def3.png)

Click on "Show more details" > "Show information about the publisher’s certificate"

![image](https://user-images.githubusercontent.com/90561566/209771625-42d5401b-2dd9-435f-a5fd-4ef1c989bf3f.png)

click on "Issue by: " link, it will open with your browser and no internet

![image](https://user-images.githubusercontent.com/90561566/209771897-de2b76f1-aa1c-49c3-8e0a-5ba589e72546.png)

From here you can escalate your privilege using either the save file dialogue

Ctrl+S and enter C:\Windows\System32\cmd.exe at search folder

![image](https://user-images.githubusercontent.com/90561566/209772396-8c617e98-33b6-4d20-aaed-166d1164c772.png)

now we can use system32 as administrator

![image](https://user-images.githubusercontent.com/90561566/209772535-365a9d32-ea59-46bf-b443-4cce2c070cef.png)

```
cd C:\Users\Administrator\Desktop
type root.txt
```

![image](https://user-images.githubusercontent.com/90561566/209772734-5c2c1636-8aff-4a1c-ad6e-9f5616614525.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/209772894-fc2cb14a-7e77-4295-a81a-9853bd9ea083.png)

## Day 14: Unknown Storage

























