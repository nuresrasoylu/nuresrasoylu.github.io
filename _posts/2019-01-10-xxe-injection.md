---
layout: post
title:  "XXE Injection"
categories: "xxe "
description: "XXE Injection"
---

XML, the eXtensible Markum Language, is a mark language designed to standardize data communication between data exchange systems and platforms using the Internet.

The XML Injection vulnerability is triggered by invoking the specially defined entity while parsing the XML data. Parsing the XML data is the process of making the data understandable by the applications. Exploit of weakness the vulnerability, the XML library must allow the entity to define and invoke an entity, while in some libraries this property is explicit by default. Using this vulnerability, we can read files containing sensitive information.

Example 1:

![XXE Injection](../assets/images/2019-01-10/01.png)

We have a simple Login page as above. I started the review using the Burp Suite.

![XXE Injection](../assets/images/2019-01-10/02.png)

I’ve tried to send a “test” between the tags, and it turns me into a hash encoded with base64 as the X-Auth-Policy (Authentication Policy).

When I decode this hash:

![XXE Injection](../assets/images/2019-01-10/03.png)

After decoding Hash, we see plain-text de storage: secret.txt, path: / main directory, principal: token (The contents of the secret.txt file are token)

![XXE Injection](../assets/images/2019-01-10/04.png)

When we go to the secret.txt file in the main directory, “Congratulations, You Hacked Me !!!” We see the text.

Example 2:

![XXE Injection](../assets/images/2019-01-10/05.png)

The first thing we will do is log in to the burp suite by clicking on the forgot pwd, which is the part of the weakness from the login page.

![XXE Injection](../assets/images/2019-01-10/06.png)

At the `<!DOCTYPE Test [<!ENTITY test “EsraNSoylu”>]>` payload we determine the experiment variable and we want to print a string named EsraNSoylu. We inject our payload, and when we look at Response, we see that the XML parser parses what we send and suppresses the EsraNSoylu string. This indicates that the XXE injection deficiency exists and no measures have been taken.

![XXE Injection](../assets/images/2019-01-10/07.png)

How to protect us from this vulnerability ?

- The safest way is to completely deactivate DTD (Document Type Definition) in Java with similar method depending on the parser.
- In PHP you can disable libxml extensions.