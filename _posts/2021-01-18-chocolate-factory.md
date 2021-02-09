---
title:  "Chocolate Factory - TryHackMe"
style: fill
color: warning
tags: [TryHackMe, FTP, Steganography, Sudo]
permalink: /blog/thm/chococalte-factory
description: Another fun CTF room from TryHackMe. Really enjoyed doing it as usual. It is a beginner level machine. The room is created by @0x9747 and @saharshtapi and @AndyInfoSec.
---

{:refdef: style="text-align: center;"}
![Image]({{ "/assets/images/thm/chocolatefactory/poster.png" | relative_url}})
{: refdef}
<br>

Another fun CTF room from TryHackMe. Really enjoyed doing it as usual. It is a beginner level machine.

The room is created by [0x9747](https://tryhackme.com/p/0x9747) and [saharshtapi](https://tryhackme.com/p/saharshtapi) and [AndyInfoSec](https://tryhackme.com/p/AndyInfoSec)

Link: [https://tryhackme.com/room/chocolatefactory](https://tryhackme.com/room/chocolatefactory)

## Enumeration:

First as usual, carry out nmap scan on all the ports. Use the following command.
```shell
nmap -sV -v -p- 10.10.10.10
```
<br>
![Image]({{ "/assets/images/thm/chocolatefactory/nmapinitial.png" | relative_url}})
<br>

Nmap Initial Scan

We have 3 standard ports open.
* 21 - FTP
* 22 - SSH
* 80 - HTTP

Port 80 has a login form on it.

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/http80.png" | relative_url}})
<br>

We have several other unusual open ports. They also gave some output while service scanning.

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/banners.png" | relative_url}})
<br>

It contained some text. So, to have better look at what each port returned, we can use netcat for banner grabbing. Use following command
```shell
nc -nv 10.10.10.10 <port>
```
<br>
![Image]({{ "/assets/images/thm/chocolatefactory/Hint.png" | relative_url}})
<br>

It gives us a hint. Tells us to look somewhere else. So I looked at all the other remaining port and all the ports gave the same response except for port 113.

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/keyfile.png" | relative_url}})
<br>

It tells us the about a key. Open the URL in the browser and download the key file. Its a binary file which is not mostly readable. In order to read the human readable stuff from file, use strings command.
```shell
strings key_rev_key
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/key.png" | relative_url}})
<br>

We get a key in the output. But we don’t know yet where we can use it. Let’s enumerate further. We know that FTP port is open. After running an aggressive scan on port 21, we can see that it allows Anonymous login and the FTP server contains a .jpg file.
```shell
nmap -p21 -A -v 10.10.10.10
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/ftpanon.png" | relative_url}})
<br>

Login to the FTP as **Anonymous**. Press enter when asks for password and download the image file.
```shell
ftp 10.10.10.10
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/ftpget.png" | relative_url}})
<br>

Now that we have got an image file to deal with, we can safely assume that this has something to do with steganography. We can use steghide command to see if anything is embedded into the image file and extract it.
```shell
steghide --info gum_room.jpg
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/steghideInfo.png" | relative_url}})
<br>

We can see that there is a text file embedded in it. Now let’s extract it.
```shell
steghide extract -sf gum_room.jpg
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/steghideExtract.png" | relative_url}})
<br>

b64.txt file contains a base64 encoded content. Read it using command
```shell
cat b64.txt | base64 -d
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/charlie.png" | relative_url}})
<br>

The content is similar to any shadow file. It contains password for user charlie. There must be a user on machine named ‘charlie’. Copy the hash(text between first and second colon). We will crack it using hashcat.
```shell
hashcat -m 1800 hash.txt rockyou.txt
```

Use full path for the password file depending on your system. If you are using Kali or Parrot, the path is /usr/share/wordlists/rockyou.txt .

You can also using john the ripper to crack the password.
```shell
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

It will take some time to crack the password. So be patient.

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/charliepass.png" | relative_url}})
<br>

Now that we have got the credentials, let’s SSH into the machine.

But wait, that’s not happening. It says invalid password while logging in with SSH. Now what? Remember we have a form on port 80. Let’s try there.

Yes, we can login with the credentials on the webpage. We get a command execution input. Try to see whats there is different folders. List the users in home directory.
```shell
ls /home
```

We indeed have charlie as the user.
```shell
ls /home/charlie
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/sshkey.png" | relative_url}})
<br>

After listing the contents of the /home/charlie directory, we can see that there’s user.txt file which contains user flag. Also there are two more files, we if you know already, are very similar to SSH keys(id_rsa, id_rsa.pub) only with name “teleport”. Read the contents of “teleport” file using cat command.
```shell
cat teleport
```

Yes, its a SSH private key. Good for us. Copy the content and paste it in a text editor and bring the content in usual SSH key format as shown below. Save the file with any name you want. I will save it as id_rsa.

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/id_rsa.png" | relative_url}})
<br>

## Gaining Foothold:

Change the permissions of the key.
```shell
chmod 600 id_rsa
```

Try logging in now with user charlie.
```shell
ssh -i id_rsa charlie@10.10.10.10
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/sshlogin.png" | relative_url}})
<br>

Yes, this time we get the SSH shell.

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/userflag.png" | relative_url}})
<br>

Read the user flag.

## Privilege Escalation:

Now, its time to escalate privileges and become root.

Let’s see if the user has any sudo privileges. Use command
```shell
sudo -l
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/sudol.png" | relative_url}})
<br>

Yes, charlie can execute **/usr/bin/vi** using sudo.

Whenever there is a Sudo or SUID binary to be executed to escalate privileges, there is one place where you can find everything. Go to [GTFOBins](https://gtfobins.github.io/) and search ‘vi’ and select SUDO. You will get the command which will get you the root shell.
```shell
sudo vi -c ‘:!/bin/sh’ /dev/null
```

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/privesc.png" | relative_url}})
<br>

Go to root directory. You will see a file called root.py instead of root.txt. Very unusual right? Let’s see what cooking up.

Run the .py file
```shell
python root.py
```

It will ask you to enter a key. You remember we had got a key while enumerating? Yes, that’s the key. Enter the key. Make sure the key you enter is in format **b'\<key\>'**. Otherwise, it will give you an error.

<br>
![Image]({{ "/assets/images/thm/chocolatefactory/rootflag.png" | relative_url}})
<br>

It prints out a cool banner and root flag at the end.

That’s it for this really exciting room.
## Thankyou!!

Do follow on the socials below to stay connected for upcoming Blogs/Walkthroughs/WriteUps.