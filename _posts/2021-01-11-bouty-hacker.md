---
title:  "Bounty Hacker - TryHackMe"
style: fill
color: dark
tags: [TryHackMe, FTP, Hydra]
permalink: /blog/thm/bountyhacker
description: Short and Easy CTF-ish machine from TryHackMe created by @Sevuhl.
---
<br>
![Image]({{ "/assets/images/thm/bountyhacker/banner.png" | relative_url}})
<br>
Short and Easy CTF-ish machine from TryHackMe created by [@Sevuhl](https://tryhackme.com/p/Sevuhl).

Link - https://tryhackme.com/room/cowboyhacker

## Overview: 
Nmap Scanning, FTP, Hydra Brute-force and Privilege Escalation.

## Enumeration:
```shell
nmap -sT -A -v 10.10.10.10
```
![Image]({{ "/assets/images/thm/bountyhacker/aggressivenmap.png" | relative_url}})
<br>

I found 3 open ports: 21, 22 and 80 running FTP, SSH and HTTP respectively.

FTP allowed Anonymous Login.

I straight away went to port 80 to see whats happening there. Got an interesting index page with some dialogue exchange between some characters. Nothing interesting found in source code and also in gobuster directory search.

<br>
![Image]({{ "/assets/images/thm/bountyhacker/web.png" | relative_url}})
<br>

Note down all the names on the webpage in a text file.

Then I went to FTP Anonymous login. Use the command
```shell
ftp 10.10.10.10 
```
Enter username as **Anonymous** and you will be in

<br>
![Image]({{ "/assets/images/thm/bountyhacker/ftpfiles.png" | relative_url}})
<br>

Found two files. Immediately downloaded them to my machine with **get** command. Read the content inside the file.

<br>
![Image]({{ "/assets/images/thm/bountyhacker/nameintask.png" | relative_url}})
<br>

In task.txt you will get the answer of one of the question.

The second file had passwords in it. So, it was clear that we needed to brute-force. SSH port was open. But I was not sure of what the possible username could be. So I listed all the names(lowercase) I found on the webpage and the task.txt file in a text file(users.txt)

## Gaining Foothold:

Brute-forced SSH with Hydra
```shell
hydra -V -t 4 -L users.txt -P locks.txt <target-ip> ssh
```
![Image]({{ "/assets/images/thm/bountyhacker/sshpass.png" | relative_url}})
<br>

Got the password for user **lin**. If you want to save some time use the username **lin** instead of trying all the names.

Login with **SSH** using found credentials and get the user flag on Desktop.

<br>
![Image]({{ "/assets/images/thm/bountyhacker/userflag.png" | relative_url}})
<br>

## Post Exploitation Enumeration:

Whenever I get a low-privileged shell, first thing I do is look through the sudo permissions that the user has. Use **sudo -l** command.

<br>
![Image]({{ "/assets/images/thm/bountyhacker/sudol.png" | relative_url}})
<br>

Luckily, the user had sudo permission to run command /bin/tar.

Best resource to know how to exploit sudo permission for a program is [GTFOBins](https://gtfobins.github.io/). Search for the command and look how to escalate privileges with specific program using sudo.

## Privilege Escalation:
```shell
sudo tar -cf /dev/null /dev/null — checkpoint=1 — checkpoint-action=exec=/bin/sh
```
Run the command, get the root shell and read the root flag.

<br>
![Image]({{ "/assets/images/thm/bountyhacker/privescrootflag.png" | relative_url}})
<br>

It was a straight-forward privilege escalation as it was a Easy difficulty machine.

## Thank-you!!

Do follow me on socials below for stay connected for these kind of writeups/walkthroughs/blogs.