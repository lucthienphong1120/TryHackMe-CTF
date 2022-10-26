# Pickle Rick

> A Rick and Morty CTF. Help turn Rick back into a human!

## Scanning

Scan the target with nmap

```
nmap -A -T4 10.10.197.149
```

![image](https://user-images.githubusercontent.com/90561566/198048801-c6fab2ef-a96b-4a83-b5d4-3c07ff43ae30.png)

I found 2 port open are SSH (22) and HTTP (80)

## HTTP

Go to the webpage

![image](https://user-images.githubusercontent.com/90561566/198049923-948dc644-54cf-469b-9ea6-b6baf279fb4c.png)

It's an username `R1ckRul3s`

![image](https://user-images.githubusercontent.com/90561566/198051564-2b2260db-9d45-47cb-b434-f5827de6e595.png)

## Enumeration

scan directories with gobuster

```
gobuster dir -u http://10.10.197.149/ -w /usr/share/wordlists/dirb/common.txt -t 30 -x php
```

![image](https://user-images.githubusercontent.com/90561566/198052681-c707007f-f40b-4733-82e5-673bdd46ecef.png)

I see a robots.txt seen meaningless `Wubbalubbadubdub`

![image](https://user-images.githubusercontent.com/90561566/198052867-3a41c8d9-179f-48b0-833c-cd1fe52044c5.png)

Let see login.php page

![image](https://user-images.githubusercontent.com/90561566/198053586-cd22ad1b-80c0-4dcb-aec9-94ae7de50310.png)

Try login with R1ckRul3s/Wubbalubbadubdub, wow login successful

## Exploitation

I see here is only a command injection page and other navbars are denied

![image](https://user-images.githubusercontent.com/90561566/198055489-af19a7e4-69ba-414d-b7de-aab61c409193.png)

```
ls -l
```

![image](https://user-images.githubusercontent.com/90561566/198057111-103948f9-2441-4190-9e93-16461221130f.png)

`cat` command is disabled

![image](https://user-images.githubusercontent.com/90561566/198056671-c38d9619-92b9-4277-885f-5a46b9b3db6c.png)

but it can use `less`

![image](https://user-images.githubusercontent.com/90561566/198057362-2bea79cd-9113-403c-844f-ff255d02931f.png)

| Flag | first ingredient |
| --- | --- |
| Answer | mr. meeseek hair |

moreover, i can use this

```
less portal.php
Ctrl+U
```

there are list of disabled commands

![image](https://user-images.githubusercontent.com/90561566/198058048-ff95c816-fc36-4d3d-adbd-213469894d24.png)

a suggestion

![image](https://user-images.githubusercontent.com/90561566/198058623-df2788b9-430a-4e0e-a6f9-c9b78e621ca8.png)

i look for rick home folder

![image](https://user-images.githubusercontent.com/90561566/198059271-4c7e824b-1d73-43b7-91b4-00d5aeb9eca9.png)

hmm 2nd ingredient, but it's root privilege

![image](https://user-images.githubusercontent.com/90561566/198059455-3899bd11-533f-4e50-a002-c40c4b6d9479.png)

## Privilege Escalation

```
sudo -l -l
```

wow, a lot of faults on this machine haha, it can run sudoer with everything

![image](https://user-images.githubusercontent.com/90561566/198061010-16cabb2d-2259-497a-b692-63e4d4820cb6.png)

```
sudo less "/home/rick/second ingredients"
```

okay here is second ingredient

![image](https://user-images.githubusercontent.com/90561566/198061499-dc9c622f-6562-46e0-8157-a56e88da984c.png)

| Flag | second ingredient |
| --- | --- |
| Answer | 1 jerry tear |

the last, i guest it will be somewhere in /root

```
sudo ls -l /root
```

remember sudo keyword

![image](https://user-images.githubusercontent.com/90561566/198062243-2ec642fc-5a95-4465-ab3e-63f2bccb2fe3.png)

```
sudo less /root/3rd.txt
```

![image](https://user-images.githubusercontent.com/90561566/198062459-709cbec5-a14a-4460-bba7-b89a67cb9d82.png)

| Flag | third ingredient |
| --- | --- |
| Answer | fleeb juice |
