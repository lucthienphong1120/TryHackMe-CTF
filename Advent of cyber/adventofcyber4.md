# [Advent of Cyber 2022](https://tryhackme.com/room/adventofcyber4)

> Get started with Cyber Security in 24 Days - learn the basics by doing a new, beginner-friendly security challenge every day leading up to Christmas.

## Day 1: Someone's coming to town!

solve the puzzle

![image](https://user-images.githubusercontent.com/90561566/205416280-c63d70b8-36bb-44b1-bb81-da0a657897bd.png)

![image](https://user-images.githubusercontent.com/90561566/205416300-af226a24-8f77-4dec-89ba-cfbeb574b5a6.png)

![image](https://user-images.githubusercontent.com/90561566/205416313-faf9d4c0-c1f9-46a8-bd2a-a2da680e5858.png)

![image](https://user-images.githubusercontent.com/90561566/205416322-5402f760-6109-490f-b5a3-0473ba556426.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/205416343-be501ada-0346-4f21-8910-7ea6341d99bf.png)

## Day 2: Santa's Naughty & Nice Log

```
ls
```

![image](https://user-images.githubusercontent.com/90561566/205416597-d2a1b63d-5e69-4478-a8b8-1b76ab9b6b11.png)

search log about santa

```
grep santa webserver.log
```

![image](https://user-images.githubusercontent.com/90561566/205427490-3867393d-72f1-4073-b528-b00f972c5b80.png)

the timestamp is 18/Nov and it's Friday

ip, and list here

![image](https://user-images.githubusercontent.com/90561566/205427623-0ec8d09d-6fb4-4eca-9eb0-34c61c1989c1.png)

```
grep "THM{" *
```

![image](https://user-images.githubusercontent.com/90561566/205427736-079208b8-495f-4e1a-af42-406df1d91714.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/205427785-1ef4c4fd-1d8e-4c33-afed-7f8270198dff.png)

## Day 3: Nothing escapes detective McRed

search for whois information

![image](https://user-images.githubusercontent.com/90561566/205477673-6863c50e-0662-46a8-bd7a-d9ec0a8af0ec.png)

search domain source code in github, open config.php and you 'll see

![image](https://user-images.githubusercontent.com/90561566/205478123-48af054b-f185-494b-a98a-3b920b2a9487.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/205478178-52c055fe-edb7-4c00-bc37-f1d95b962e73.png)

## Day 4: Scanning through the snow

scan the target

```
nmap -A -T4 10.10.98.27
```

![image](https://user-images.githubusercontent.com/90561566/205568647-600bfb5a-0775-4ac5-acc6-3f339c96f700.png)

connect to smb by type `smb://10.10.98.27` on folder

+ Username: ubuntu
+ Password: S@nta2022

![image](https://user-images.githubusercontent.com/90561566/205572532-6fd61e4f-12b3-4c8d-9a0f-7ac24eaf93c0.png)

there are 2 files on /admin

![image](https://user-images.githubusercontent.com/90561566/205573043-c151df54-752d-4a11-9a91-2aa5a17d2727.png)

there are a list of username and password here

![image](https://user-images.githubusercontent.com/90561566/205573719-5d3f27ca-eea5-4896-9cc0-28a80ea3ef8b.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/205574145-c77ee6f4-1afa-44d7-b895-c593cb590830.png)

## Day 5: He knows when you're awake

scan the target

```
nmap -sS -sV -T4 10.10.26.253
```

![image](https://user-images.githubusercontent.com/90561566/205795643-91daec7e-efcc-4291-9c7b-a67fe71e8fa2.png)

bruteforce vnc server

```
hydra -P /usr/share/wordlists/rockyou.txt vnc://10.10.26.253 -v -f
```

![image](https://user-images.githubusercontent.com/90561566/205797695-6d81ea85-4c26-4742-a09f-898fa3a4a552.png)

open remmina in your linux distro

![image](https://user-images.githubusercontent.com/90561566/205795132-9531c268-cf0b-4a82-86cf-44efffb7d31d.png)

connect to vnc server

![image](https://user-images.githubusercontent.com/90561566/205798971-e266a51c-0d4a-4bd7-9e9e-a04cea387562.png)

![image](https://user-images.githubusercontent.com/90561566/205799727-05488110-d1cf-4b01-9c11-114db2e9a0ac.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/205800560-70a54fce-4d8b-470c-a6ce-6ddd908759b5.png)

## Day 6: It's beginning to look a lot like phishing

open eml file with sublime text

![image](https://user-images.githubusercontent.com/90561566/206357657-10ecd22f-c0b8-4531-89d6-1101fe065cc1.png)

decode Message-ID

```
base64 -d message.txt
```

![image](https://user-images.githubusercontent.com/90561566/206357943-65074ddf-da7c-4c1a-b0a8-3f5c20867b94.png)

Go to https://emailrep.io/ and check for sender's email

![image](https://user-images.githubusercontent.com/90561566/206410602-d55700e3-f3d1-4f12-a10a-6c687f2255ec.png)

search for attachment

![image](https://user-images.githubusercontent.com/90561566/206410940-862e835d-bb6e-450f-8793-8b516091b276.png)

extract attachments

```
emlAnalyzer -i Urgent\:.eml --header --html -u --text --extract-all
```

![image](https://user-images.githubusercontent.com/90561566/206413100-db23ef66-4830-4b51-b42b-4216e9faa329.png)

```
cd eml_attachments
sha256sum Division_of....
```

![image](https://user-images.githubusercontent.com/90561566/206414076-b0267bf4-52e4-4ab0-abcf-f1337e1e75cf.png)

check it on virustotal

![image](https://user-images.githubusercontent.com/90561566/206414347-2907f4c9-4860-412c-80f9-9ee5b1c37057.png)

go to behavior tab, search for 2nd tactic Mitre ATT&CK section

![image](https://user-images.githubusercontent.com/90561566/206414838-8f8cdaa5-723e-4817-b2fe-0e15b219cd8a.png)

go deep analysic in https://labs.inquest.net/

![image](https://user-images.githubusercontent.com/90561566/206415811-8252b4d5-afbd-4bb9-99fe-ec4f3bbe0f81.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/206416200-652a0b4c-0804-4398-b820-9935ef887fef.png)

![image](https://user-images.githubusercontent.com/90561566/206416293-e47b6e2a-eb2e-4026-a8b7-0281381dd658.png)

## Day 7: Maldocs roasting on an open fire

open browser, there is a bookmark of cyberchef here

![image](https://user-images.githubusercontent.com/90561566/206486366-7e5d3db1-d420-47d4-9e36-9c497011f9e8.png)

first, add a strings, all printable character, minlen=258

![image](https://user-images.githubusercontent.com/90561566/206488560-817a2156-d448-4dc1-8b70-b4517095f98b.png)

find/replace regex `[\[_\]\n]`

![image](https://user-images.githubusercontent.com/90561566/206489153-4a9da692-3d67-45ff-a5f2-2e225c1c1a11.png)

next, we drop bytes 124 and decode base64

![image](https://user-images.githubusercontent.com/90561566/206490773-461bbade-8855-4a96-8fa9-2e3acf6c38f3.png)

after that, we decode UTF-16LE

![image](https://user-images.githubusercontent.com/90561566/206490980-a1d710f4-492a-4bc2-afdc-81fc5ee0e868.png)

replace regex too ```['()+'"`]```

![image](https://user-images.githubusercontent.com/90561566/206491340-31b89b2b-6e4d-4d76-9491-9f50829bb0ca.png)

replace `]b2H_` with `http`

![image](https://user-images.githubusercontent.com/90561566/206492584-e80357e6-30d6-410c-80fd-c902ec4a7f06.png)

extract url and split with `@`

![image](https://user-images.githubusercontent.com/90561566/206493048-da83ebad-aec7-47b9-9811-1e1d46248411.png)

lastly, defang url

![image](https://user-images.githubusercontent.com/90561566/206493338-26fe1c86-1f53-40a3-959d-ce86d6fdd257.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/206494799-b50ba3ba-e20f-4919-96a4-01cbea059c17.png)

## Day 8: Last Christmas I gave you my ETH

go to remix IDE and upload 2 files to local workspace

![image](https://user-images.githubusercontent.com/90561566/206726837-bb9d2bce-969a-4fc1-ba52-7b5aae273d44.png)

open 2 files and compile both

![image](https://user-images.githubusercontent.com/90561566/206728718-f59333b8-bc8d-4fc5-a8ce-2b7b37796191.png)

go to deploy tab, deploy etherstore first

![image](https://user-images.githubusercontent.com/90561566/206729040-77ca23d8-2441-42f5-8b1a-16352dd18063.png)

add 1 ether and deposite, you will see the balance is increased

![image](https://user-images.githubusercontent.com/90561566/206732631-4cbfb582-d20d-4d9d-b304-3a9a9511b990.png)

switch to another account, deploy attack constract with etherstore address

![image](https://user-images.githubusercontent.com/90561566/206729986-9452ce74-fe05-4eb3-8414-a40592564a4d.png)

if i deposite or withdraw, i can see a pending signal

![image](https://user-images.githubusercontent.com/90561566/206730882-497ef871-e59d-438b-b989-df155e38251e.png)

here is flag

![image](https://user-images.githubusercontent.com/90561566/206731171-8dbeb256-eb76-499d-8fb6-830b6659ea39.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/206731491-21d6c986-323a-4c0c-a7a3-4fe78dbb83c2.png)

## Day 9: 















