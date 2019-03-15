---
layout: post
title:  "ae27ff wargame challenges 1-5 sloutions"
date:   2019-04-14
excerpt: "a writeup of how to solve ae27ff wargame challenges 6-10"
image: "/images/2019-3-15-4-46-23.jpg"
comments: true
---

## ae27ff  wargame challenges 6-10 sloutions


<p>Decided to write the sloutions i used to solve <a href="http://ae27ff.meme.tips/">ae27ff</a> wargame challenges.</p>
>  <a href="http://ae27ff.meme.tips/">ae27ff</a> is a set of levels that simulate challenges and puzzles that one may encounter during an ARG (Alternate Reality Game) including simple ciphers, steganography, different types of encodings, and familiarity with internet resources.
> Each level consists of some text, images, data, or files that is intended to lead you to the next page with some amount of investigation.
<br><br>

### #6 Defeated by brutes![Crypto]:

> Hvs doggkcfr wg lobori

From the challenge name this is a crypto challenge, the given string looks like it was encoded with an <a href="https://en.wikipedia.org/wiki/ROT13">ROT cipher</a> so first i googled it and found <a href="http://hot-cross--puns.tumblr.com/post/132751979982/parsing-code-caesarshift-key-12">a post on tumblr</a> where someone decrypted a string that contains the word "doggkcfr" with ROT12 therefore i decided to try to decrypt my string using ROT12 with href="https://gchq.github.io/CyberChef/">CyperChef</a> and got:
~~~
Input:  Hvs doggkcfr wg lobori
Recipe: ROT12
Output: The password is xanadu
~~~
to level 7!.

### #7 xanadu [Crypto]:

> Qhr nhrq lrvhf fs uoulfbye. Ln oenlos fs. Eedfiy. V mhuk... tuaw'm qhr pdmpwbrg: blreiefb
> 
> The key has already been given to you.

Another crypto challenge I started with looking up the title "xanadu" and found the Wikipedia page on <a href="https://en.wikipedia.org/wiki/Project_Xanadu">project Xanadu</a>:
> Project Xanadu was the first hypertext project, founded in 1960 by Ted Nelson. Administrators of Project Xanadu have declared it an improvement over the World Wide Web, with mission statement: "Today's popular software simulates paper. The World Wide Web (another imitation of paper) trivialises our original hypertext model with one-way ever-breaking links and no management of version or contents."[1]
> 
> Wired magazine published an article called "The Curse of Xanadu", calling Project Xanadu "the longest-running vaporware story in the history of the computer industry".[2] The first attempt at implementation began in 1960, but it was not until 1998 that an incomplete implementation was released. A version described as "a working deliverable", OpenXanadu, was made available in 2014. 

While it was a fun read, it didn't help much though maybe there is a reference am missing. The given string seems to be an encrypted alphabetic text similar to Caesar cipher, but there are a password and it's given to us. At first, I thought the password was "blreiefb" because of the common format "text/email: password" and ended up wasting time then decided to make sure what kind of encryption is used and a found a handy tool called <a href="https://github.com/nccgroup/featherduster">FeatherDuster</a>:
> FeatherDuster is a tool for breaking crypto; It tries to make the process of identifying and exploiting weak cryptosystems as easy as possible. Cryptanalib is the moving parts behind FeatherDuster, and can be used independently of FeatherDuster.

 and decided to give it a try:
~~~
$: featherduster xanadu.txt                                                      (master|…) 6:38:34
Welcome to FeatherDuster!

To get started, use 'import' to load samples.
Then, use 'analyze' to analyze/decode samples and get attack recommendations.
Next, run the 'use' command to select an attack module.
Finally, use 'run' to run the attack and see its output.

For a command reference, press Enter on a blank line.


FeatherDuster> analyze 
[+] Analyzing samples...
[+] Messages may be encrypted with a stream cipher or simple XOR.
[!] Individual messages have failed statistical tests for randomness.
[!] This suggests weak crypto is in use.
[!] Consider running single-byte or multi-byte XOR solvers.

[+] Suggested modules:
   alpha_shift          - A brute force attack against an alphabetic shift cipher. 
   base_n_solver        - A solver for silly base-N encoding obfuscation.          
   single_byte_xor      - A brute force attack against single-byte XOR encrypted ciphertext.
   multi_byte_xor       - A brute force attack against multi-byte XOR encrypted ciphertext.
   many_time_pad        - A statistical attack against keystream reuse in various stream ciphers, and the one-time pad.
   vigenere             - A module to break vigenere ciphers using index of coincidence for key length detection and frequency analysis.
