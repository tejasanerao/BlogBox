---
layout: post
title:  "eJPT/PTS Review - An initial boost"
date:   2021-01-24 10:12:30 +0530
categories: Exam Blog
---
In this article, I will be sharing my experience and review for the eJPT course and exam. Hopefully, it will help you in approaching the exam.

![]({{ "/assets/images/eJPT/review.png" | relative_url}})

## <b>About Me</b>

Hello, My name is Tejas Anerao and it’s been more than a year(at the time of writing this) since I entered this field of CyberSec. I started with gaining the knowledge which is required as the foundational base for this field i.e Networking and then slowly and simultaneously I started studying about the Linux and Windows Operating systems. The most important quality you must have in order to flourish in this field is the curiosity to learn new things everyday and to know how and why the things work. In order to break into something you should know how they are built. Not in extreme deep manner, but enough to understand what you are working with.

I passed my eJPT on January 22, 2021. Before starting eJPT course material provide by INE(Starter Pass), I had enough experience of hacking boxes(not enough root all the Medium to Hard level Boxes but had pretty good confidence with Easy to few Medium boxes) from different platforms like Vulnhub, HackTheBox and TryHackMe. But eJPT is slightly different. You will learn things in this course which you didn’t use in HTB or THM or CTF boxes. eJPT will give you a glimpse(basic level) of how a real world Penetration Test scenario looks like.

So, I will divide this review in following parts:
* Course
* Labs
* Exam
* Tips
* Verdict

&nbsp;
> <p style="color: #aaaaaa">While sharing my experience and review of the certification, I will make sure I don’t give away any spoiler related to exam which can be against the terms and conditions of eLearnSecurity.</p>

&nbsp;

## <b>Course</b>

The eJPT/PTS course material is now being included in the Starter Pass of INE which is free. You get unlimited lab time + all the PPTs + video lessons. The PTS course is divided in three sections.

### Penetration Testing Prerequisites:

This section contains different modules like Networking and Web application fundamentals. This is section is as important as the Hacking section. Especially the networking module. You must study and understand the network protocols, use of wireshark(Analyzing traffic), routing(very important), different network devices, etc. I suggest you to take the Networking module very seriously as it has a great influence on the exam. Also study the web application module as it will help in the last section of course.

### Penetration Testing: Preliminary Skills & Programming

This section teaches you basic programming of C++ and python + you will learn basic command line scripting in Linux and Windows. I had a good coding background and a good experience in programming using C++ and python. I also knew about Bash and windows cmd. So, I didn’t actually study this section. But if you are new to programming, then I suggest you to go through the lessons. Note that programming is not a part of exam. You won’t need to code your own exploits. But it will be good for you to know enough programming to have basic understanding of any code you encounter. The labs in this section contains heavy coding, so its okay if you skip it. eCPPT might need a great programming knowledge. eJPT doesn’t. Study the Command Line module. That is helpful.

### Penetration Testing Basics

This is the section where you will learn actually exploitation. This is the section which you have to study thoroughly. You are going to use most(if not all) of the techniques and attacks from this module in the exam. You need make a strong methodology for yourself. Make notes of all the tools and commands you will use. Its always good to have them in one place. Study all the PPTs and Videos. Don’t miss anything. Its always good to know. Think of it as learning something new and not just for passing the exam. If you feel the material is not enough, you have google for your rescue. You can watch videos on youtube too for a particular topic. Gather as much resources as you can and study them. Think from long term perspective.

## <b>Labs</b>

Labs are the best part of INE’s course material. There are multiple lab assignments for different modules. Whatever you learn in PPTs and videos, you will get to apply it in the labs. I suggest you to do each lab atleast twice. And make notes of how you solved the labs. If you get stuck, you can refer the solutions. And even you were able to solve it on your own, have a look at the solution to see if any different method is used to solve it.

### Black Box Penetration Labs:

You get 3 black box penetration labs at the end of the course material. And this is every all your accumulated knowledge will be used. To be honest, these labs just surprised me. These were lot better and complex than I expected. This is where you will get to know the difference between an Internal Penetration Testing and CTFs(HTB, THM boxes). I got stuck multiple times and I had to refer the solution. When you complete these, make a note of all the new methods or tools you encountered. Do the labs twice or thrice just to make your methodology strong.

## <b>Exam</b>

