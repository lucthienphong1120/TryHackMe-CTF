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

Answer:

![image](https://user-images.githubusercontent.com/90561566/203927756-1a7a26e4-e544-40bd-aa7a-84d53c3e22db.png)














