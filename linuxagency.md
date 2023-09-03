# [Linux Agency](https://tryhackme.com/room/linuxagency)

> This Room will help you to sharpen your Linux Skills and help you to learn basic privilege escalation in a HITMAN theme. So, pack your briefcase and grab your SilverBallers as its gonna be a tough ride.

##  Linux Fundamentals

ssh into the machine

```
ssh agent47@10.10.74.216
640509040147
```

after login, i get the mission1 flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ca18c877-1bc7-4d95-bd28-9aca34f7812a)

or you can do with `cat .ssh/rc`

```
su mission1
mission1{174dc8f191bcbb161fe25f8a5b58d1f0}
```

very easy

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/32678ea1-072a-4aa7-a1ee-34e857fb71eb)

```
su mission2
mission2{8a1b68bb11e4a35245061656b5b9fa0d}
```

do the same

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/05e43459-be21-479e-a0d2-0fdbae497b7d)

```
su mission3
mission3{ab1e1ae5cba688340825103f70b0f976}
```

this time is a message for us

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b5e962c1-d278-4035-95ec-881d6ac28c03)

after some stuck, i see flag is hidden in the file with non-printable characters

```
cat -v flag.txt
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/919bd8d3-4ee4-4209-af50-32534537169c)

```
su mission4
mission4{264a7eeb920f80b3ee9665fafb7ff92d}
```

easy too

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9413d777-c9a4-4758-87d9-db85ee61f5ce)

```
su mission5
mission5{bc67906710c3a376bcc7bd25978f62c0}
```

hidden file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5e648ed1-ccc7-4796-b89f-f764efb685dd)

```
su mission6
mission6{1fa67e1adc244b5c6ea711f0c9675fde}
```

the same

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/72bb103f-9852-43e4-9044-59b680c06dca)

```
su mission7
mission7{53fd6b2bad6e85519c7403267225def5}
```

we don't have permisson on our home

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4ca4fc94-f654-4717-ab27-b399b9daefc2)

cd out and cd again to get flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/cc50e55e-19cd-43cf-84e5-9f84d119b8d8)

```
su mission8
mission8{3bee25ebda7fe7dc0a9d2f481d10577b}
```

the flag on / is own by mission8

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/df8cb933-38a2-474b-88f7-dec37fcdcf9a)

cat /flag.txt

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d8192ee5-2f09-4e55-a72e-3fcc32d87148)

```
su mission9
mission9{ba1069363d182e1c114bef7521c898f5}
```

grep flag from the file in our home

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6d887078-719b-4137-ace5-4f1d283cf597)

```
su mission10
mission10{0c9d1c7c5683a1a29b05bb67856524b6}
```

find reverse

```
grep -r mission11 . 2>/dev/null
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8d669620-3493-4727-abf7-2469f760a5f1)

```
su mission11
mission11{db074d9b68f06246944b991d433180c0}
```

you can find the flag in `cat .bashrc`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/45efbb57-b1df-4da2-9120-97a6617e9ddd)

or export the `env` variables

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/41feaf5f-b2b2-4a2e-baae-5d380456d17f)

```
su mission12
mission12{f449a1d33d6edc327354635967f9a720}
```

we need add permission before read

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8fcc6a32-0da8-4fcf-9998-10661cb75f87)

```
su mission13
mission13{076124e360406b4c98ecefddd13ddb1f}
```

decode base64

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6334a14d-fb02-4442-b130-8dcbb00851b1)

```
su mission14
mission14{d598de95639514b9941507617b9e54d2}
```

binary decode with cyberchef

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a27e8906-29ee-4b59-b681-cb3c33baea8a)

```
su mission15
mission15{fc4915d818bfaeff01185c3547f25596}
```

cyberchef decode from hex

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5c7a077a-1b1d-4257-b6bc-4a9a0887683a)

```
su mission16
mission16{884417d40033c4c2091b44d7c26a908e}
```

it's an executable file, run it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2213c125-3267-4f10-9857-593949cb6177)

```
su mission17
mission17{49f8d1348a1053e221dfe7ff99f5cbf4}
```

compile the jave file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2568d454-dc82-4756-9c15-2a263818bce4)

```
su mission18
mission18{f09760649986b489cda320ab5f7917e8}
```

it's a ruby file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/905612b9-0160-426b-8d3f-d5d8c8c01274)

```
su mission19
mission19{a0bf41f56b3ac622d808f7a4385254b7}
```

compile c file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a367fcd8-be98-45aa-b1bd-34886eb8a166)

```
su mission20
mission20{b0482f9e90c8ad2421bf4353cd8eae1c}
```

and python run

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e7cf0b15-aff3-4f25-9e94-db560e075bfb)

```
su mission21
mission21{7de756aabc528b446f6eb38419318f0c}
```

our shell was /bin/sh, spawn a bash to get flag

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/999a449e-dc38-4ff4-a2ed-cb8c24710f58)

```
su mission22
mission22{24caa74eb0889ed6a2e6984b42d49aaf}
```

now, spawn bash from our python shell with `import pty;pty.spawn("/bin/bash")`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e8efa39d-6c0b-4fac-8569-d70fde4942ce)

```
su mission23
mission23{3710b9cb185282e3f61d2fd8b1b4ffea}
```

we have a message

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3bd210e3-2160-4968-a112-2c17cceb158d)

check host file and get flag from website

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f1ef108f-b979-459d-a94f-207238a9fb64)

```
su mission24
mission24{dbaeb06591a7fd6230407df3a947b89c}
```

we have an executable file here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/676e9cd9-975f-4f6e-81a7-c80f2a18ee96)

let's inspect it with `ltrace ./bribe`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5727da21-7445-480d-bc63-8d0234a52d81)

you see it take 2 env variables, test some

```
export pocket=100
ltrace ./bribe
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5578d6ab-9955-4856-9169-2b41c023b930)

it use strcmp, `pocket` variables must equal "money"

```
export pocket=money
./bribe
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f779b215-5c61-4f35-b892-a36f0a9abec9)

```
su mission25
mission25{61b93637881c87c71f220033b22a921b}
```






## Privilege Escalation


| Flag | root.txt |
| --- | --- |
| Answer | <flag> |
