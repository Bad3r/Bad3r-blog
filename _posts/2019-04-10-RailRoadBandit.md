---
layout: post
title:  "RailRoadBandit CVE-2019-5418"
date:   2019-04-7
excerpt: "Reproducution and demonistration of the vulnerablity"
image: "/images/ruby_on_rails.png"
category: writeup
tags: [ruby, CVE,demo]
comments: false
---

## test test ....
f


## Intro:
There is a File Content Disclosure vulnerability in Action View affecting Rails versions <5.2.2.1, <5.1.6.2, <5.0.7.2, <4.2.11.1.
Specially crafted accept headers in combination with calls to '''render file: '''can cause arbitrary files on the target server to be rendered, disclosing the file contents.
The impact is limited to calls to '''render''' which render file contents without a specified accept format. 

## What is Rails?
Ruby on Rails, or Rails, is a server-side web application framework written in Ruby that uses the model–view–controller (MVC) structure pattern, providing default structures for a database, a web service, and web pages. 
<span class="image fit"><img src="{{ "/images/MVC.png" | absolute_url }}" alt="" /></span>

Rails is used to build full web applications like (Basecamp, GitHub, Shopify, Airbnb, Twitch, SoundCloud, Hulu, Zendesk, Square, Cookpad.)

## Path Traversal:
This vulnerability is a path traversal vulnerability (<a href="https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control">OWASP Top 10: A5:2017-Broken Access Control</a>)
A path traversal attack (also known as directory traversal) aims to access files and directories that are stored outside the web root folder. By manipulating variables that reference files with “dot-dot-slash (../)” sequences and its variations or by using absolute file paths, it may be possible to access arbitrary files and directories stored on file system including application source code or configuration and critical system files. It should be noted that access to files is limited by system operational access control (such as in the case of locked or in-use files on the Microsoft Windows operating system).

This attack is also known as “dot-dot-slash,” “directory traversal,” “directory climbing” and “backtracking.” 
source: <a href="https://www.owasp.org/index.php/Path_Traversal">OWASP Path Traversal</a>)

## reproducing:
To demonstrate the vulnerability, i bult a Ruby on Rails site: ///url///
you can clone my site and run it locally by installing Ruby on Rails then cloning my repo:<a href="https://github.com/Bad3r/RailroadBandit">Bad3r/RailRoadBandit</a>
Digital Ocean has an excellent guide to get you started: <a href="https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-18-04">
How To Install Ruby on Rails with rbenv on Ubuntu 18.04</a>
you can also create your demo manually by creating a controller by excuting the command
~~~Bash
┌ ~
└> % railrs generate controller bandit
~~~
the controller will be genrated in 
> <web root folder>/app/controllers/
then edit the route file located git 
> <web root folder>/config/routes.rb
