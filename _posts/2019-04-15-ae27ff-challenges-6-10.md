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

### #8 Last-minute calculations [Math,Unknown]:

> It seems that one of our developers has been helping candidates cheat on the test; seems it was a mistake to transfer him from Finance. We found the following note being passed, but we don't know what it means.
> 
> Maybe you could figure it out for us?
<span class="image fit"><img src="{{ "/images/horrible.jpg" | absolute_url }}" alt="" /></span>

This is a math problem so I started by using bc:
~~~
┌ ~
└> % bc                                                                                       8:54:42
bc 1.07.1
Copyright 1991-1994, 1997, 1998, 2000, 2004, 2006, 2008, 2012-2017 Free Software Foundation, Inc.
This is free software with ABSOLUTELY NO WARRANTY.
For details type `warranty'. 
(2 * 3922138 - 18368) / 1101
7108
~~~
the calculated number is 7108 there is a hint in the picture that says “flip!” because this is a math problem, and the output is an integer I remembered how you could spell words in a calculator, but I didn’t have one next to me, so I wrote a python script that takes in a number and gets the letters equivalent to the digits and flips it:
~~~ python
#! /usr/bin/python3
'''
It's known that when digital digits are looked at upside down they resemble an English letter.
this script given a number it will convert it to English letters using the calculation:
Number  	 Letter
0       	 O/D
1       	 l
2       	 Z
3       	 E
4       	 h
5       	 S
6       	 g
7       	 L
8       	 B
9       	 G
example : 
    number  = 77345
    letters = LLESH
    word    = SHELL
'''

def getLetters(st):
    dict = {0: ')O/D(', 1: 'I', 2: 'Z', 3: 'E', 4: 'H', 5: 'S', 6: 'G', 7: 'L', 8: 'B', 9: 'G'}

    letters = ""
    for num in map(int, st):
        letters += dict[num]

    return letters

def main():
    print("Enter a number:")
    letters = getLetters(input().strip())

    print("word = " + letters[::-1].lower())


if __name__ == '__main__':
    main()
~~~

then I ran the script:
~~~ bash
┌ ~
└> % python3 ./numToStringCalculator.py                                                       9:00:33
Enter a number:
7108
word = b(d/o)il
~~~

The output is "b(d/o)il" the 2nd letter could be either 'd' or 'o' if it's 'o' the word would be "boil" tested it out and that was the password.


### #9 Darkest darkness [Steg]:

> <span class="image fit"><img src="{{ "/images/boil.jpg" | absolute_url }}" alt="" /></span>

All you get for this challenge is a black picture, and the title is "Darkest darkness" made me think that all I have to do is reset the color table; therefore, I opened Gimp and loaded the picture after that from the menu at the top:
Colors --> Auto --> equalize
and a word showed up on the screen "rosebud" and what do you know its the password for the next level :).
Note that there is other ways to solve this you can retrieve the message by restoring the color table in a graphics editor like Gimp (Colors → Map → Set Color Map) or by editing the data directly (change bytes 13, 14 and 15 of the file from 000000 to ffffff).

<span class="image fit"><img src="{{ "/images/boil-solved.jpg" | absolute_url }}" alt="" /></span>
