# [OWASP Juice Shop](https://tryhackme.com/room/owaspjuiceshop)

> This room uses the Juice Shop vulnerable web application to learn how to identify and exploit common web application vulnerabilities.

## Open for business!

Within this room, we will look at OWASP's TOP 10 vulnerabilities in web applications. 

You will find these in all types of web applications. 

But for today we will be looking at OWASP's own creation, Juice Shop!

We will, however, cover the following topics which we recommend you take a look at as you progress through this room.
+ Injection
+ Broken Authentication
+ Sensitive Data Exposure
+ Broken Access Control
+ Cross-Site Scripting XSS

## Let's go on an adventure!

Before we get into the actual hacking part, it's good to have a look around. 

In Burp, set the Intercept mode to off and then browse around the site. 

This allows Burp to log different requests from the server that may be helpful later. 

This is called walking through the application, which is also a form of reconnaissance!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1f99cdb3-5546-48b8-a5bf-a1e8ee674d0a)

check on each project, there is a review and the author on apple juice

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/28702071-7e8b-410c-8cbf-2f9905a7d534)

you can use search, it use a parameter `search?q=`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/d249e91e-fce0-46c2-b1bf-08695c3eab39)

check out the Jim's review on Green Smoothie

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/01c4f894-230e-41c4-8562-60516f04aea1)

if we google "replicator" we will get the results indicating that it is from a TV show called Star Trek

## Inject the juice

This task will be focusing on injection vulnerabilities. 

Injection vulnerabilities are quite dangerous to a company as they can potentially cause downtime and/or loss of data. 

Identifying injection points within a web application is usually quite simple, as most of them will return an error. 

There are many types of injection attacks, some of them are:
+ SQL Injection
+ Command Injection
+ Email Injection

After we navigate to the login page, enter some data into the email and password fields.

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6fa38dcd-6eed-4ccf-ad32-6fae967a9c6e)

change the email to: `' or 1=1--` and forward it to the server.

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e4d1df28-54e1-4c72-84a4-dac10b500f96)

capture the login request again, but this time we will put: `bender@juice-sh.op'-- `as the email. 

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/40047a16-ee07-483b-b81b-e650d36da8d7)

## Who broke my lock?!

In this task, we will look at exploiting authentication through different flaws. 

When talking about flaws within authentication, we include mechanisms that are vulnerable to manipulation. 

These mechanisms, listed below, are what we will be exploiting. 
+ Weak passwords in high privileged accounts
+ Forgotten password pages

We have used SQL Injection to log into the Administrator account but we still don't know the password

Let's try to bruteforce the Administrator account's password using Burp Intruder

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/f94eb239-6bbf-4fbf-b663-f6a73bbdfa0e)

for the payload, we will be using the best1050.txt from Seclists.

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c4ccf428-5f0e-4032-a2c5-07ebab1f6466)

wait until see a status code of 200 (success)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3f40d681-30fe-46dc-ba47-ec0236cfdb07)

our flag now with password `admin123`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5a21e2e5-c01f-404b-9d1d-87ce49d1ac09)

now, try to reset password of `jim@juice-sh.op` with some research

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/184a071f-eb93-4e56-8bd7-bd045f553487)

googling "Jim Star Trek" gives us a wiki page for Jame T. Kirk from Star Trek.

look at the wiki, we have the answer for security question `Samuel`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/52093772-95fc-4e1f-94b2-0ae6ea361d59)

change jim password to pass the challenge

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/5f5e7667-338c-45e5-b833-ea69cea6931f)

## AH! Don't look!

A web application should store and transmit sensitive data safely and securely. 

But in some cases, the developer may not correctly protect their sensitive data, making it vulnerable.

Most of the time, data protection is not applied consistently across the web application making certain pages accessible to the public. 

Other times information is leaked to the public without the knowledge of the developer, making the web application vulnerable to an attack. 

Navigate to the About Us page in the left menu

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/51e7537b-8d99-4e6d-bbd4-fd42107a827e)

Check out our terms of use, you will see that it links to `http://10.10.207.113/ftp/legal.md`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/287ed1c7-7bf1-48d3-bfe8-9363300434bc)

hmm, why they store it in /ftp, try to access /ftp/ directory reveals that it is exposed to the public!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/872952e0-ef6b-4ce7-97ad-6212bf5ec28b)

check the acquisitions.md and save it

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/eeba81fe-4e95-4dd5-8b33-61dfa6b7ba4c)

navigate to the home page to receive the flag!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/db70820e-0f9a-48b9-a0c0-96ab82b8ef59)

Log into MC SafeSearch's account!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bcfa8dd9-59b0-4537-a448-dc6c8abffaf2)

He notes that his password is "Mr. Noodles" but he has replaced some "vowels into zeros", meaning that he just replaced the o's into 0's.

We now know the password to the `mc.safesearch@juice-sh.op account` is `Mr. N00dles`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/31daa265-8232-4e81-a003-3ecb229bf2d2)