~~~ 
I did not really need to use the tool but, it confirmed that the encryption used is <a href="https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher">Vigenère cipher</a> so I went back to <a href="https://gchq.github.io/CyberChef/">CyperChef</a> and used <a href="https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher">Vigenère Decode</a> and tried with the passphrase "blreiefb" but it was wrong my second guess was the name of the challenge "xanadu":
~~~
Input: Qhr nhrq lrvhf fs uoulfbye. Ln oenlos fs. Eedfiy. V mhuk... tuaw'm qhr pdmpwbrg: blreiefb
Recipe: Vigenère Decode, Key = "xanadu"
Output: The next level is horrible. It really is. Really. I mean... that's the password: horrible
~~~

found the password for the next level "horrible" bring it on.

### #8 DELiberation:

> Find the number of bits required to represent one character in the original ASCII encoding...
> Multiply it by 1702

1 char is stored in 1 byte = 8 bits. the US-ASCII uses 7 bits per character and the 8th bit is set to 0.
quick math in the terminal using bc:
~~~ bash
$: man bc
NAME:
       bc - An arbitrary precision calculator language
~~~

~~~ bash
$: bc
7 * 1702
11914
~~~

the password for the next level is "11914".


### #4 Popular with spiders:

> 155 141 156 151 164 157 142 141
> 
> ...if they counted with their legs

The hint is about legs, and the title of the challenge is "Popular with spiders" a spider has 8 legs the equivalent in computing is an octet because it has 8 bits
those numbers separated by a space are the octal values in the <a href="https://www.ascii-code.com/">ASCII table</a> where "155" = "m" and so on.
Instead of doing it manually I used <a href="https://gchq.github.io/CyberChef/">CyperChef</a> to convert from octal to text the output was "manitoba" and now I can move on to the next level.


### #5 Invested Capital:

> We've been getting swamped with email spam this week along with another batch of "Nigerian Prince" letters, however the boys in the IT office are telling me we should take a second look at this email in particular.
> Let me know if you find anything interesting.
> >Pardon me, Admirable colleague,
> >
> >Suppose that you had enough money for life, Some might call me crazy - Who is to say whether that is actually the case but you yourself? on the Other hand
> >i am ready to make you a substantial *offer*. Regarding this offer, you only need to call 555-121-1709 and arrange a one-time payment and in 6-8 months
> >receive a substantial payout that will last you a lifetime. Don't you think it is time to fight for yourself? It is! So don't sit there sorry for yourself, call now!
> >
> >This offer is only good for a limited time.  All of the time people throw out one of the best offers - Little do they know what they are missing; Because it
> >looks like spam. Unless you're already rich, you'd be crazy not to take this offer!
> >
> >Thank you for your time.
> 
> (Do not call this phone number - it is fake)


I wasn't sure what to do with this one, but the title sent me in the right direction. The tile is "Invested Capital" made me think it might be related to the capitalized words in the given email, so I decided its time to practice some python and write a simple script that takes in the text and returns to me the words that start with a capital letter:

~~~ python
#! /usr/bin/python3
# script that prompts for a file  and return the words that start with 
# a capital letter in the given file.

def processString(str):

    words = str.split()
    lst = []
    output = ""
    for word in words:
        if word[0].isupper():
            lst.append(word)
            output += word[0]
    print("list of words that start with a capital letter:")
    print(lst)

    print("word combination:")
    print(output)

def main():
    print ("Enter a file name:")
    file = open('' + input().strip(), 'r')
    str = file.read()
    processString(str)
    file.close()



main()
~~~

I saved the email into a text file and ran the scipt:
~~~
$: python3 ./capital-letter-detect.py
Enter file name:
email.txt
list of words that start with a capital letter:
['Pardon', 'Admirable', 'Suppose', 'Some', 'Who', 'Other', 'Regarding', "Don't", 'It', 'So', 'This', 'All', 'Little', 'Because', 'Unless', 'Thank']
word combination:
PASSWORDISTALBUT
~~~
the script returns at the end the combination of those capitalized letters "PASSWORDISTALBUT", the password is "talbut".

It was fun solving those challenges, and I might keep going and write another post for higher level challenges.