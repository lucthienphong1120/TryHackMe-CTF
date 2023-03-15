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


















