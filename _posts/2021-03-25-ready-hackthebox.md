---
title:  "Ready - HackTheBox"
style: fill
color: dark
tags: [HackTheBox, CVE, Docker]
permalink: /blog/htb/ready
description: A fairly easy box which takes us through gaining foothold using public exploit and but requires good enumeration skills to PrivEsc.
---
<br>
![Image]({{ "/assets/images/htb/ready/banner.png" | relative_url}})
<br>

## Overview: 
An easy box which takes us through gaining foothold using public exploit and but requires good enumeration skills to PrivEsc. [Access the machine here](https://app.hackthebox.eu/machines/304)

## Enumeration:

Firing up the [nmapAutomator](https://github.com/21y4d/nmapAutomator).

```
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
|   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
|_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
5080/tcp open  http    nginx
| http-robots.txt: 53 disallowed entries (15 shown)
| / /autocomplete/users /search /api /admin /profile 
| /dashboard /projects/new /groups/new /groups/\*/edit /users /help 
|_/s/ /snippets/new /snippets/*/edit
|_http-title: GitLab is not responding (502)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Two ports 22 & 5080 are open. Port 5080 is running a web server.
Accessing the ip:port in the browser redirects us to sign in page of Gitlab. We don't have any credentials but we can register ourselves. 

<br>
![Image]({{ "/assets/images/htb/ready/register.png" | relative_url}})
<br>

After registering, we are redirected to dashboard. In the help menu, we get the version of Gitlab running on the web server.

<br>
![Image]({{ "/assets/images/htb/ready/version.png" | relative_url}})
<br>

Using searchsploit on "gitlab", we can see the **Gitlab 11.4.7** is vulnerable to **Remote Code Execution**.

<br>
![Image]({{ "/assets/images/htb/ready/searchsploit.png" | relative_url}})
<br>

## Gaining Foothold:

There are two exploits available for the found version. We will use the latest one i.e 49334.py. Copy the exploit to your current directory. We need to provide **username**, **password**, **url**, **lhost** & **lport** as arguments to the script. Start the netcat listener and fireup the script. You will get a shell back.

<br>
![Image]({{ "/assets/images/htb/ready/revshell.png" | relative_url}})
<br>

## Post-Exploitation Enumeration:
You will find a user directory with name "**dude**". You can read the user flag as you are part of group **git**.

<br>
![Image]({{ "/assets/images/htb/ready/userflag.png" | relative_url}})
<br>

Tried enumerating the machine manually but there are lot of directories and I didn't want to miss out on anything. So decided to run Linpeas.

Tranfer [linpeas.sh](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) script to the target machine and run it.<br>
First thing we come to know that we are inside a **docker container**.

<br>
![Image]({{ "/assets/images/htb/ready/docker.png" | relative_url}})
<br>

Going through the output of the script, you will find few hashes of users of gitlab and also one plain-text password. Also there is a file named **root_pass** which contains a string which maybe a password for root.

<br>
![Image]({{ "/assets/images/htb/ready/password.png" | relative_url}})
<br>

## Privilege Escalation:

Ran hashcat over the hashes found but didn't get the password. Tried using the found password on user **dude** but no success. But the password works for root user. Switch the user to root using the found password. The one found in **root_pass** file didn't work for me.

<br>
![Image]({{ "/assets/images/htb/ready/suroot.png" | relative_url}})
<br>

Going over to root directory, you won't find the root flag. This means there is more work to do. Remember you are inside a docker container. Most probably the root flag is on the host filesystem. You have to escape the docker container or try to find a way to access the host filesystem. After a bit of googling, found a great article from Hacktricks which takes us through different privilege escalation technique using docker. <br>

Take a look at article from [Hacktricks](https://book.hacktricks.xyz/linux-unix/privilege-escalation/docker-breakout)
. The technique I leveraged is mounting the host file system which worked as the way it is meant to be.

First find the correct filesystem to be mounted.

<br>
![Image]({{ "/assets/images/htb/ready/filesystem.png" | relative_url}})
<br>

The filesystem on the device **/dev/sda2** is the one you need. Mount it and get the flag.

<br>
![Image]({{ "/assets/images/htb/ready/rootflag.png" | relative_url}})
<br>

That's it for this machine.


## Thank-you!!

Do follow me on socials below for stay connected for these kind of writeups/walkthroughs/blogs.
