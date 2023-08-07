# [Forensics](https://tryhackme.com/room/forensics)

> This is a memory dump of compromised system, do some forensics kung-fu to explore the inside.

## Volatility forensics

Volatility is a tool for Digital Forensics and Incident Response can be installed from github

```
git clone https://github.com/volatilityfoundation/volatility.git
cd volatility
sudo apt-get install pcregrep libpcre++-dev python-dev -y
sudo python2 setup.py install 
```

download the file and unzip it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/65933f54-8104-4e71-90b4-8a6820b2f3f4)

start check the image info

```
volatility -f victim.raw imageinfo
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/4bf1ddde-bc5a-4380-b9a2-8a9c53779f17)

let's check process list

```
volatility -f victim.raw --profile=Win7SP1x64 pslist
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/903bbf5f-9794-483d-9e0e-ad03eb81d929)

What is the last directory accessed by the user?

```
volatility -f victim.raw --profile=Win7SP1x64 shellbags
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/566aa633-9bf3-4869-b2df-31be930a83ed)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d148eb9c-8dc8-42db-a0e3-1f45420851ff)

## Dig a little more

scanning networks and inspect all listening ports in memory 

```
volatility -f victim.raw --profile=Win7SP1x64 netscan
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c51a1e43-b477-4f21-ade9-f9fb78a6323f)

find malicious processes in user mode memory

```
volatility -f victim.raw --profile=Win7SP1x64 malfind
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bc8da52d-ccf0-480a-9230-b694c4012506)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/76a6da0e-7ba3-42cf-b442-2e49457d8abd)

## IOC SAGA

inspect memory of svhost.exe first

```
mkdir -p 1820 && volatility -f victim.raw --profile Win7SP1x64 memdump -p 1820 -D 1820
```

Lets find out URLs

```
strings 1820.dmp | grep 'www\.go....\.ru'
strings 1820.dmp | grep 'www\.i....\.com'
strings 1820.dmp | grep 'www\.ic......\.com'
strings 1820.dmp | grep '202\....\.233\....'
strings 1820.dmp | grep '...\.200\...\.164'
strings 1820.dmp | grep '209\.190\....\....'
```

check the environmental variable of PID 2464

```
volatility -f victim.raw --profile Win7SP1x64 envars -p 2464
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/367df1cf-787c-4127-8e45-44f25db90c8c)

Answer:

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2af9bfe3-9cc0-4b4e-8c55-1e551207cc7d)
