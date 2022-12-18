# [Corridor](https://tryhackme.com/room/corridor)

> Can you escape the Corridor?

## Scanning

scan the target

```
nmap -sS -sV 10.10.225.145
```

![image](https://user-images.githubusercontent.com/90561566/208304304-ebb780f4-5fd9-4ad6-b3d7-8b06d5bf51c5.png)

## HTTP

go to the webpage, we can see a corridor with a lot of door can open

![image](https://user-images.githubusercontent.com/90561566/208304440-ffdb757e-26fe-486a-ae69-b9309b6b08de.png)

when view source, i can see a lot of hash value on each door

![image](https://user-images.githubusercontent.com/90561566/208304578-22aff0f2-7cd0-45ae-b961-e26e0db2b4db.png)

## Enumeration

now, i will colect all these hashes to further research

```
curl http://10.10.225.145 | grep 'alt' | cut -d '"' -f4 > hash.txt
```

![image](https://user-images.githubusercontent.com/90561566/208305766-3116f552-71e3-4687-8194-e0c5daf30d35.png)

## Cracking

crack the hash with john the ripper

```
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

![image](https://user-images.githubusercontent.com/90561566/208305948-476b7b4d-d722-4aaa-92f3-b56b09d10610.png)

Clearly, all hashed URL endpoints are numbers from 1 to 13.

## Exploitation

let's think about IDOR vulnerability, i will change the hashed URL to over the zone, maybe 14 or 0

```
echo -n 14 | md5sum
echo -n 0 | md5sum
```

![image](https://user-images.githubusercontent.com/90561566/208306173-7b83d179-445a-46ed-a4d6-dac6fb7f7026.png)

nothing at room 14 but there is a flag at room 0

![image](https://user-images.githubusercontent.com/90561566/208306218-0f9d7e81-34a7-4b37-8c83-73ca1c21f34c.png)

Here you are

| Flag | Answer |
| --- | --- |
| flag | flag{2477ef02448ad9156661ac40a6b8862e} |
