---
layout: post
title:  "‘Natas’ Solutions 1-12"
categories: "natas, hacking "
description: "‘Natas’ Solutions 1-12"
---

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/01.png)

Natas teaches the basics of serverside web-security. 

## 1.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/02.png)

This was a simple question. We looked at the source code of the page.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/03.png)

## 2.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/04.png)

If right click is blocked , we use browser shortcuts to open the debugger. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/05.png)

## 3.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/06.png)

Okeyi Let’s look source code.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/07.png)

Source reveals a hidden image located at /files/pixel.png. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/08.png)

Navigating to /files/, we see the file /files/users.txt which contains the password. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/09.png)

## 4.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/10.png)

When we look source code, a comment in the source says: 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/11.png)

“[…] Not even Google will find it this time…”.
Google indexes the web, but honours a site’s `robots.txt` file, which tells crawlers not to visit web pages. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/12.png)

 The `robots.txt` excludes the contents of `/s3cr3t/`. Looking in this folder we find a file `user.txt` which contains the password. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/13.png)

## 5.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/14.png)

We can trick the server into thinking we’ve come from that URL by adding the Referer header to our HTTP request:
`“http://natas5.natas.labs.overthewire.org/”`

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/15.png)

## 6.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/16.png)

Inspecting the site, we see that the following cookie has been set:
`loggedin=0`

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/17.png)

Change this cookie to 1, and the password is returned.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/18.png)

## 7.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/19.png)

In the source code, we see an included file `/includes/secret.inc`.  

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/20.png)

Navigating to this page, we see that the secret is `FOEIUWGHFEEUHOFUOIU`. Enter this secret to get the password. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/21.png)

## 8.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/22.png)

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/23.png)

A clue in the source says that the password we’re looking for is stored at `/etc/natas_webpass/natas8` on the server. If we navigate to the Home or About page, we can change the value of page in the URL query to hit other files on disk.

The query `?page=../../../../etc/natas_webpass/natas8` reveals the password. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/24.png)

## 9.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/25.png)

 Looking at the source code, we see that the secret, when encoded must match:`3d3d516343746d4d6d6c315669563362`. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/26.png)

To find out the clear text secret, we can reverse the encoding steps: 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/27.png)

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/28.png)

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/28_2.png)

Submit this secret to see the password. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/29.png)

## 10.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/30.png)

Looking at the source code, we see that PHP passthru function.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/31.png)

In this level, user input is passed to the PHP passthru function:
`“passthru(“grep -i $key dictionary.txt”);”`
We can terminate the grep with a semicolon, run an arbitrary command, and comment any code that comes after with:
`; cat /etc/natas_webpass/natas10 #`

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/32.png)

## 11.

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/33.png)

This level is the same as level 10. 

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/34.png)

 But the characters ;, | and & are blocked by the server. We can utilise the grep to search for everything in the password file:

`.* /etc/natas_webpass/natas11 #`

![‘Natas’ Solutions 1-12](../assets/images/2019-05-06/35.png)
