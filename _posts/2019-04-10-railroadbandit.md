---
excerpt: Reproducution and demonistration of the vulnerablity
layout: post
date: 2019-04-7
comments: false
title: RailRoadBandit CVE-2019-5418
tags:
  - ruby
  - CVE
  - demo
image: /images/ruby_on_rails.png
category: writeup
---

# 2019-04-10-RailRoadBandit

## Intro:

There is a File Content Disclosure vulnerability in Action View affecting Rails versions &lt;5.2.2.1, &lt;5.1.6.2, &lt;5.0.7.2, &lt;4.2.11.1. Specially crafted accept headers in combination with calls to '''render file: '''can cause arbitrary files on the target server to be rendered, disclosing the file contents. The impact is limited to calls to '''render''' which render file contents without a specified accept format.

## What is Rails?

Ruby on Rails, or Rails, is a server-side web application framework written in Ruby that uses the model–view–controller \(MVC\) structure pattern, providing default structures for a database, a web service, and web pages. ![](https://github.com/Bad3r/Bad3r-blog/tree/501930b64c6ef02b40e53942547eaa8a7be319a6/_posts/%7B%7B)

Rails is used to build full web applications like \(Basecamp, GitHub, Shopify, Airbnb, Twitch, SoundCloud, Hulu, Zendesk, Square, Cookpad.\)

### Path Traversal:

This vulnerability is a path traversal vulnerability \([OWASP Top 10: A5:2017-Broken Access Control](https://www.owasp.org/index.php/Top_10-2017_A5-Broken_Access_Control)\) A path traversal attack \(also known as directory traversal\) aims to access files and directories that are stored outside the web root folder. By manipulating variables that reference files with “dot-dot-slash \(../\)” sequences and its variations or by using absolute file paths, it may be possible to access arbitrary files and directories stored on file system including application source code or configuration and critical system files. It should be noted that access to files is limited by system operational access control \(such as in the case of locked or in-use files on the Microsoft Windows operating system\).

This attack is also known as “dot-dot-slash,” “directory traversal,” “directory climbing” and “backtracking.” source: [OWASP Path Traversal](https://www.owasp.org/index.php/Path_Traversal)

### reproducing:

To demonstrate the vulnerability, i bult a Ruby on Rails site: [bandit.bad3r.xyz](http://bandit.bad3r.xyz/) you can clone my site and run it locally by installing Ruby on Rails then cloning my repo [Bad3r/RailRoadBandit](https://github.com/Bad3r/RailroadBandit) Digital Ocean has an excellent guide to get you started make sure to install a vulnerable Rails \(e.g 5.2.1\):  [How To Install Ruby on Rails with rbenv on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-18-04) you can also create your demo manually by editing the default application controller in

```text
<web root folder>/app/controllers/application_controller.rb
```

open the file and add a call to "render file:"

```ruby
class ApplicationController < ActionController::Base
    def index
        render file:  "#{Rails.root}/public/bandit/bandit.html"
    end
end
```

here am just rendering an html file but you can also render any text fle.then edit the route file located in

```text
<web root folder>/config/routes.rb
```

to add a route for the controller:

```ruby
Rails.application.routes.draw do
    get 'application/index' => 'application#index'
    root 'application#index'
end
```

thats all you need to follow this demo. you should check if the file is rendered correctly and the route.rb config is correct.

### Exploiting the vulnerability

the script used in the demo is available in my Github repo [Bad3r/RailRoadBandit](https://github.com/Bad3r/RailroadBandit) run the script using the format:

```bash
┌ ~/RailroadBandit
└> % python3 Bandit.py http://bandit.bad3r.xyz/
```

then you will see a menu of builtin options to view some sensetive data:

```bash
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
```

you can use one of the builtin options or give the path for the file manually

```bash
../../../../../../../../../etc/passwd
```

here we use ../ for directory traversal, the result is: ![](https://github.com/Bad3r/Bad3r-blog/tree/501930b64c6ef02b40e53942547eaa8a7be319a6/_posts/%7B%7B)

am working on adding more features to the script like brute forcing the web root diretory name.

### Ruby on Rails sensitive files

there is some important files in ruby on rails that can also reveal sensitive information like:

| File | Description |
| :--- | :---: |
| /config/database.yml | May contain production credentials |
| /config/initializers/secret\_token.rb | Contains a secret used to hash session cookie |
| db/seeds.rb | May contain seed data including bootstrap admin user |
| /db/development.sqlite3 | May contain the SQL database |
| ---- |  |

### Patch

[https://github.com/rails/rails/commit/f4c70c2222180b8d9d924f00af0c7fd632e26715](https://github.com/rails/rails/commit/f4c70c2222180b8d9d924f00af0c7fd632e26715) ![](https://github.com/Bad3r/Bad3r-blog/tree/501930b64c6ef02b40e53942547eaa8a7be319a6/_posts/%7B%7B)

