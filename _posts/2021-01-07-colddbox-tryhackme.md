---
layout: post
title:  "ColddBox - TryHackMe Walkthrough"
date:   2021-01-07 10:12:30 +0530
categories: WriteupBlog
---
ColddBox is a Beginner Friendly machine. It’s a free machine on TryHackMe(no need of subscription).

Link: <a href="https://tryhackme.com/room/colddboxeasy">https://tryhackme.com/room/colddboxeasy</a>

{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/thmbanner.png" | relative_url}})
{: refdef}

## Overview:

Nmap Scanning, WordPress Enumeration, Brute-forcing and multiple ways to escalate privileges.
## Enumeration:

First step as always carry out Nmap scan
```shell
nmap -sT -A -v <target-IP>
```
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/nmapscan.png" | relative_url}})
{: refdef}

<br>
In nmap scan you will see that port 80 is open and its running Apache httpd server 2.4.18. Also you can see there’s WordPress site running. Now have a look at the website in the browser.

<br>
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/wordpresssite.png" | relative_url}})
{: refdef}

<br>
After having a look at whole website you can see at the bottom left section there is a ‘login’ for the website. Its a WordPress login. Now do a WordPress scan with wpscan and enumerate for users.

```shell
wpscan --url http://<target-IP>/ -e u
``` 
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/wpscanusers.png" | relative_url}})
{: refdef}

<br>
You will see that there are 4 users. Now try to get the password for the user by brute-forcing with wpscan. I personally used SecLists/Passwords/Common-Credentials/10-million-password-list-top-10000.txt for cracking the password.

The command syntax is:
```shell
wpscan --url http://<target-IP>/ --passwords wordlist.txt
```
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/wpscanpass.png" | relative_url}})
{: refdef}

<br>
Great!! You have cracked the password for user c0ldd. Login to the wordpress dashboard.
## Gaining Foothold:

Now navigate to Appearance->Editor->Select 404 Template. You can see its a php code in the editor. You have to replace that code with php-reverse-shell code from <a href="https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php">pentestmonkey</a>. Change the IP to your TryHackMe Ip address. See the image below for reference.
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/wordpressdashboard.png" | relative_url}})
{: refdef}

Update the file and leave the dashboard and go the the blog site.

Now to get the 404 error code in order the execute the php shell you need to access the blog which is not present on the site. Before that start a listener on your attacking machine.

```shell
nc -nvlp 1234
```
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/nclistener.png" | relative_url}})
{: refdef}

To access the 404 error page, do the following:

Click on the first blog with title “The Colddbox is here”

Have a look at the URL now. You can see that there is a parameter p= which is point to the current blog. Now all you need to do is change the parameter to any random number and press enter. Make sure that you have a listener running before doing this.
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/urlparam.png" | relative_url}})
{: refdef}

You will get a shell which is unstable for now. Execute following commands to make the shell stable:
```shell
python3 -c ‘import pty;pty.spawn(“/bin/bash”)’
export TERM=xterm
```
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/shell.png" | relative_url}})
{: refdef}
## More Enumeration:

Now enumerate more to find a way to escalate the privileges.

You know that there’s a WordPress site running. Go the /var/www/html and have look at the files. Check through those which you feel will contain some important information. There is a file which has credentials in plain text.

The file ‘wp-config.php’ is what you need. Read the file and you will get the credentials for MySQL database.
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/wpconfig.png" | relative_url}})
{: refdef}

You cannot access mysql database as the current user. But there is always a high possibility that MySQL password and login password for the user is the same. Try to login to the user c0ldd.

{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/c0lddshell.png" | relative_url}})
{: refdef}

Cool!! You got it.

Now check for the user flag in user’s directory.
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/userflag.png" | relative_url}})
{: refdef}

## Privilege Escalation:

Now it’s time to escalate privileges. Try to check for the sudo permissions for the user c0ldd.
{:refdef: style="text-align: center; width: 500px; height: 200px;"}
![Image]({{ "/assets/images/thm/colddbox/sudoperms.png" | relative_url}})
{: refdef}

<br>
Awesome!! We can escalate privileges with multiple commands.
1. **/usr/bin/vim**\
Execute sudo vim in terminal, enter the password for the user c0ldd.
You will get vim interface. There you have to type “:!/bin/bash” and press enter
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/vim.png" | relative_url}})
{: refdef}

<br>
2. **/usr/bin/chmod**\
With “sudo chmod” you could change the permissions of any directory or file and access it. That’s a big hint. I guess you can get through this…

<br>
3. **/usr/bin/ftp**\
All you need to do is this…
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/ftp.png" | relative_url}})
{: refdef}

Now get the root flag…
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/colddbox/rootflag.png" | relative_url}})
{: refdef}

That’s it. Although there may be more ways to escalate privileges. You can try to find those if you are curious.
## Thankyou!!