# [Advent of Cyber 2021](https://tryhackme.com/room/adventofcyber3)

> Get started with Cyber Security in 25 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 18: Playing With Containers

pull the latest image from GrinchEnterprises Elastic Container Registry (ECR)

```
docker pull public.ecr.aws/h0w1j9u3/grinch-aoc:latest
```

start a shell to interact with the container

```
docker run -it public.ecr.aws/h0w1j9u3/grinch-aoc:latest
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/16a5c4a3-6a50-4bb6-8057-8f07772a302f)

we can do some command here, but our challenge requires that we exit the shell

```
mkdir aoc
cd aoc
docker save -o aoc.tar public.ecr.aws/h0w1j9u3/grinch-aoc:latest
tar -xvf aoc.tar
```

let's read manifest.json using jq

```
cat manifest.json | jq
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4c065059-8fc2-4e5a-9a51-1dc238e0dce9)

we are guided to find the answer to the last question

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2e43a97d-ae64-4b23-9538-613b45c49868)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/fa6814c3-f1cb-4f17-a701-c13411d24c41)

```
cat root/envconsul/config.hcl | grep "token"
```

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b5518ee2-98f7-4706-9ef2-9e456aedafc6)

## Day 19: Something Phishy Is Going On

firstly, we have a machine like that

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4d39aa52-05e7-4134-b5c9-14964d79c91f)

open the email.eml on the top panel

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5876396b-2972-40ad-8b84-6919120eb910)

it's like a phishing email, malicious reply and 1 misspelled word

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/163fe0d3-6bdf-4868-bb20-8c8ba98dc457)

on the button also contain a malicious link

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/573af55f-bc26-4efd-92d0-cba1f66faa5e)

click on more/view source, you can see a malicious header

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8c665635-7af2-4159-b88e-4897ffe5efaf)

check the Email Artifacts of other colleagues and read attachment.txt

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3c326511-6c26-46ca-9971-e7e1515b2012)

we need to decode the attachment to a file, you can load the file into CyberChef, decode and save it

in my case, i can use command to decode itself and open it

```
cat attachment-base64-only.txt | base64 -d > file.pdf
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4ef4de67-38a3-46c0-9e23-c823528114fa)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f8ecc006-a1e4-45ec-a7ac-d1a33de13b62)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3646bb97-79ea-41c2-b2ad-91d80c244c58)

## Day 20: What's the Worst That Could Happen?





















































