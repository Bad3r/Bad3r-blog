---
layout: post
title:  "ae27ff challenges 1-5 sloutions"
date:   2019-04-14
excerpt: "a writeup of how to solve ae27ff challenges 1-5"
image: "/images/2019-3-14-16-14-51.jpg"
comments: true
---

## ae27ff challenges 1-5 sloutions


<p>Decided to write the sloutions i used to solve <a href="http://ae27ff.meme.tips/">ae27ff</a> challenges.</p>
>  <a href="http://ae27ff.meme.tips/">ae27ff</a> is a set of levels that simulate challenges and puzzles that one may encounter during an ARG (Alternate Reality Game) including simple ciphers, steganography, different types of encodings, and familiarity with internet resources.
> Each level consists of some text, images, data, or files that is intended to lead you to the next page with some amount of investigation.
<br><br>

### Challenge #1 Introductions:

> Welcome to the candidate screening process.  Throughout this test you will be confronted with puzzles that will test your competence in a wide variety of subjects.
> Your goal is to approach each puzzle as a means to acquire a password. You should then use this password in a certain way to get to the next page.  All passwords should be entered as lowercase unless otherwise instructed.
> To give you a head-start, the password for this screen is "start". The password for the next screen is "thr3e".  From that you should be able to Address the question of how to advance.
<p>The password for the next level is given "thr3e", this more to make sure you understand how to navigate the site.
To solve this challenge click on the site icon top left then in the "Mobile password entry:"<sup id="fnref:1"><a href="#fn:1" class="footnote">1</a></sup> field write the password "thr3e" to proceed to the next level.</p>

<div class="footnotes">
  <ol>
    <li id="fn:1">
      <p> note that the password field is case sensitive. <a href="#fnref:1" class="reversefootnote">&#8617;</a></p>
    </li>
  </ol>
</div>


### Challenge #2 Human Error:

> Apologies,
> It appears our developer entered the password for this level incorrectly.
> Maybe it was just a typo and the right screen still exists?

The password we got from level #1 was "thr3e" which is a typo for "three" I entered it and got to level #3.


### Challenge #3 DELiberation:

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


### Challenge #4 Popular with spiders:

> 155 141 156 151 164 157 142 141
> ...if they counted with their legs

The hint is about legs, and the title of the challenge is "Popular with spiders" a spider has 8 legs the equivalent in computing is an octet because it has 8 bits
those numbers separated by a space are the octal values in the <a href="https://www.ascii-code.com/">ASCII table</a> where "155" = "m" and so on.
Instead of doing it manually I used <a href="https://gchq.github.io/CyberChef/">CyperChef</a> to convert from octal to text the output was "manitoba" and now I can move on to the next level.


