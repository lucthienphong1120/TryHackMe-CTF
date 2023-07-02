# [Advent of Cyber 2 [2020]](https://tryhackme.com/room/adventofcyber2)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 18: The Bits of Christmas

login to machine using provided credentials

![image](https://user-images.githubusercontent.com/90561566/224522683-8f6d933d-3b60-4434-ad4d-46738457e956.png)

![image](https://user-images.githubusercontent.com/90561566/224522801-9ba685d0-83fb-4d9d-8bb3-905c94b15ceb.png)

run ILSpy and open TBFC_APP.exe

![image](https://user-images.githubusercontent.com/90561566/224522879-265c1cc5-1371-4d28-af84-bda60cb011cf.png)

i found a strange component "Crackme"

![image](https://user-images.githubusercontent.com/90561566/224522927-185d6902-475a-4f47-b42f-099e722a6e8b.png)

see in the MainForm, maybe it's our panel

![image](https://user-images.githubusercontent.com/90561566/224522951-37e10504-4518-4cf2-abd9-5d3c72233833.png)

on buttonActivate_Click(), you will found santa's password and flag here

![image](https://user-images.githubusercontent.com/90561566/224522997-4a4de20f-4083-4644-8c52-72d89eaf5ab8.png)

```
// CrackMe.MainForm
using System;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;
using System.Windows.Forms;

private unsafe void buttonActivate_Click(object sender, EventArgs e)
{
  IntPtr value = Marshal.StringToHGlobalAnsi(textBoxKey.Text);
  sbyte* ptr = (sbyte*)System.Runtime.CompilerServices.Unsafe.AsPointer(ref <Module>.??_C@_0BB@IKKDFEPG@santapassword321@);
  void* ptr2 = (void*)value;
  byte b = *(byte*)ptr2;
  byte b2 = 115;
  if ((uint)b >= 115u)
  {
    while ((uint)b <= (uint)b2)
    {
      if (b != 0)
      {
        ptr2 = (byte*)ptr2 + 1;
        ptr++;
        b = *(byte*)ptr2;
        b2 = (byte)(*ptr);
        if ((uint)b < (uint)b2)
        {
          break;
        }
        continue;
      }
      MessageBox.Show("Welcome, Santa, here's your flag thm{046af}", "That's the right key!", MessageBoxButtons.OK, MessageBoxIcon.Asterisk);
      return;
    }
  }
  MessageBox.Show("Uh Oh! That's the wrong key", "You're not Santa!", MessageBoxButtons.OK, MessageBoxIcon.Hand);
}
```

line 10 is santa's password and line 30 is our flag

![image](https://user-images.githubusercontent.com/90561566/224523110-21f289d5-9a71-48a3-b73a-2bbc7ef0b9a2.png)

Answer:

![image](https://user-images.githubusercontent.com/90561566/224523119-e920e611-7783-4753-bac7-fe3a6163f380.png)

## Day 19: The Naughty or Nice List

open the webserver, we be able to see a website to search name and santa's login panel

![image](https://user-images.githubusercontent.com/90561566/225262034-d1fb0404-9b28-4040-962c-0e660188eeb5.png)

try to search for 'test', i see the url is processed with a parameter `proxy`

![image](https://user-images.githubusercontent.com/90561566/225262561-4532e0c4-82da-446a-9e6a-ff280cf0e047.png)

as the instruction, i will provide with `proxy=http%3A%2F%2Flist.hohoho.localtest.me` to see what is going on localhost

![image](https://user-images.githubusercontent.com/90561566/225263270-fafb94fa-4847-4e8c-b285-f57222151f61.png)

so, we have santa's password, it's too long wow

login with `Santa` and `Be good for goodness sake!`

![image](https://user-images.githubusercontent.com/90561566/225264802-10a469e9-d5ab-4fc5-a65e-052aaa533353.png)

here is our flag

![image](https://user-images.githubusercontent.com/90561566/225265002-66afa2c3-2b13-4b19-8c6d-52b25fe01926.png)

Answer:

![image](https://user-images.githubusercontent.com/90561566/225265093-7b2f758d-0030-4e7e-a9b9-b337946869a5.png)

## Day 20: PowershELlF to the rescue

ssh to the machine

```
ssh mceager@10.10.73.111
r0ckStar!
```

enter powershell mode

```
powershell
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/58af1623-9848-41cb-a85b-446fc70a08d1)

you can using PS or linux cmdlet are both okay

```
ls # or Get-ChildItem
cd Documents # or Set-Location Documents
ls -h # or Get-ChildItem -File -Hidden
cat elfone.txt # or Get-Content elfone.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0d3b6e29-f479-426f-bca6-fa54d8265cb1)

```
Set-Location ../Desktop
Get-ChildItem -Directory -Hidden
Set-Location elf2wo
Get-ChildItem -File
Get-Content e70smsW10Y4k.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/0bce3566-9e1d-4dc7-bc58-ce67d9e1193b)

```
Get-ChildItem -Path C:\Windows -Directory -Recurse -Hidden -Filter '*3*' -ErrorAction SilentlyContinue
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/366be101-8099-4a5f-a65d-9e84c490809d)

```
Set-Location C:\Windows\System32\3lfthr3e
Get-ChildItem -File -Hidden
```

there are 2 files here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8161d9cb-8f2c-442b-900e-f437a55528e8)

count the words in first file and get 2 words

```
Get-Content 1.txt | Measure-Object -Word
(Get-Content 1.txt)[551]
(Get-Content 1.txt)[6991]
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9628cef8-fd9b-4f7f-b165-7da37a89769c)

find Elf 3 want

```
Select-String -Path 2.txt -Pattern "redryder"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3e43f6aa-ccc3-4261-b003-a654e63b0fd7)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/09a9744e-48d8-439e-8741-2432999bd6dd)

## Day 21: Time for some ELForensics

connect to the machine using Remmina

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ab6932b3-96b8-4d53-96cf-b2d3b83242da)

```
Get-ChildItem
Set-Location Documents
Get-ChildItem -File
Get-Content 'db file hash.txt'
Get-FileHash -Algorithm MD5 deebee.exe
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5a090cae-37d4-4e03-8436-490436bf3225)

find the hidden flag on exe file

```
C:\Tools\strings64.exe -accepteula .\deebee.exe | Select-String -Pattern "THM"
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/28a0b985-fc61-4746-a3f0-e4ea750a984b)

run the database connector file and capture the alternate data stream

```
.\deebee.exe
Get-Item -Path deebee.exe -Stream *
wmic process call create $(Resolve-Path ./deebee.exe:hidedb)
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a3236ea0-c330-408a-b32c-c518d702d274)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ee8000b6-d754-4a91-bab6-11669b6e22ed)

## Day 22: Elf McEager becomes CyberElf







































