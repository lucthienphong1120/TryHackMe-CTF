# [Advent of Cyber 5 [2023]](https://tryhackme.com/room/adventofcyber2023)

> Get started with Cyber Security in 24 Days - Learn the basics by doing a new, beginner friendly security challenge every day leading up to Christmas.

## Day 9: She sells C# shells by the C2shore

open dnSpy with JuicyTomaToy file at Desktop

explore functions in Program, you can see HTTP User-Agent request on GetIt() function

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/057b3f2a-1eff-440b-9a69-1ef42dc2c6e7)

look at Main function, you can see it ExecuteCommand before PostIt()

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1c526c18-eb73-4fe3-8d1f-19d6ba098b69)

you can found the key used to encrypt C2 data

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/688c3451-1b70-47f4-a69d-7ebb7e78b44b)

and the URL used by the malware reveal at Main() function

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/48fffc09-d564-47ed-9262-3e0869d2daad)

on the Main function, you can see it use Sleeper() 15 seconds

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e2fad2ae-59b1-49f9-a86b-30794aa2ab63)

deep into the Main function, you can see it ExecuteCommand() at Array[1] which is `shell`

after that it download another binary

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8b6e7b7c-22b2-4599-a672-7b287356fe17)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/12339856-a9c5-4f50-94c7-39dad3901151)

## Day 10: Inject the Halls with EXEC Queries
























