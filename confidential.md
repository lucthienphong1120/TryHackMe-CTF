# [Confidential](https://tryhackme.com/room/confidential)

> We got our hands on a confidential case file from some self-declared "black hat hackers"... it looks like they have a secret invite code.

## Overview

we have a pdf file contain our flag on a hidden qr code

![image](https://user-images.githubusercontent.com/90561566/221769613-644bc2fa-44d2-4aee-a6df-c42e51ce6cfe.png)

i will transfer it to my machine for easier work

my machine

```
nc -vlnp 1234 > Repdf.pdf
```

target machine

```
nc 10.9.43.204 1234 < Repdf.pdf
```

![image](https://user-images.githubusercontent.com/90561566/221771183-718c751f-cf7a-4cbd-b751-dd2f8fc744fe.png)

## Forensics

i will use binwalk for searching binary images

```
binwalk Repdf.pdf
```

![image](https://user-images.githubusercontent.com/90561566/221771807-8fcb0290-c135-4592-969e-83a30b8d2881.png)

hmm, nothing usefull

so, we can try with pdfimages

```
pdfimages -all Repdf.pdf imgs
```

![image](https://user-images.githubusercontent.com/90561566/221773249-373a1170-50ea-447a-8019-1690b7cc06b9.png)

finally, we extracted main file to 3 component files

![image](https://user-images.githubusercontent.com/90561566/221773416-2cb687f8-a8eb-4b39-b73a-65d5d7729516.png)

here you go

![image](https://user-images.githubusercontent.com/90561566/221774512-f2eacc3f-81dc-4c83-8c3e-05b9adeccab4.png)

| Flag | root.txt |
| --- | --- |
| Answer | flag{e08e6ce2f077a1b420cfd4a5d1a57a8d} |
