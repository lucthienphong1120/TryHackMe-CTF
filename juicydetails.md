# [Juicy Details](https://tryhackme.com/room/juicydetails)

> A popular juice shop has been breached! Analyze the logs to see what had happened...

## Introduction

You were hired as a SOC Analyst for one of the biggest Juice Shops in the world and an attacker has made their way into your network. 

Your tasks are:
+ Figure out what techniques and tools the attacker used
+ What endpoints were vulnerable
+ What sensitive data was accessed and stolen from the environment

An IT team has sent you a zip file containing logs from the server. 

## Reconnaissance

unzip the file, we see 3 log files

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/57aa5f9a-a0a5-41d5-ae60-72065a8297f8)

check access.log, you can see `User-Agent` header contain some popular tools, grab all the user agents with cut

```
cat access.log | cut -d'"' -f6 | sort -u
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/acc457ab-05f0-4e21-a210-e933a64ec6d9)

we can see that contain a famous Credential brute-forcing tools `hydra`, grep to see the vulnerable endpoint

```
cat access.log | grep -i hydra | sort -u
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/91130cb1-abfe-4647-ba92-6304591beff4)

check again with SQL injection tools by `sqlmap`, grep to see the vulnerable endpoint

```
cat access.log | grep -i sqlmap | sort -u
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/16fbd9d1-6d29-4c25-9bd0-9b6fc1b6865a)

attacker try to use to retrieve files using an endpoint `ftp`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/641b2c34-8cb2-463d-9c04-f3f8468f700a)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e3740caf-8448-493d-a018-88ffbae6225a)

## Stolen data

you can see a lot of endpoint related to product reviews page may contain user email addresses

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c54277ba-3497-4d1e-b389-4e97399a6c25)

check the timestamp of the successful login by hydra

```
cat access.log | grep -i hydra | sort -u | grep 200
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d29d8e4b-6755-4eca-9e32-16c696d00d6a)

check the users information of SQL injection from vulnerable search form

```
cat access.log | grep "?q=" | sort -u | grep -i users
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/27b2fd96-c9f2-4e00-b3b7-907214e55392)

check the downloaded file of vulnerable endpoint

```
cat access.log | grep /ftp | sort -u
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6d2200f9-a483-4d5d-a43b-506b1b853592)

service and account name were used to retrieve files that related to ftp log file

```
grep -i login vsftpd.log
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5bad56f8-c013-4310-b1b0-9b761c28b369)

service and account name were used to gain shell to server that related to auth log

```
grep -i accepted auth.log
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3159733e-38d6-459f-8950-242288db1b76)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/634bcc91-e97f-44d6-bc9d-76f20e8b498d)
