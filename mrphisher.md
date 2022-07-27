# [Mr. Phisher](https://tryhackme.com/room/mrphisher)

> I received a suspicious email with a very weird looking attachment. It keeps on asking me to "enable macros". What are those?

## Deploy the machine

We see 2 files

![image](https://user-images.githubusercontent.com/90561566/181204033-2973b41f-2b4b-4080-86d8-7b27354ef0b3.png)

Open file .docm and we will see

![image](https://user-images.githubusercontent.com/90561566/181204209-3417bdd0-2516-4418-9f70-7b0bdf8e5e70.png)

## View macro

We choose Tools/Macro/Edit macro

![image](https://user-images.githubusercontent.com/90561566/181204588-670c7afc-4349-46df-a933-bc157f051b14.png)

Open macro file in object MrPhisher/Project/Module/

![image](https://user-images.githubusercontent.com/90561566/181205191-56223c3c-e06a-4752-979b-1853b363c86a.png)

## Solve the script

It's VBscript, so we can use http://vb2py.sourceforge.net/online_conversion.html to convert it to python

![image](https://user-images.githubusercontent.com/90561566/181205796-49d63546-0b57-49df-a985-8ababd5e1f8c.png)

It will help us in translate code, but to know it, we still need to understand the code

I can translate again to

![image](https://user-images.githubusercontent.com/90561566/181206220-20712890-9602-4fbd-933b-4dd8607ce335.png)

a var is a string, b var is a list, so after xor a with i in range(a), print b, we have the flag

![image](https://user-images.githubusercontent.com/90561566/181206737-9dba4846-0ce8-4859-9ab1-a5f3ca7d9170.png)

| Flag | Flag |
| --- | --- |
| Answer | flag{a39a07a239aacd40c948d852a5c9f8d1} |
