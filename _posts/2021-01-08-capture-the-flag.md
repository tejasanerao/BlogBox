---
title:  "c4ptur3-th3-fl4g - TryHackMe"
style: fill
color: secondary
tags: [TryHackMe, Translation, Spectrograms, Steganography]
permalink: /blog/thm/capturetheflag
description: This is a really fun room by TryHackMe. The room was created by @dcdavidlee.
---
<br>
![Image]({{ "/assets/images/thm/capturetheflag/banner.png" | relative_url}})
<br>

This is a really fun room by TryHackMe. I enjoyed alot while trying to find the solutions. The room was created by [@dcdavidlee](https://tryhackme.com/p/dcdavidlee).

Link: [https://tryhackme.com/room/c4ptur3th3fl4g](https://tryhackme.com/room/c4ptur3th3fl4g)

**Note: In this walkthrough I won’t be giving out the answers to the questions directly. I will just tell you how to get to the answer. It’s fun to do the things on our own.**

> [CyberChef](https://gchq.github.io/CyberChef/) is the best place where you can find all the translation, encoding, decoding, etc. in one place. I used the same to get the answers. I suggest you to use the same.

<br>
<hr>

## TASK 1 - Translation & Shifting

<br>
Q1. c4n y0u c4p7u23 7h3 f149?

`This is the simplest of all the questions in the room. I strongly believe you can easily guess what it is trying to tell you.`

<br>
Q2. 01101100 01100101 01110100 01110011 00100000 01110100 01110010 01111001 00100000 01110011 01101111 01101101 01100101 00100000 01100010 01101001 01101110 01100001 01110010 01111001 00100000 01101111 01110101 01110100 00100001

`You can clearly tell it’s in Binary format. All you need to do is convert from Binary to ASCII(text).`

<br>
Q3. MJQXGZJTGIQGS4ZAON2XAZLSEBRW63LNN5XCA2LOEBBVIRRHOM======

`We normally come across these type of encoding and think that it’s similar to base64. But this one is in base32 format.`

<br>
Q4. RWFjaCBCYXNlNjQgZGlnaXQgcmVwcmVzZW50cyBleGFjdGx5IDYgYml0cyBvZiBkYXRhLg==

`This one is the pure base64 encoding.`

<br>
Q5. 68 65 78 61 64 65 63 69 6d 61 6c 20 6f 72 20 62 61 73 65 31 36 3f

`Combination of digits and letters in a pair. Its in Hexadecimal format.`

<br>
Q6. Ebgngr zr 13 cynprf!

`If you have often come across cryptography or read about it, you will guess it quickly. But also there is a hint given in the question itself. It’s ROT13, rotation-based cipher.`

<br>
Q7. *@F DA:? >6 C:89E C@F?5 323J C:89E C@F?5 Wcf E:>6DX

`This contains more than just letters. There are digits & symbols too. If you know about ROT13 then there are high chances you know about this one. It’s ROT47. ROT47 uses larger set of characters and rotates 47 times to create a cipher.`

<br>
Q8.

\- . .-.. . -.-. --- -- -- ..- -. .. -.-. .- - .. --- -.

. -. -.-. --- -.. .. -. --.

`It’s a Morse code. You will often come across this if you play CTFs challenges.`

<br>
Q9. 85 110 112 97 99 107 32 116 104 105 115 32 66 67 68

`Don’t confuse this one with the Hexadecimal format. You might have heard about ASCII tables right? Its contains 0–127 decimal numbers with their equivalent characters or digits or symbols. You have to convert this from Decimal to ASCII characters. Use the above CyberChef link that I have provided.`

<br>
Q10. Its a long question😂

`This ate my brain just because I missed the ‘=’ sign at the end of the string. The question extend to the right of window and I had to scroll to the right in order to get the whole string. At first I missed the last ‘=’ sign and kept scratching my head. You don’t do the same mistake if it’s the same case with you. In this multiple ciphers are used. You have to keep decoding the result of previous cipher till you get your answer. Base64 → MorseCode → Binary → ROT47 → Decimal(BCD) → Your Answer.`

<br>
<hr>

## TASK 2 - Spectrograms

`You need to download the file provided by TryHackMe. Its an audio WAV file. If you are curious to know about Spectrograms you can google about it. To solve the puzzle you need to decode the Spectrogram(Audio file). Go to this link below and upload the audio file and you will get your answer represented on the graph.`

Link: [https://academo.org/demos/spectrum-analyzer/](https://academo.org/demos/spectrum-analyzer/)

<br>
<hr>

## TASK 3 - Steganography

`If you want to know more about Steganography, visit this link. To get the answer you need have have ‘steghide’ installed in your machine. If you are using linux just ‘sudo apt-get install steghide’. Steghide allows us to extract any hidden data in a file. Use "steghide --info <filename>" to see if there is anything hidden in the file.`

<br>
![Image]({{ "/assets/images/thm/capturetheflag/steghideinfo.png" | relative_url}})
<br>

`There is a text file hidden in the image. Just press enter for passphrase. Now to extract the file use "steghide extract -sf <filename>" command. Read the extracted text file and you will have your answer.`

<br>
![Image]({{ "/assets/images/thm/capturetheflag/steganoAns.png" | relative_url}})
<br>

<br>
<hr>

## TASK 4 - Security through obscurity

`This was a bit tricky. You won’t get your answer with steghide. There is one more tool which can extract data from image file - "binwalk". I tried this and got the first answer. Use "binwalk -e <filename>" command to extract the data from image.`

<br>
![Image]({{ "/assets/images/thm/capturetheflag/binwalk.png" | relative_url}})
<br>

`Go to the extracted folder and you will get the answer of first question(file.extension). After this I tried various things to find the second answer but they were unsuccessful. Finally I tried the 'strings' command on image file which we got after extracting. 'strings' command is used to read human readable text from binary content. Use "strings <filename>" to and the last line of the result is your second answer.`

<br>
![Image]({{ "/assets/images/thm/capturetheflag/strings.png" | relative_url}})
<br>

That’s it for the room. It was fun doing this…
## Thank you!!

Do follow me on socials below for stay connected for these kind of writeups/walkthroughs/blogs.