Starting your exam is really simple. You just have to click on “Begin Certification Process” button and you will be provided with a “Letter of Engagement” which is very important. It will tell you about the exam scenario and few more things. After you completely read and understand it, start the lab and download the VPN. Make sure you have assigned the username and password for the VPN file from your dashboard before starting the Certification process.

The time allotted for the exam is 3 days(72 hrs) which is more than enough. You will have 20 questions(MCQs) to answer which will be totally based on the practical you do. You need to answer at least 15 questions(75%) to pass the exam.

I started my exam at 00:00 am on 22nd January, 2021. I was able to answer 17 questions till 6:00 am. I slept at 6:00 am and woke up again at 9:30 am to carry forward the testing. Again took a break at 11:00 am and came back at 12:30 pm. Finally at 1:00 pm I got the answers of remaining questions. After rechecking all the answers I submitted the exam at 1:15 pm. Got a perfect score💯.

Now let’s talk about the questions. At first you will feel some questions quite confusing. I felt the same at the start. But later everything started making sense. If your basics are clear, you will know the answers of them. Some questions will test your patience. And yes patience is the key. Keep your mind cool and fresh. Take breaks time to time. Don’t be in hurry to give the exam in as less time as possible. You will have a timer on dashboard. I suggest you note down the questions in a file and enter the answers in the file as soon as you find them.

![]({{ "/assets/images/eJPT/score.png" | relative_url}})

Once you submit the exam, you will get the score and a beautiful certificate immediately.

## <b>Tips</b>

In this section I will give you some tips that I personally followed and they really helped me throughout the exam.

* <b>Networking</b>: Don’t ignore this module. Without this module you won’t be proceed in the exam or give answers to the questions. You must know how to route or pivot in an internal network. Get to know the network devices and network protocols.
* You don’t need to know much C++ or python but its always good to know the basics in order to understand any code which you encounter. You won’t need to code your own exploits. But complete the Command line scripting or Windows commands module.
* <b>Google</b>: You are going to need google a lot. You need to know how to google for any specific vulnerability or exploit. Keep a habit of reading articles explaining the exploits.
* Learn using <b>Nmap</b>(with NSE), <b>Nessus</b>(you can use it if you want), <b>Metasploit</b>, <b>SQLMap</b>, <b>Hydra</b>, <b>Dirbuster</b>, <b>John The Ripper</b>, <b>enum4linux</b>, <b>Wireshark</b>(Analyzing the traffic). All of these tools are your friends and you will need them.
* Don’t hesitate to <b>bruteforce</b>.
* <b>Enumerate</b>, <b>Enumerate</b>, <b>Enumerate</b>. You need to gather as much information as you can about the services. Carry out service version scan, OS scan and scan all the ports instead of popular ones.
* Everything in the exam is within the scope of course. Practice Black Box Penetration Testing Labs at least twice to build a methodology. Stick to your methodology and don’t get carried away.
* If you have any experience with HackTheBox, TryHackMe or Vulnhub boxes then it will surely help. Although it's not mandatory to have this experience.
* Keep in mind that this is an internal penetration testing. So, its not necessary that all machines are exploitable.
* Don’t be under a wrong influence of cracking the exam in 3–5 hrs if you have read experiences of any other candidate. It totally depends on how long you are into this field. Depending on your experience, you can crack it in 5 hrs or it can even take a day. It took me roughly around 8 hrs to complete it. There is a reason you are allotted 3 full days. eLearnSecurity wants you to know about real world penetration testing scenarios starting with the basics.

I have created a cheatsheet for eJPT exam. Its always good to have everything in one place. You can make your own if you want. But I will be sharing with you the one I managed to make.

<a href="https://github.com/tejasanerao/eJPT-Cheatsheet">Get the cheatsheet here.</a>

## <b>Verdict</b>

In my opinion, this is the best beginner course and exam. The fact that the course material and labs are free now, it even makes more sense going for eJPT. You need to pay $200 for the exam voucher. But trust me, it is totally worth it. You are not going to get a job with this Cert but what you will get is an initial boost in your confidence if you are looking forward OSCP or eCPPT. After looking at the eJPT exam, I wonder how good the exam scenario of eCPPT or eCPTX would be. Currently I am studying for OSCP. But I would love to experience more of eLearnSecurity in future after I start earning.

### Thankyou!!

Feel free to DM me on the socials mentioned below if you have any queries. Share this article with people you know if they are also looking forward for eJPT.