check /ftp/package.json.bak, but it seems a 403 which says that only .md and .pdf files can be downloaded. 

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/e4755fc4-5bff-4aaf-95a4-84073ee3782c)

To get around this, we will use a character bypass called "Poison Null Byte". A Poison Null Byte looks like this: %00. 

The Poison Null Byte will now look like this: `%2500`. Adding this and then a .md to the end will bypass the 403 error!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3c815ed1-3b81-4936-99cf-0fd5837fac56)

A Poison Null Byte is actually a NULL terminator. By placing a NULL character in the string at a certain byte, the string will tell the server to terminate at that point, nulling the rest of the string. 

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ee7f5284-7e94-4a70-89fc-4a283bf3968b)

## Who's flying this thing?

Modern-day systems will allow for multiple users to have access to different pages. 

Administrators most commonly use an administration page to edit, add and remove different elements of a website.

When Broken Access Control exploits or bugs are found, it will be categorised into one of two types:
+ Horizontal Privilege Escalation
+ Vertical Privilege Escalation

using DevTools and check the main script file

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/c2e34bfa-421a-44cd-b8f8-c84451c83a42)

now search for the term "admin", you may find something interest `path: administration`

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/8293fb80-e8f1-45e9-a2be-32a53b82954f)

This hints towards a page called `/#/administration`, but going there while not logged in doesn't work. 

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a83525f1-3e5a-402c-89c0-71e8f49d948f)

As this is an Administrator page, it makes sense that we need to be in the Admin account in order to view it.

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/86865f0f-3ecd-46cc-9a7c-3ab8bf752928)

you now can see all user email here

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/59916405-c8d0-4041-949f-0634bae9c010)

Login to the Admin account and click on Your Basket, intercept the request

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/474903d2-e35a-45bb-9990-c48522b20e14)

Now, we are going to change the basket number to `GET /rest/basket/2`

it will now show you the basket of UserID 2. You can do this for other UserIDs as well, provided that they have one!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/38a25d07-cc5e-4fc4-8fcf-6b5df6a4f6ad)

return back to /administration and remove all 5-star reviews!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9f30a927-9903-4707-aedf-684b7a78e5b4)

## Where did that come from?

XSS or Cross-site scripting is a vulnerability that allows attackers to run javascript in web applications. 

These are one of the most found bugs in web applications. 

Their complexity ranges from easy to extremely hard, as each web application parses the queries in a different way. 

There are three major types of XSS attacks:
+ DOM XSS (Special)
+ Persistent XSS (Server-side)
+ Reflected XSS (Client-side)

Inputting xss into the search bar will trigger the alert.

```
<iframe src="javascript:alert(`xss`)"> 
```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/1c92c737-016d-4920-91eb-8748b6ec79c6)

now, we are going to navigate to the "Last Login IP" page to perform a persistent XSS

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/87242836-cf07-4abc-ab79-8fd33546b9a2)

logout and catch the `GET /rest/saveLoginIp` request, check Headers tab where we will add a new header

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/dec99233-769f-4efa-b238-fd2966fbee63)

we performed a persisted XSS attack with HTTP-Header XSS

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b9d64a0c-9333-4f51-bf19-945576604da7)

navigate to the "Order History" page to perform the reflected XSS!

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/a09134fc-d04d-426b-aa8f-29f72c396d18)

clicking on Track Order will bring you to the track result page

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9c8d949b-dcd9-4682-b9de-dda2e1a823b3)

We will use the iframe XSS to the id ```/track-result?id=<iframe src="javascript:alert(`xss`)">```

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/660fdbc8-c538-4fe4-9189-0695abe99bce)

The server will have a lookup table or database (depending on the type of server) for each tracking ID. 

As the 'id' parameter is not sanitised before it is sent to the server, we are able to perform an XSS attack.  

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/ca33547c-6dcc-443f-b1b9-5e88cccd6d33)

## Exploration!

If you wish to tackle some of the harder challenges that were not covered within this room, check out the /#/score-board/

Here you can see your completed tasks as well as other tasks in varying difficulty.

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/3bfd524b-8671-4f3d-9fc2-2c3e2dad5899)

1 star level

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/9d12c7de-82d6-462f-843a-d9692e596112)

2 star level

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6167ee05-a888-4222-9b27-ffe3691cb80d)

3 star level

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/2ab5e0dd-8e6d-4de1-915b-de4cbaa6775a)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/6c90a304-4617-4efb-84fd-466e21d4cee7)

4 star level

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/b813881e-17ad-407f-b63e-de5bba5ed4df)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/feb88b4a-caf3-486f-9df1-652d60d4e659)

5 star level

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/daf9364c-6ef0-4fb3-ab30-1518495dac19)

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/97cda03e-dc35-4089-8d8f-c4510b09377c)

6 star level

![image](https://github.com/lucthienphong1120/TryHackMe-CTF/assets/90561566/bf79fc9e-72ca-4c4e-bf29-c314bbb5f54e)
