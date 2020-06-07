---
title: Uploading Files to Cheaha from the Command Line
author: ~
date: '2020-06-06'
slug: uploading-files-to-cheaha-from-the-command-line
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2020-06-06T23:48:24-05:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

I constantly work from the command line, and I find that I'm less productive whenever I need to leave it to use a different application. For uploading and downloading files to Cheaha, I typically use Filezilla since it has a pretty seemless interface that walks you through everything you might need to connect to a specific server. However, being able to do this while still at the command line has saved me a ton of time. 

For those who use Macs, here is how you do it. 

First install [Homebrew](https://brew.sh) if don't already have it installed since we're going to use it to install `curl-openssl`. The native `curl` that comes with Mac does not support `sftp` connections, which is what we need in order to connect securely to *Cheaha*. 

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

Next, install `curl-openssl`. 

```bash
brew install curl-openssl
```

I didn't want this installation to interfere with the native `curl` and wanted to instead use a separate command for using it. To do this, we need to create an `alias` which will point to binary specific to `curl-openssl`, which for me was `/usr/local/opt/curl-openssl/bin/curl`. 

```bash
echo "alias curl-ssl='/usr/local/opt/curl-openssl/bin/curl'" >> ~/.bash_profile
source ~/.bash_profile
```

Now you're ready to use it! To use most of these commands, you'll need to supply your username and password using the `--user "USERNAME:PASSWORD"` flag and the address needs to be your username followed by **@cheaha.rc.uab.edu/**.  

To retrieve the directory listing for your home folder and to subsequently test if its working,


```bash
curl-ssl -k  sftp://USERNAME@cheaha.rc.uab.edu/home/USERNAME/ --user "USERNAME:PASSWORD"
```

Lets first create a file for us to practice with. 

```bash
# creates a file named practice.txt
touch practice.txt

# writes text to the file
echo "Lorem ipsum dolor sit amet, consectetur adipiscing elit." >> practice.txt

# to look at the file we made
cat practice.txt
```


### Uploading a file

```bash
curl-ssl -T practice.txt sftp://USERNAME@cheaha.rc.uab.edu/home/USERNAME/practice-to-cheha.txt --user "USERNAME:PASSWORD"
```


### Downloading a file

```bash
curl-ssl -o /PATH-TO-FILE/FILENAME sftp://USERNAME@cheaha.rc.uab.edu/CHEAHA-PATH/FILENAME --user "USERNAME:PASSWORD"
```


### Uploading Entire Directory

```bash
find mydir -type f -exec /usr/local/opt/curl-openssl/bin/curl --user "USERNAME:PASSWORD" --ftp-create-dirs -T {} sftp://USERNAME@cheaha.rc.uab.edu/CHEAHA-PATH/{} \;
```

I specify the specify the path to `curl-ssl` because it through an error without it. 





