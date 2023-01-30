# [TakeOver](https://tryhackme.com/room/takeover)

> This challenge revolves around subdomain enumeration.

## Prepare

make sure to add the subdomain to `/etc/hosts` before opening

```
echo 10.10.149.64 futurevera.thm >> /etc/hosts
```

now, we are able to view that page

![image](https://user-images.githubusercontent.com/90561566/215426100-4dc91522-251b-46c6-9233-dbd7cf68c2aa.png)

## Scanning

try to scan dns with nmap

```
nmap -sS --script dns-* 10.10.149.64
```

![image](https://user-images.githubusercontent.com/90561566/215426907-9f331c39-c0bc-4bb4-b3c5-7b0fd0c08e84.png)

nothing found from nmap, source code is the same

## Enumeration

let's enum with ffuz for http

```
ffuf -w /usr/share/wordlists/dirb/common.txt -u "http://10.10.149.64" -H "Host: FUZZ.futurevera.thm" -fw 1
```

![image](https://user-images.githubusercontent.com/90561566/215431870-18b25759-7b19-415a-b216-a02fc3862620.png)

remember to add it to /etc/hosts

```
echo 10.10.149.64 portal.futurevera.thm >> /etc/hosts
```

go to portal.futurevera.thm

![image](https://user-images.githubusercontent.com/90561566/215433802-7ac493ba-15d5-4343-9935-ad840a27ae06.png)

enum again with ffuz for https

```
ffuf -w /usr/share/wordlists/dirb/common.txt -u "https://10.10.149.64" -H "Host: FUZZ.futurevera.thm" -fw 1511
```

![image](https://user-images.githubusercontent.com/90561566/215433863-005b5ab0-fb26-4963-9093-da528a822380.png)

```
echo 10.10.149.64 blog.futurevera.thm >> /etc/hosts
echo 10.10.149.64 support.futurevera.thm >> /etc/hosts
```

we have a blog

![image](https://user-images.githubusercontent.com/90561566/215434239-0d5f00f3-417c-4926-810b-4627027c60ae.png)

and a support page

![image](https://user-images.githubusercontent.com/90561566/215434449-5a21cbdb-edb1-46df-a2da-f44d692a6285.png)

check the website's certificate

![image](https://user-images.githubusercontent.com/90561566/215434684-cbd8fc7e-2354-4ed6-a998-adb0aab51edf.png)

you with found a secret dns here

![image](https://user-images.githubusercontent.com/90561566/215434823-24f53564-b38e-4ce1-ad4e-c7e6287a460f.png)

```
echo 10.10.149.64 secrethelpdesk934752.support.futurevera.thm >> /etc/hosts
```

go to `http://secrethelpdesk934752.support.futurevera.thm`

you will see it redirect to a website contain our flag

![image](https://user-images.githubusercontent.com/90561566/215436061-f615b3a2-8f1b-4d7e-a837-b2302d77b63a.png)

| Flag | flag |
| --- | --- |
| Answer | flag{beea0d6edfcee06a59b83fb50ae81b2f} |
